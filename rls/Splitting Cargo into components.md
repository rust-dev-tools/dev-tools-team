## Build backend
One of the main goals would be to allow building Rust with different build _backends_.

Idea: Split Cargo into
```
                       resolution/download
Cargo.toml -> [Cargo] ----------------------> abstract build plan -> [Build backend]
```
Would this mean that `cargo build` would shell out to a respective backend as part of its routine (e.g. call `cargo-buckend --plan=file.json`)?

Backends would be responsible for translating the intermediate rules to the ones the build executor understands (in this model `build.rs` would be an implementation detail for Cargo build executor).


### Resolution
Cargo resolves optional dependencies, concrete package versions and feature set as part of its resolution.

How does it affect integration (especially vendoring)?

Translating and commiting rules _a priori_ for the external build system presumably makes the maintenance easier but makes some Cargo-specific workflow harder: in particular, because external build system rules correspond to a Cargo post-resolution (lockfile) state.

How would `cargo update -p <pkg>` in post-translation environment work? Many obstacles:
* We'd need to understand external rules
* synthesize an internal Cargo lockfile
* How to get package *constraints*? (for all we know all dep constraints could be of concrete `=1.2.3` format)
* Retranslate and clobber existing build system rules?