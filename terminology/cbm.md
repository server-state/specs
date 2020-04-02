# Client Base Module (*CBM*)
A client-base module provides a visualization of data from a Server Module that can get used in the  [client-base](https://github.com/server-state/client-base).

CBMs are, in the simplest form, the combination of some metadata and a React component that follows a standard that makes it possible to integrate them into client software implementing the required interfaces.

## Development process
CBMs can get developed using either the tooling provided by Server State or custom solutions implementing the required standards,  as established by the Server State specifications.

CBMs get packaged into JS-compatible  .cbm files.  These can either get distributed and installed manually or via the official or an unofficial CBM Registry.

## Dos and Don'ts
A CBM,  when viewed in an MVC-like model,  is nothing bu t a view component.  They are responsible for providing a visual representation of the data passed to them, **not to**
-   actively manipulate that data
-   take active action in one way or another
-   escape the encapsulated environment in and for which they get used

These restrictions **do not mean** that it's impossible to provide powerful features of CBMs that,  in a non-intrusive way, enhance the visualization of the data.  These features may include settings for the visualization,  showing modals with more data _upon user interaction,  and more_.

It's also possible for a CBM to self-refresh its data.  Refreshing gets achieved by calling an outside method passed to the component and not from within the component itself.  This outside call effectively destroys the component instance and re-renders it using the freshly fetched data.  Moving this to the outside means that except for rare exceptions, **CBMs do not need to perform any external communication like,  e.g.,  HTTP/-S requests**.

**A CBM should** provide a visualization of the data passed to it that
-   provides a good User Experience for inspecting the data
-   works as one component in a whole dashboard
-   works across the relevant sizes  (width:  240  -  1920px)  and devices  (Desktop,  Mobile,  etc.)
-   adjusts,  in a reasonably good way,  to the settings provided to it,  like the current theme
-   if possible,  uses the Material Design Components  (and library)  used by the client-base to fit the surrounding interface nicely
-   detects incompatible data types passed to it and _throws_ useful error messages  (that,  then,  get handled outside the CBM)
-   renders performantly  (as a lot of CBMs might get rendered simultaneously)
-   is free of any external resource dependencies,  i.e.,  all a CBM needs to render is the data provided to it in a suitable JSONLike format.
- has as much localization as possible, but at least provides English information

## Technical requirements
### Distribution and the `.cbm` file format
CBMs get distributed via a CBM registry. A central, most-used official registry, provided by the SerSta team, exists, but it's also possible to host one's own registry, e.g., for internal company purposes.

To upload a CBM into the registry, a `.cbm` file has to get built using the `@server-state/cli`, available in npm.

This file format is, at a basic level, a JSON file containing an `object` with the following properties:
- `id: string` A unique plugin id, provided by the registry.
- `name: string` The CBM's name
- `version: string` An npm-compatible version number
- `description: string` A description of the CBM
- `support_url: string` A url where users can get support, including the capability of contacting the CBM's author
- `website: string | undefined` The CBM's website, if applicable
- `repo_url: string | undefined` The CBM's repository, if applicable
- `code: string` The CBM's code in JS, see the specification below

### Specification of the `.cbm`'s `code: string` property
The `code` property is a string containing valid JavaScript code that exports (in CommonJS2, i.e., using `module.exports = {}`) an object containing the following properties:

- `component: React.Component` A react component that actually defines the CBM, as defined below in *The React Component*
- `isCompatible: (data: JSONSerializable) => boolean | undefined` A function that checks whether the `data` is in a format that can get visualized by this CBM. For compatibility reasons, providing this function is optional, but not providing it leads to a flag stating *Compatibility unknown* in the CBM selection screen in the *client-base*.

While most dependencies have to get compiled into the code itself (this happens automatically when using the SerSta CLI), some modules get provided when parsing the code and should get used for developing CBMs:

- React-specific modules: `react`, `react-dom` and `prop-types`
- Design system modules: `react-md` and `material-icons`
 
### The React Component
To fit the requirements we laid out above,  the following arguments  (in the form of props)  have to get passed to a CBM's component and get considered to be the standard for CBMs:
-   `data: JSONLike`:  The data received from a Server State Instance.
-   `registerMenuItem: (label: string, cb: () => void) => void`:  Registers a menu item that,  according to this standard,  must get made accessible to the end-user within the context of the CBM usage.  When the menu item gets used,  the callback passed as cb gets called.  Duplicate calls with identical labels override each other.
-   `refresh: () => void`:  A function that,  when called,  leads the client software to refresh the data for this specific CBM usage,  leading to the CBM getting replaced by a loading screen and then re-rendered,  effectively destroying the CBM instance.  It should never get called when it's unsafe to destroy the CBM component.
-   `theme`:  The library's theme object should get respected by the CBM to the greatest extent possible without lowering usability
-   `storage: { get: (key: string, val: JSONLike) => void, set: (key) => JSONLike }`:  A permanent,  in-browser storage for this specific CBM usage.  It can get used to store per-usage preferences or similar values.
-   `lang: string`:  A two-letter language code for the current UI language of the client software,  following the _ISO 639-1_ standard.  If the passed language is not supported,  English gets used.

## Compatibility
for the standards laid out in this document  (and thus,  of version 1.0 of the Server State ecosystem)  gets kept as long as possible within the justification of reasonability.  Providing compatibility does, explicitly, not forbid additions,  as long as we keep backward compatibility.

We,  the Server State team,  recognize the importance of external developers for a thriving ecosystem and, thus, the success of the project itself and vow to recognize and follow the above statement with the utmost priority.
