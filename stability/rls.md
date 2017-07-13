# Stability - RLS

This document lays out what stability means for the RLS. It discusses what
backwards compatibility means, some possible measures of quality, and what is
required for several milestones toward stable release. For background on what
stability means for tools in general see [README.md](README.md).

The RLS is interesting from a stability point of view because it is not user-
facing and is only used directly by other tools. I've tried to define stability
in terms of the RLS's interfaces, rather than in what a user would encounter in
a client. Stability should thus be independent of the user's editor choice.

## `unstable_features` option

Can be set by users per-project in rls.toml (i.e., it is opt-in). It cannot be
changed when the RLS is running.

It is currently used for features that are buggy or where there is a non-trivial
chance of breaking user's code. We should extend its use to features where we
don't want to promise backwards compatibility.

We should restrict use of `unstable_features` to the nightly channel. I.e., if a
user is on non-nightly, then the option is ignored and they can't use any
unstable features.


## Backwards compatibility

Where we promise backwards compatibility:

* RLS continues to compile inside the Rust repo (compiler changes, dep crates e.g., rls-data)

Because of compiler dependence and tight integration with Cargo, this is less
trivial than it sounds.

* using the LSP

We will continue to use LSP v3.0. We'll only upgrade the LSP version if it is
backwards compatible (for all clients) to do so. We won't change to some other
protocol.

* adding/removing/changing LSP messages

We will not remove message support. We can add message support freely. The
actual data returned for any message may change, but processing a message should
not change (for example, if a message returns a list of data, each datum may
gain fields, but should not lose fields, the number of data may change).
Informally, for any message we may return more information, but not less
(however, we do not guarantee this).

* LSP extensions

LSP extensions that do not require the `unstable_features` config option have
the same status as core LSP messages. Extensions that do require that option
have no guarantees.

* --version

The `--version` command line option will continue to exist and return data in
the same format. The data will change.

Where we do not promise backwards compatibility:

* API of helper crates (rls-analysis, save-analysis data)

These are unstable dependencies, their stability needs to be tracked separately.

* command line options

All command line options (except --version) are unstable.

* command line interface

The CLI mirrors the LSP interface and so will in practice be somewhat stable.
However, it is only intended as a debugging tool, so the interface may change or
it may disappear entirely (we could restrict use to the nightly channel).

* unstable features

Any feature that requires the `unstable_features` config option can change in
any way at any time.


## Quality

Possible measures:

* coverage of LSP
* bugs
  - analysis data
  - cargo coverage
  - crashes/hangs/overusing CPU
* other features
  - formatting
  - refactoring
  - code generation
  - apply suggestions
  - auto-imports
  - ...
* usable in multiple editors
* size of test suite

## Milestones

### Inclusion with 'Rust product'

I.e., what does RLS need to do to ride the trains

* Agree on back compat guarantees
* Tests running on Rust CI
* De facto unstable features are behind the unstable features option
* Unstable features option is linked to nightly toolchain
* Decide on name of the Rustup component
* Proper channel info from `--version`.

### stable

I.e., when we can announce 1.0

* Quality - waiting on bug reports from stable channel release
* Cargo project coverage - lib + bin, workspaces
* Better testing (better protocol coverage, prevent regressions due to compiler data)
* new features - TBD
