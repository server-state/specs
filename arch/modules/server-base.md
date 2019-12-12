# @server-state/server-base

|Info|Value|
|---|---|
|Repository|https://github.com/server-state/server-base|
|NPM Package|[`@server-state/server-base`](https://www.npmjs.com/package/@server-state/server-base)|
|Package version|![Package Version Badge](https://camo.githubusercontent.com/a37713882a15380828c3498b8ad97653b8142e15/68747470733a2f2f62616467652e667572792e696f2f6a732f2534307365727665722d73746174652532467365727665722d626173652e737667)|
|CI Build|![CI Build Badge](https://camo.githubusercontent.com/873940dadf68b7a19f23be765015d20cc7071c15/68747470733a2f2f7472617669732d63692e636f6d2f7365727665722d73746174652f7365727665722d626173652e7376673f6272616e63683d6d6173746572)|
|Issues|[GitHub Issues](https://github.com/server-state/server-base/issues?q=is%3Aissue+is%3Aopen+sort%3Aupdated-desc)|

Server-side (NodeJS based) implementation of the server-base architecture (no modules included).

Responsible for creating the routes described in [API Routes](/api/server-base.md).

## General structure

```plantuml
@startuml
component "@server-state/server-base" as sb {
    class ServerBase {
        # logger: Logger
        # config: Config
        # modules: Object
        # args: Object
        # authorizedGroups: Object
        --
        + ServerBase(config: Config)
        ..
        + addModule(name: string, fn: function,\n authorizedGroups: string[], options: *): void
        + init(app: ExpressApp): void
    }

    class Logger {
        + log(...args): void
        + warn(...args): void
        + error(...args): void
        # _configure(config: Config): void
    }

    ServerBase *-- "1" Logger
    ServerBase ..> Logger: configures
    Logger --() stdout
}

together {
object "config: Config" as config {
    logToConsole: boolean
}

config --[hidden]>expressApp

object "expressApp: ExpressApp" as expressApp {
    get()
    post()
    [...]
}

    expressApp --() HTTP
}

config -> sb: passed into
sb <-- expressApp: passed into

sb <. ServerBase: export

ServerBase .> expressApp: registers paths
@enduml
```

## Usage Example
```shell
npm install @server-state/server-base
``` 

`index.js`:

```js
const express = require('express');
const SB = require('@server-state/server-base');

const app = express();

const serverBase = new SB({
    logToConsole: true
});

serverBase.addModule(
    'static', 
    () => 'Hello World', 
    ['guest']
);

serverBase.init(app);
app.listen(8080);
```
