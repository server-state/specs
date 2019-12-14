<!-- Search and replace '[module-name]' in this file and replace it with the actual module name -->

# [module-name]


|Info|Value|
|---|---|
|Repository|https://github.com/server-state/[module-name]|
|NPM Package|[`@server-state/[module-name]`](https://www.npmjs.com/package/@server-state/[module-name])|
|Package version|[![npm version](https://badge.fury.io/js/%40server-state%2F[module-name].svg)](https://www.npmjs.com/package/@server-state/[module-name])|
|CI Build|[![Build Status](https://travis-ci.com/server-state/[module-name].svg?branch=master)](https://travis-ci.com/server-state/[module-name])|
|Issues|[GitHub Issues](https://github.com/server-state/[module-name]/issues?q=is%3Aissue+is%3Aopen+sort%3Aupdated-desc)|

## Abstract
A concise description of what the module does

<!-- Copy and rename the _sm-template folder and adjust the diagrams contents to fit you SM -->
[Structure Diagram](_sm-template/structure.puml ':include :type=code plantuml')

## Input

### Type

```typescript
Object<{someVar: string}>
```

### Description
Object with configuration. Values:

- `someVar: string`: Description of `someVar`

## Output
This is using

- [x] A standard data format as defined in [Data Formats](/arch/data-formats.md): [Data format]
- [ ] A custom data format described below

### Custom data format specifications (if applicable)

Type:

```ts
Object<{returnVar: string | number}>
```

### Specifications about the information in the return variable

[specifications]

### Exceptions (if applicable)

<!-- n/a -->
[description of cases, when return value differs from data format for specified reasons, e.g., errors]

## Performance
[description of the module's performance]

The module performs the tasks in a runtime-complexity of

$$O(n)$$

where $n$ is the number of chars in the `someVar: string` member of the `options` input. 
