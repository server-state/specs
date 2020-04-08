# server-base API Routes

API routes provided by [server-base](/arch/modules/server-base.md):

## `/api/v1/`
**Possible status codes:**
* `404` when the server is running

Base route. Will result in 404 or, if the API is setup.

## `/api/v1/all`
**Response type:** JSON

**Possible status codes:**
* `200` when everything works as expected
* `403` for unauthorized users
* `500` when the server has a wrongly configured module

Returns a JSON object with properties of all modules accessible by the authenticated user as a key-value pair with
the key being the name of the [SM](/terminology/server-module.md).

**Example 1**
```json
{
  "module1": { "a": "a", "b": "b" },
  "module2": "abc",
  "module3": [ "a", "b", "c" ]
}
```

## `/api/v1/[module-name]`
**Response type:** JSON

**Possible status codes:**
* `200` when everything works as expected
* `403` for unauthorized users
* `404` when the module isn't registered
* `500` when the module has falsy configuration or an error occurs during the SMF's execution

Returns evaluated value of the module specified by the `[module-name]` as JSON.

**Example 1**
`/api/v1/module2`

```json
"abc"
```

**Example 2**
`/api/v1/module3`

```json
[ "a", "b", "c" ]
```

## `/api/v1/[module-name]/permissions`
**Response type:** JSON

**Possible status codes:**
* `200` when everything works as expected
* `403` for unauthorized users
* `404` when the module isn't registered

Returns a JSON `string[]` containing the names of groups who have access to the endpoint `[module-name]`.

**Example 1**
`/api/v1/module2/permissions`

```json
["admin","developer","devops"]
```

**Example 2**
`/api/v1/module3/permissions`

```json
["admin","developer","devops","user","guest"]
```

## `/api/v1/[module-name]/sample`
**Response type:** JSON

**Possible status codes:**
* `200` when everything works as expected
* `404` when the module isn't registered

Returns an array of sample data for the SMF of this module.

> [!WARNING]
> This endpoint has to be accessible for unautenticated, too. Therefore, it may
> not expose any vulnerable information about the server.

**Example 1**
`/api/v1/module2/sample`

```json
["some string","some other string","this SMF only returns strings"]
```

**Example 2**
`/api/v1/module3/sample`

```json
[
  ["this SMF", "contains arrays"], 
  ["that only", "contain strings", "but can be empty"], 
  []
]
```
