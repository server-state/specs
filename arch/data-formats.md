# Data Formats (*DFs*) for the project

Common Data Formats, that can get used to allow for using standard frontend modules, shall be listed below. Ideally, all 
these formats should include at least one implementation of a viewer in every Frontend implementation of *Server State*.

### Line / XY Data
Numeric data containing 2D-points that can get plotted in a graph are called XYData. It is specified as follows:

```ts
XYData = Array<{x: number, y: number, title?: string, color?: string | number}>
```

#### Required attributes
- `x`, `y`: The x- and y coordinates of the point

#### Optional attributes
- `title`: A label of the point. Can, e.g., get shown on mouse hover above a datapoint in a visualization
- `color`: The color of the point. This can either be a string containing the HEX value of a color or a number
  marking the index in an arbitrary color scheme.

#### Suggested visualizations
Line plots, bar plots, scatter plots, tables

### Hierarchical weighted data
Hierarchical weighted data describes a tree structure, where nodes contain `n` child nodes and where all nodes have the size 
of the sum of the sum of all child nodes, with leaf nodes having an explicit size.


```ts
HierarchicalData = { title?: string, color?: string | number, children?: Array<HierarchicalData>, size?: number }
```

#### Required attributes
n/a

#### Optional attributes
- `title`: A label of the point. Can, e.g., get shown on mouse hover above a datapoint in a visualization
- `color`: The color of the point. This can either be a string containing the HEX value of a color or a number
  marking the index in an arbitrary color scheme.
- `children`: The node's child nodes
- `size`: The leaf node's size. Can only get used for leaf nodes, i.e., where `children.length === 0`.

#### Suggested visualizations
Lists, tables, treemaps, sunburst charts

### Markdown
Raw Markdown that gets generated in the SM and rendered in the client

```ts
MarkdownData = string
```

#### Suggested visualizations
Markdown renderer, Raw text renderer for environments with no Rich Text support

### Text
Raw text that gets generated in the SM and rendered in the client

```ts
TextData = string
```

#### Suggested visualizations
Plain text output
