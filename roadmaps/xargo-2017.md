# Xargo

[Xargo] is a Cargo drop-in replacement that automatically compiles and manages a
sysroot for you. It's main use case is compiling the `core` crate for targets
where a binary release of that crate is not available via `rustup`.

[Xargo]: https://github.com/japaric/xargo

## Getting rid of Xargo

Xargo requires a nightly (or dev) compiler to work thus targets that require
it for development are stuck with the nightly channel for as long as Xargo is
required.

The proper way to get rid of Xargo is to add Xargo's functionality, building a
sysroot / std facade on the fly, to Cargo. There's [an RFC] for this, but it has
not been approved yet.

[an RFC]: https://github.com/rust-lang/rfcs/pull/1133

## Removing the dependency on Xargo

The Xargo functionality will take a while to land in Cargo and be stabilized.
Meanwhile we can remove the Xargo requirement from *some targets* by providing a
pre-compiled sysroot in the form of a `rust-std` component installable via
rustup. The targets where this is more urgently needed are:

- `thumbv6m-none-eabi`
- `thumbv7m-none-eabi`
- `thumbv7em-none-eabi`
- `thumbv7em-none-eabihf`

Because these targets *require* Xargo to get any development done.

The next steps for getting a `rust-std` component out for these targets are:

- [ ] Landing the `compiler-builtins` repository in rust-lang/rust.
  [Instructions]

[Instructions]: https://github.com/rust-lang-nursery/compiler-builtins/issues/66#issuecomment-305943313

- [ ] Start building binary releases of the `rust-std` component. [Previous
  attempt]

[Previous attempt]: https://github.com/rust-lang/rust/pull/41133

## Improving Xargo

I pretty much consider Xargo done at this point. There is, however, one feature
that I think a sysroot-aware Cargo may support that Xargo doesn't support now:
rebuilding the sysroot when its source changes. [Issue]. This is not supported
right now because the main use of Xargo is compiling the source provided by the
`rust-src` component and that source doesn't change unless you update your
nightly compiler.

[Issue]: https://github.com/japaric/xargo/issues/139

Other ways to improve Xargo is to improve documentation and squash remaining
bugs. Check the [issue tracker] for issues that require your help.

[issue tracker]: https://github.com/japaric/xargo/issues
