# API Routes of [server-base](https://github.com/server-state/server-base)

## `/api/v1/`
**Possible status codes:**
* `403` for unauthorized users
* `404` when the server isn't running
* `500` when the server has a wrongly configured module

Base route. Will result in 404 or, if the API requires
authorization/authentication to use, 403 for unauthorized users.

## `/api/v1/all`
**Response type:** JSON

**Possible status codes:**
* `200` when everything works as expected
* `403` for unauthorized users
* `404` when the server isn't running
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
* `404` when the server isn't running
* `500` when the server has a wrongly configured module

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
* `403` for unauthenticated users
* `404` when the server isn't running
* `500` when the server has a wrongly configured module

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
