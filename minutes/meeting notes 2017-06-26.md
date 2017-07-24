# June 26th 2017

Agenda week, cross-compilation and Xargo


## Background

* https://gist.github.com/japaric/30dee827310dbf640bee461ac2eefc40
* https://github.com/nrc/dev-tools-team/blob/master/roadmaps/xargo.md
* https://github.com/japaric/cross/
* https://github.com/japaric/smoke


## Notes

* Merge with Cargo? https://github.com/rust-lang/rfcs/pull/1133
* Rustup distribution of cross-compiled std libs?
* Overview of Xargo, Cross and Smoke
  - See https://gist.github.com/japaric/30dee827310dbf640bee461ac2eefc40

* Xargo stands in the way of doing no-std development on stable for *some*
  targets (e.g. `thumbv7m-none-eabi` / ARM Cortex-M microcontrollers)
  - It's considered the number #1 blocker (along with the `panic_fmt` lang item)
    for stable embedded development.
    - https://github.com/rust-embedded/rfcs/issues/31
  - The easiest and fastest way to solve this problem is to produce and provide
    rust-std components for those targets. This approach can only be used for
    built-in targets (cf. `rustc --print target-list`)
  - The second easiest but hackiest way to solve this is to make Xargo work on
    stable. There's a mechanism in rustc that lets you use unstable features on
    stable it's only meant to be used by the Rust build systems for
    bootstrapping the compiler. Xargo could use that provided that the Rust
    developers are OK with keeping it relatively stable to not break Xargo.
  - The proper way to solve this is to teach Cargo how to compile the sysroot
    from source.
    * An RFC was proposed for this (and other things like getting rid of the
      sysroot altogether): https://github.com/rust-lang/rfcs/pull/1133

* How do we get Cross to work on macOS / Windows?
  - https://github.com/japaric/cross/issues/102
  - Both macOS and Windows now support Docker and can run Linux docker images so
    it should be possible to use Cross on those platforms.
  - Cross assumes there's a rust toolchain *for Linux*, as in the toolchain is a
    Linux / ELF binary, installed on the host. It uses that for cross
    compilation.
  - To make Cross work on macOS we would have to install a Linux toolchain
    *and* also rust-std components for that Linux toolchain on the host.
    rustup seems to be OK with the first operation (`rustup toolchain add
    nightly-x86_64-unknown-linux-gnu`), but I haven't tested the second one.

* Cross and QEMU *system* emulation
  - Cross uses QEMU user emulation to run cross compiled unit tests, but this
    emulation mode has problems with threads and can hang if the unit test uses
    too many threads.

  - An alternative is to make Cross use QEMU *system* emulation, but this is
    more complicated because requires spawning a QEMU virtual machine that fully
    boots a Linux kernel / distribution.
    * Alex has done some work on this area and landed full test suite support
      for ARM in the rust-lang/rust repo.
    * https://github.com/rust-lang/rust/pull/39400

* Would it make sense to share Docker images between Cross and the
  rust-lang/rust project?
  - Both projects use Docker images and have the same goal of producing the most
    portable binaries possible.
  - This might lead to use Cross within the rust-lang/rust repo, and leave all
    the management of Docker images to the Cross project.

* Revive the Smoke project, and make it use Cross

ACtion items

* Japaric to write a minimal Xargo in Cargo RFC
  - brson to help
* nrc to bring up bootstrap key use with core team
* nrc to talk to infra team to see if anyone is interested in working on smoke and linking in with our infra
