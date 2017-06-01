# May 30th 2017

Focussed week - Rustdoc, irc meeting in #rust-dev-tools

## Notes

### Overview/future plans

Areas for potential improvement

* improve contribution
* technical debt
* UI/UX
* multi-toolchain support (platforms, feature flags, etc)
  - e.g., Windows only stuff not showing up on Linux builders, etc.
* 'small' technical issues
  - re-exports, auto-traits, etc.

Rewrite is meant to address the first two, help with the third and last.

### the rewrite

* Use the same data as the RLS, provided by rls-analysis crate.
* Separate Rustdoc from the compiler (could be in separate repo)
* Frontend is web-tech, no Rust/complications
* Backend is a Rust app, runs offline. Processes RLS data via rls-analysis, produces Rustdoc-specific JSON data.
* JSON data is statically served
* Frontend is a web page/app, renders the JSON as web pages
* Extra modes (long term goals)
  - static mode - we generate HTML offline and statically serve pages (for no-JS people)
  - live mode - uses minimal RLS data (maybe more important long term with incr. compilation), and produces Rutsdoc (JSON) data on the fly
  - in-page mode (compile to WASM) and put everything in a web page

#### backwards compatibility

* URLs
  - must at least have redirects
* search box, search syntax, search via URL
* anchor links - e.g., https://docs.rs/itertools/0.6.0/itertools/trait.Itertools.html#method.sorted
* rustdoc input

### theming

We briefly discussed on theming. Having multiple renderers (frontends in the new system) also a possibility. Separating frontend should make theming easier.

### "the rustdoc book"

Steve is planning to document Rustdoc. Cover flags, attributes, etc. Help would be very welcome. Could be a step towards a tools book. The man page is an alternative or source. Similarly to good docs, a good test suite would help with the rewrite.

### Not discussed, but notes from etherpad

* docs.rs/inter-crate linking
* design vision
* how can we better present information? concise overviews/API references vs. browsable docs with lots of 'prose'
* add more interactivity to the way we render docs? e.g.,
  - collapse trait impls by default (just trait name + method names)
  - inline expand related information (e.g. click on result type to get a summary inline)
  - runnable playpens in the docs
  - context aware search that highlights results for items the current page
* benchmark: how to *nicely* render the docs page of a struct with 15 trait impls?
* linting of doc content: markdown structure (cf. <https://deterministic.space/machine-readable-inline-markdown-code-cocumentation.html>), grammar -> might actually become part of clippy
* full-text search
* Add other outputs (I was thinking to markdown for example)
* RFC 1946 - https://github.com/rust-lang/rfcs/pull/1946
* technical issues
  - I expect the rewrite will force a completely new set of issues
  - macros
  - re-exports
  - auto-traits
  - see also https://public.etherpad-mozilla.org/p/rustdoc-tool-peers
* Does it make sense to integrate rustw into rust doc's [src] view?
  - context: https://github.com/rust-lang/rust/issues/39726#issuecomment-303242391 and @nrc's reply
* rustdoc as renderer for mdbook

