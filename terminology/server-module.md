# Server Module
A server module (or short: *SM*) is a [server module function](/terminology/server-module-function.md)
registered to the server-base under a specific  `name` with a specific set of
`options`.

All names must be distinctive for the given `ServerState` instance and may not
be `auth` or `all`. Modules can use the same [server module
function](/terminology/server-module-function.md) as long as their names are distinctive.

Names furthermore may include nothing but lower-case, URI-compatible characters and
should ideally match this regex:

```
[a-z][a-z0-9\-]+[a-z0-9]
```
