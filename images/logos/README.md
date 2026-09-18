# Company / industry logos

Logos used for industry talks in the Speakers section.

| Industry talk       | Logo                                         | Status      |
|---------------------|----------------------------------------------|-------------|
| IBM Quantum         | `../../IBM_Quantum/Raster/RGB/` (theme-aware) | ✅ in repo  |
| Quantum AI Global   | `quantum-ai-global.png`                       | ✅ provided |
| Taqbit Labs         | `taqbit-labs.png`                             | ✅ provided |

Until a `logo:` file exists, the site shows `placeholder.svg`.

To add another industry talk, add an entry to the `speakers` array in
`index.html` with either a `logo:` path (single logo) or
`logoDark:`/`logoLight:` paths (theme-swapped logos).
