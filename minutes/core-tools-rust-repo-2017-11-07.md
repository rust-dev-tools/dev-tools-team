# Core team (subset) meeting on tools in Rust repo - 2017-11-07

## How's it going

* basically OK
* a bit more bustage than expected
* was tough with the old dance, but with the ability to break tools, it's OK
* rustup UI is an issue
  - installing rls component when it's missing
  - updating rls component when it's missing
* concern raised about the long term design for the RLS. Will the query design cause more breakage?
  - we think no
 

## Next steps

* turn on Clippy tests (bustage might happen more often, but shouldn't be harder to deal with)
* distribute rustfmt (we already block RLS, so nothing more should break)
  - rustfmt-preview component
* distribute Clippy later (let's see how the next phase goes, then do this in 6 or 12 weeks)
* let's not add any tools to the combined installers (only rustup) to see if that is a problem or not


## Possible mitigations

We want to make life easier for Rust contributors and for tools users (especially on nightly).

* Rustup UI improvements mentioned earlier (https://github.com/rust-lang-nursery/rustup.rs/issues/1277)
* tracking issue for bustage for people to follow?
  - not clear if this would help much
* schedule broken/non-broken times for tools, e.g., not bustage one week before beta
  - No. Current system seems to be working, we can fix on beta.
* Move libsyntax to crates.io
  - still lives in Rust repo, just published (automatically) to crates.io
  - doesn't need to be stable
  - should be fairly easy
  - file an issue (done - https://github.com/rust-lang/rust/issues/45859)
* vendor the tools into the repo and upstream changes
  - No. More effort than it is worth, but could reconsider if things get worse in the future.
* handle tools bustage automatically rather than devs opting-in with the manifest
  - lowers bar for Rust contributors
  - test breakage would not block CI
  - automation would ensure that breakage is monitored and interested parties are notified
  - only on master/nightly, beta would still have blocking CI
  - downside - might lose some testing via tools
  - file an issue (done - https://github.com/rust-lang/rust/issues/45861)
 
## Running Rustfmt

* nrc and others want to start running Rustfmt on the Rust repo
* suggestion: start voluntarily, one commit at a time, then enforce on CI (one commit at a time), then do everything left over
* alternative: flag day - do the whole repo at once, turn on CI
* the flag day alternative was preferred
* could start with other tools and crates - Cargo, Rustup, top 10 crates.
* need to be distributing with Rustup before starting this stuff
