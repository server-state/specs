# Server Module Function
A server module function (short: *SMF*) is a function defining the output of a
given module for a given set of options.

It can get used for [server modules](server-module.md) and must follow the
following signature:

```
(options?: *) => Promise<object | array | string | number | boolean> | object | array |
string | number | boolean
```

A SMF should be a `read-only` operation on the system (not changing any files), and

1. Return a JSON-serializable value on success
2. Throw (or, for asynchronous modules, reject with) an `Error` with a descriptive error message (`throw new Error('what went wrong')`)
3. **Not** log to the console or do other "active" operations. Handling error messages etc. is up to the SM's consumer (in most cases, [server-base](https://github.com/server-state/server-base)).

The above guidelines shall not be seen as strict rules, but exceptions should be well-justified (especially for official modules) to allow for consistency and ease of use.
