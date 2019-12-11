# Client Base Module (*CBM*)
A client-base module is a visualization that can get used in the [client-base](https://github.com/server-state/client-base).

It gets passed the data returned form the API and visualized the data.

We release CBMs as npm modules with a naming schema of `[module-name]-cbm` for modules specialized for a 
specific *SM* or `[data-type-name]-[vis-name]-cbm` for general CBMs for specific [standardized data formats](/arch/data-formats.md).

### Examples of names
- `linux-raid-module-cbm`: A CBM that can get used for the `linux-raid-module` (which doesn't use a standard data format)
- `xydata-line-chart-cbm`: A line chart visualization for XYData
- `xydata-table-cbm`: A table visualization for XYData

### Technical specifications
CBMs are "importable" modules (may they get installed via npm, locally or otherwise), that consist of a Component and some Meta info.

The structure of an CBM should allow, if the CBM is in `my-cbm`, to be importable as `'my-cbm'`, giving an object (`{component: ReactComponent, info: object}`), `'my-cbm/component'`, giving `'my-cbm'.component` and `'my-cbm/info'`, giving `'my-cbm'.info`.

`info` has to follow an interface that looks as follows:

```
{
  name: string | IntlString,
  description?: string | IntlString,
  about?: string | IntlString,
  version?: string,
  logoUrl?: string, // Ideally, a base64 encoded data URI
}
```

The component is the CBM shown to the user and must work for widths from 320px to 1080px. It gets passed the following `props`:

*   `data: JSONSerializable`: The data returned by the API endpoint
*   `onRegisterAction: (name, action) => void` a function that lets the CBM register an action for the "Three-Dots/More Actions" menu
*   `onRefresh: () => Promise<void>` a function to reload the API response for this CBM from inside the CBM (e.g., for modules requiring frequent updates etc.)

Components get registered in `/src/config/component-registry.js` as the object importable from `my-cbm`, i.e., the object containing `component` and `info`. Because of this, it's possible to use something like:

```
import MyCbm from 'my-cbm';

const components = {
  MyCbm.info.name: MyCbm,
  [...]
};
```
