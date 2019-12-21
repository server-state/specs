# raw-module
|Info|Value|
|---|---|
|Repository|https://github.com/server-state/raw-module|
|NPM Package|[`@server-state/raw-module`](https://www.npmjs.com/package/@server-state/raw-module)|
|Package version|[![npm version](https://badge.fury.io/js/%40server-state%2Fraw-module.svg)](https://www.npmjs.com/package/@server-state/raw-module)|
|CI Build|[![Build Status](https://travis-ci.com/server-state/raw-module.svg?branch=master)](https://travis-ci.com/server-state/raw-module)|
|Issues|[GitHub Issues](https://github.com/server-state/raw-module/issues?q=is%3Aissue+is%3Aopen+sort%3Aupdated-desc)|

## Abstract
A module that runs commands in the shell and returns the exit code(s) as well as the outputs from `stdout` and `stderr`.

[Structure Diagram](raw-module/structure.puml ':include :type=code plantuml')

## Input
### Type
```typescript
Array<string>
```

### Description
Commands that get executed in arbitrary order.

> [!WARNING]
> The order of execution of the commands is arbitrary and may also happen in parallel.
>
> Because of that, duplicate entries lead to an error.

Example:

```js
['ls', 'whoami', 'echo "Hello World"']
```

## Output
This is using

- [ ] A standard data format as defined in [Data Formats](/arch/data-formats.md)
- [x] A custom data format described below

### Custom data format specifications
Type:

```ts
{ 
    [command: string]: { 
        exitCode: number, 
        stdout: string,
        stderr: string 
    } 
}
```

Where the keys (`[command: string]`) are identical to the commands passed into the config.

### Specifications about the information in the return variable
The return object contains the results of the shell commands in a key-value-pair manner, where the key is equal to the command name passed in the config.

The results contain the `exitCode` of the command (on Linux systems, `0` means success) and the `stdout` and `stderr` outputs.

> [!NOTE]
> When `stdout` and `stderr` exist, the order of outputs cannot get determined by the output of the module.

#### Example
For the config

```js
['ls', 'whoami', 'echo "Hello World"']
```

the result might look something like this:

```json
{
  "ls": {
    "exitCode": 0,
        "stdout": "myfile",
        "stderr": ""  
    },
    "whoami": {
        "exitCode": 0,
        "stdout": "username",
        "stderr": ""
    },
    "echo \"Hello World\"": {
        "exitCode":  0,
        "stdout": "Hello World",
        "stderr": ""
    }
}
```

### Exceptions (if applicable)
#### No commands passed
If no commands get passed, an `Error` containing the following message gets thrown:
```
No commands specified for the "raw-module".
```

#### Duplicate Commands
If duplicate commands get passed, an `Error` containing the following message gets thrown:
```
Command already run: "[command]"
```

## Performance
The module performs the tasks in a runtime-complexity of

$$O(n)$$

where $n$ is the number of commands in the config array.
