# Stability - RLS

WIP

* RLS sv VSCode vs other IDEs
* somewhat interesting because the RLS has no users, only clients which have users

currently there is an option to enable 'unstable features', users opt-in in rls.toml
* currently used for broken stuff
* should extend for stability
* could restrict to nightly channel (some how)

## Backwards compatability

* continuing to compile (compiler changes, dep crates e.g., rls-data)
* using the LSP (c.f., some other protocol)
* adding/removing LSP elements
* LSP extensions
* API of helper crates (rls-analysis, save-analysis data)
* command line options, command line interface


## Quality

* coverage of LSP
* bugs
  - analysis data
  - cargo coverage
  - crashes/hangs/overusing CPU
* other features
  - foramtting
  - refactoring
  - code generation
  - apply suggestions
  - auto-imports
* use in mutliple plugins
* size of test suite
