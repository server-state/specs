# CLI (@server-state/cli)


|Info|Value|
|---|---|
|Repository|https://github.com/server-state/cli|
|NPM Package|<!--[`@server-state/server-cli`](https://www.npmjs.com/package/@server-state/cli)-->*Coming soon*|
|Package version|n/a|
|CI Build|n/a|
|Issues|[GitHub Issues](https://github.com/server-state/cli/issues?q=is%3Aissue+is%3Aopen+sort%3Aupdated-desc)|

A Command-Line Interface for developing server-state modules (CBMs and SMs).
One can install it via `npm` (or `yarn`):

```shell
npm install -g @server-state/cli
# Or Yarn:
yarn add -g @server-state/cli

# Run it:
server-state <command>
```
 
or use it directly with `npx`:

 ```shell
npx @server-state/cli <command>
```
 
Installing it creates an executable in the `PATH` called `server-state`.

## General structure

[server-state CLI structure Component Diagram](cli-structure.puml ':include :type=code plantuml')

## Repositories
- [CLI](https://github.com/server-state/cli)
- [CBM Test Environment](https://github.com/server-state/cbm-test-environment)

## `server-state init`
Initializes a new CBM, SM or both.

## `server-state start [path-to-cbm]`
Start the CBM test environment for the CBM in `[path-to-cbm]` (CWD by default).

## `server-state build`
Build the CBM for production-use (or for publishing it to npm)

## `server-state test`
Run the jest tests defined in the CBM's `tests` folder.

## `server-state test-e2e`
Start the e2e tests using cypress. 
