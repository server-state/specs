# Authentication

Coming soon

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
User->"simple-server":GET endpoint \nwith token
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
