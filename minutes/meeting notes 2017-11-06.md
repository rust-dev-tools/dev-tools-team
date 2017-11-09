# November 6th, 2017

Agenda week, Vidyo meeting: Cargo integration with build systems

## Notes

### Paths for installing Rust programs

* Cargo/Rustup default directories, XDG stuff
* https://github.com/rust-lang/rfcs/pull/1615
* RFC seems stuck, needs a motivated shepherd
* action item: nrc to write a motivation comment to fix the RFC text (there already kind of is one, matklad to reach out to tbu about getting restarted in some fashion)


### Build system integration

See also https://paper.dropbox.com/doc/What-do-Rust-tools-need-from-the-build-system-bxX9pq7l2E7Djq9J7dAy7

#### Motivation and background

* Integrate with big, complex build systems
* There might be Rust <- C <- Rust deps
* Building might all happen remotely (though presumably IDEs have some way to build some code locally too)
* Cargo might do very little (perhaps only configure rustc, but not actually run it)
* We want the Rust tools ecosystem to continue working in this scenario

Potential integration points:

* cargo metadata
* cargo CLI
* parsing TOML/lock files
* cargo API


#### What do tools need from Cargo?

* IntelliJ - consumes `cargo metadata`, then does some kind of build itself
* RLS
  - currently uses Cargo API (the `Executor` API)
  - For dependent crates, needs them to be built and needs save-analysis data
  - Currently replaces rustc to do this, but only because `-Zsave-analysis` is unstable
  - For the primary crates, needs to know the flags and environment Cargo would use, and doesn't need Cargo to build the primary crates
* Rustfmt - is a cargo plugin, but only really needs access to the primary crate source
* Rustdoc (current)
  - needs Cargo to build as usual
  - replace rustc with rustdoc for all crates
* Rustdoc (new)
  - needs save-analysis data for all crates
  - could be by replacing rustc or by passing `-Zsave-analysis`, etc.
* Clippy - basically needs to replace rustc with rustc+Clippy (aiui, the cargo plugin is just a convenient way to do this)
* Bindgen - nothing of note
* Other cargo plugins are not really relevant because they affect Cargo behaviour which is not relevant in the context of integrating with build systems

Question: could cargo metadata be expanded to fulfill tools' needs?

It could help the RLS with the primary crates if it was a way to get flags and environment. But doesn't help with the big issue for most tools which is building deps. Ideally tools want less info from Cargo, but more customisability, rather than more info.

**Important abstraction**: most tools need to be able to build dependencies like `cargo build` would, but with tweaks to rustc (either replacing rustc with another tool or passing extra flags). Tools may or may not want to apply the same thing to the primary crates (or might want to do their own thing with primary crates).

This concept of replacing or tweaking rustc when we run Cargo is what the RLS uses the Cargo API for. It would be good for the RLS and Cargo if this could be replaced with a declarative API.


#### Build scripts

Always complicate things.

[Declarative build scripts](https://github.com/rust-lang/rfcs/pull/2196) might help.

Hopefully not an issue for the RLS or similar tools, whether a build script runs or not is part of the dependencies black box. For primary crates, there might be a build script, but hopefully this is fairly close to the 'pure Cargo' scenario.


#### Cargo plugins

Can `cargo foo` continue to work in the 'integrated' scenario? Depends a lot on the plugin. Most plugins either only touch the primary crates or are so tied to Cargo that they don't make sense in the 'integrated' scenario.

Ideally, shelling out to `cargo build` or other built-in subcommands should continue to work.

Do we need APIs to access info about the build? Ideally `cargo metadata` should supply everything which is needed. Might need fancier things if tools with more complex needs emerge.

