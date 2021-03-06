# Authentication

## Authentication flow (diagram)
```plantuml
@startuml
actor User

== Authentication ==

activate User
User->"simple-server":request token
activate "simple-server"
"simple-server"->"Authentication service":Authenticate user
activate "Authentication service"
"simple-server"<--"Authentication service":Token for user
deactivate "Authentication service"
User<--"simple-server":Token
deactivate "simple-server"

== API Request ==
User->"simple-server":GET endpoint \n with token
"simple-server"->"server-base":forward request
activate "server-base"

"server-base"->"simple-server":isAuthorized()
"simple-server"->"Authentication service":Check authentication for token
activate "Authentication service"
"simple-server"<--"Authentication service":User status
deactivate "Authentication service"

"simple-server"->"simple-server":Check group permissions
"simple-server"-->"server-base":Authorized?
deactivate "simple-server"
"server-base"->"server-base":Call SMF
User<--"server-base":Response
@enduml
```

## Implementation inside `server-base`
- Inside the config, we pass an optional parameter called `isAuthorized`. It has the type signature `(req: ExpressRequest, authorizedGroups: string[]) => boolean` and is a function that, based on the request (which could include a token or other authentication data) and a list of authorized groups (e.g., `['admins', 'developers']` for one endpoint and `['admins', 'developers', 'project-management']` for another) determines whether the current user has the authorization to make that request. It returns `true` if the user has the authorization and `false` if he/she is not.
- When the above function returns `false`, no code may get executed and an `HTTP 403` response gets returned
- When we register no authorized groups for an endpoint, for security reasons, no authorization should get assumed in any case, meaning calling the method gets skipped.
- If `isAuthorized` is `undefined` or `null`, no authorization check gets performed and the authorization (i.e., `isAuthorized() === true`) gets assumed
- **Breaking Change:** `authorizedGroups: string[]` gets added as third argument for `addModule()`, moving `options: *` to fourth position. These groups get stored and passed into `isAuthorized` when the module gets executed.

## Groups and Permissions
A user belongs to $0..n$ groups. To which groups a user belongs gets determined by the Authentication service.

```plantuml
@startuml
class User {
    username:string
    passwordHash: string
    groups: Group[]
}
class Group {
    name: string
}

class APIEndpoint {
    path: string
    authorizedGroups: Group[]
    isAuthorized(group: Group)
}

Group "0..*" *-- User : belongs to <
Group "0..*" *-- APIEndpoint : has access to >
@enduml
```
### Group names
Group names can contain letters, numbers and dashes. Thus, they follow the following Regular Expression:

```regexp
[a-zA-Z0-9-]+
```

Group names are case-insensitive and get evaluated after getting transformed to lowercase, meaning `admins` is equal to `AdMiNS` to make configuration easier.

Typical examples of groups include:
- `admin`
- `devops`
- `developer`
- `user`
- `guest`

### Permissions and Endpoints
As specified above, an implicit permission system emerges from us passing the groups that may access the endpoint when creating the endpoint with

```js
serverBase.addModule('module-name', myFunc, [
    'admin',
    'developer'
], options);
```
Here, users who belong to the group *admin* or *developer* gain access to the endpoint *module-name*.

The `isAuthorized` callback function passed to the *server-base* has to determine whether the current user has access an endpoint with the specified groups that get granted access.

## Authentication Implementation in the *simple-server*
In the *simple-server*, a function for `isAuthorized` has to get passed to the *server-base* to use Authentication. This function then can make use of one of the auth modules provided in the *server-state* organization or custom code (or a combination thereof).

One example (using the JWT system) could look something like this:

```js
// [...]
const jwt = require('@server-state/auth-jwt-server');

jwt.setup({
    privateKey: privateKey,
    publicKey: publicKey,
    issuer: 'This server',
    db: {
        type: jwt.db.SQLITE3,
        location: './users.sqlite',
});

// [...]

let serverBase = new ServerBase({
    logToConsole: true,
    // [...]
    isAuthorized: (req, authorizedGroups) => {
        if (authorizedGroups.includes('guest'))
            return true;
        
        const currentUsersGroups = jwt.currentUsersGroups(req)
        
        // Intersections between currentUsersGroups and authorizedGroups:
        return array1.filter(value => array2.includes(value)).length > 0;
    }
});

serverBase.addModule('a', aSMF, ['admin'], {});
serverBase.addModule('b', aSMF, ['admin', 'guest'], {});

// [...]

serverBase.init(app)

// [...]

app.listen(8080);
```

This can differ for other auth modules, but the general method remains the same. We check whether the current user (parsed from the Express Request object `req`) is in a group that's included in `authorizedGroups`.

In the above example, we have a group called `guest`, which skips the check since modules accessible by this group need no authentication.

## Authentication Implementation in the *client-base*
In the *client-base*, standard protocols implemented for the *simple-server* get provided and may get activated/deactivated via the *client-base* options.

Supported protocols will include:

1. OAuth 2.0
2. OAuth 1.0
3. JWT
4. No authentication

For every server configured in the *client-base*, an Auth-server (or *No Authentication*) has to get selected in the options. This *Auth-Server* includes information about the protocol and URLs of this server. Two or more servers can use the same Auth-Server.

```plantuml
@startuml
class Dashboard

Widget "1..*" --* "1..*" Dashboard

class Widget {
    endpoint
    cbm
}

Widget "1..*" *-- "1" Endpoint

class Endpoint {
    url
    server
}

Endpoint "1..*" -- "1" Server

class Server {
    baseURL
    authServer
}

Server "1..*" *-- "1" AuthServer

interface AuthServer {
    authenticate(username, password): AuthHeader
}

AuthServer <|-- OAuth2Server
class OAuth2Server {
    clientSecret
    authURL
}

AuthServer <|-- JWTServer
class JWTServer {
    clientSecret
    authURL
}

AuthServer <|-- NoAuthServer
@enduml
```
