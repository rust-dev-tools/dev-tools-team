# Stability for tools

We plan to distribute some tools with Rust, primarily via Rustup. For these
tools, we must decide on what stability means and how this is advertised to
users. Since the tools will be part of the main Rust distribution they will be
viewed (to some degree) as part of the 'Rust product' and officially blessed.
There is a risk that if handled badly, deliberate or accidental instability of
tools could reflect poorly on Rust's stability guarantees (which are an
important and sensitive topic).

[Rustfmt](rustfmt.md)
[RLS](rls.md)
[Clippy](clippy.md)
[Rustdoc](rustdoc.md)

## background

Since the 1.0 release, Rust has strived for strong stability guarantees. We have
stuck to a strict semver policy and avoided major version increments. That is,
we have mostly avoid breaking changes entirely. We will probably allow some form
of more major version change in the future; plans are not yet final.

Where breaking changes have been necessary (to fix either soundness or design
errors), we have tested thoroughly (using Crater where possible) and staged
changes as warnings (where possible allowing users to opt-in, and then opt-out)
before making errors. In all cases, breaking changes have been very minor.

There is a new version (semver minor version number increment) of Rust released
every six weeks. This includes the compiler (rustc), standard libraries,
rustdoc, and Cargo (that has its own, independent version number scheme, which
is pre-1.0, but de-facto stable. Changes to Cargo are effectively stable as soon
as they land).

Rustup is the preferred way to install Rust. It is not part of the distribution,
but it manages the distribution and is part of the official Rust project. It is
versioned independently of the distribution.

Rust is also distributed using other installers (the old rustup script, two
Windows installers) and by Linux packagers.

Rust has three distribution channels: nightly, beta, and stable. Nightly is
released nightly, beta and stable are released every six weeks. Nightly is the
current state of the master branch of the Rust repo. On 'release day' beta is
forked from master (equivalently, the current nightly), and the old beta becomes
the new stable. Rust CI ensures that every PR landed in the Rust repo builds and
passes tests and is thus a suitable candidate for nightly.

When new features are added to the language or standard library, they are hidden
behind a feature flag. These must be explicitly turned on using a `feature`
attribute in each crate. Feature flags can only be used on the nightly channel
of Rust, they are disabled for beta and stable channels.

Command line arguments for rustc may also be marked as unstable. These may only
be used if the `-Z unstable-options` flag is also used. This should only be
usable on the nightly channel, but is currently available on all channels
(should be fixed in v1.19).

Some tools need access to the compiler's internals (RLS, Clippy, Rustfmt). The
compiler does not have a stable API, thus for users on the stable channel to use
these tools we must distribute them with the compiler. Primarily, that will mean
there are Rustup components for these tools that the user can opt-in to using.
However, the stability of these tools may be below what we would like to see on
the stable channel. Without further precautions, from the user's perspective
there would be no indication of this (they just add and use a component on the
stable channel). This could frustrate users and harm Rust's reputation.

A key concept is that users should not fear upgrading. That is, upgrades should
fit their expectations - upgrading on nightly might (rarely) cause issues, but
users must explicitly opt-in to the higher risk nightly channel and to
individual features that are most likely to change. Users on the stable channel
expect a high quality product and no breaking changes.


## Key concepts

### Backwards compatibility and quality

There are two components to stability: backwards compatibility and quality.
Backwards compatibility means (in the strictest sense) that when the user
upgrades, their experience only improves - there is no way in which the
experience regresses for any user. For libraries, this means features are only
added to the API, they are never removed or changed. For the language, it means
that code which compiles will continue to compile and will continue to execute
with the same semantics.

Quality is about the expectations the user has for software. Software that is
alpha quality is expected to have some bugs or missing features. Software that
the Rust project officially releases and stands behind, should be relatively bug-
free and complete.


### Backwards compatibility for components

At the coarsest level, a component (such as a tool) that exists, must continue
to exist. This means that the component must be maintained. There is a risk that
the community around a tool disappears and it ceases to be properly maintained.
The Rust community must then find a solution for continued maintenance or
remove the tool from distribution. The latter is clearly a backwards
compatibility hazard.

More interesting is what backwards compatibility means 'within' a tool. I think
this must be a per-tool thing, and we'll try to define it for some tools in
other files in this directory.


### Version numbers

A 1.0 release has special significance. Technically, there is nothing special
about 1.0 - breaking changes can still be made by incrementing the major version
number and there is no explicit quality guarantee. However, there is a de facto
expectation in the Rust community that 1.0 software will only rarely make
breaking changes and will be of high quality.


## Proposed changes for tool distribution

These changes are still under discussion.

There will be a path for tools to be added to the Rust distribution. These will
be Rustup components (optional, at least at first); status for other installers
is TBD. Tools' tests will be run by the CI and only tools with passing tests
will be installed by Rustup (the details of this still need to be agreed).

One possible path is to add a new channel - 'nightly-tools'. The nightly channel
will be released every night that Rust tests pass (i.e., every night). The
nightly-tools channel is released when all tools tests also pass (hopefully,
most nights).

The name of the Rustup component could be used to reflect the stability of the
tool. E.g., using `rls-alpha` or `rls-nightly`, rather than just `rls`. We could
also add a flag to rustup such as `--unstable-components` which is required for
channels other than nightly to install 'unstable' tools.


## Milestones for tools

We should define how stable a tool must be for each of these milestones.


### Inclusion with 'Rust product'

* included into the main repo
* tests run on CI
* distributed on nightly channel only
* a Rustup component

Current example: RLS


### stable channel

* distributed on all channels (stable, beta, nightly)
* possibly distributed on channels other than Rustup


### stable

* tool is 1.0 (and meets expectations for 1.0 stability)
* Rustup component is un-suffixed

Current examples: Rustdoc, Cargo
