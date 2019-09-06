# Server Module
A server module is a [server module function](server-module-function.md)
registered to the server-base under a specific  name` with a specific set of
`options`.

All names must be distinctive for the given `ServerState` instance and may not
be `auth` or `all`. However, modules can use the same [server module
function](server-module-function.md) as long as their names are distinctive.
