# Server Module Function
A server module function (short: *SMF*) is a function defining the output of a
given module for a given set of options.

It can get used for [server modules](server-module.md) and must follow the
following signature:

```
(options?: *) => Promise<object | array | string | number | boolean> | object | array |
string | number | boolean
```
