# Maintaining tools in the Rust repository

This document contains information for tool maintainers and Rust contributors
who need to work with tools that are part of the Rust distribution and
repository. For information on requirements for such tools see the
[stability docs](https://github.com/nrc/dev-tools-team/blob/master/stability/README.md).

When adding tools or making significant changes, please be sure to cc @nrc and
@alexcrichton on PRs. Adding tools to the Rust repo requires approval from the
dev-tools team and sign-off from the core team.


## Repository layout

Tools should live in their own repository which should be in an official
organisation (e.g., [rust-lang-nursery](https://github.com/rust-lang-nursery)).
Tools should be included into the Rust repository as a Git submodule in the
[src/tools](https://github.com/rust-lang/rust/tree/master/src/tools) directory.


## Constraints on tools

It is essential that any commit used as a submodule in the Rust repo is
preserved forever (i.e., is *never* deleted or changed). This ensures that we
will continue to be able to build any historic release of Rust (which is
important for bisecting regressions, etc). In order to do this, any commit
pulled into the Rust repo as a submodule must be from a Rust organisation repo,
not an author's. If the commit is only on a branch, that branch cannot be
deleted. Whether the commit is on a branch or master, it cannot be rebased
(which essentially deletes the commit and creates a new one).

Tools used in the Rust repo cannot use Cargo workspaces. This is because the
tool itself will be treated as a workspace crate and workspaces cannot be
nested.


## Making breaking changes

If making a change to a 'core' part of Rust causes breakage in any of the tools,
then there is a bit of a dance to perform to land the Rust PR.

Communication is key here! If you're a compiler dev, communicate with tool devs
as soon as possible (and vice versa).

At the end of the day, the tool's maintainers are responsible for ensuring that
their tool continues to build and pass tests in the Rust repo. For non-trivial
fixes, tool devs should be happy to help fix breaking changes.

How to land a breaking change:

* Make your changes to the Rust repo
* Make changes to the tool (against its own repo, not the submodule in the Rust
  repo) to address the breaking changes.
    - check that the tool's tests pass inside the Rust repo
    - either push a branch to the tool's repo or submit a PR
* Point the Rust submodule at the new tool commit
* Land the PR
* Wait for the PR to be included in a nightly release...
* Merge the tool commit to the tool's master branch
    - if the commit was changed or rebased, **do not delete** the commit's branch
    (See https://github.com/llvm-mirror/compiler-rt/branches for example of this policy)
* Create a new PR to the Rust repo pointing the tool submodule back to tool's
  master tip; land it.

When changing the tool submodule, make sure that any other commits are OK to
land (i.e., they are not work in progress and tests pass, etc.).


### Exceptions

Rarely, a breaking change might be too complex to manage in the above fashion.
In such cases it is OK to temporarily stop building and testing the tool while
the tool devs fix the issue.

* If you think this is necessary, you must communicate this to the tool's
  maintainers, the dev-tools team, and the compiler team before landing the PR
  (and preferably before starting the work).
* Try and do this early in the release cycle to give the maximum amount of time
  to fix the tool before the beta uplift.
* It is never OK to do this on beta or stable branches.
* Avoid doing this, if you can.

The steps to follow are similar to the previous steps, but you must comment out
code in the build system as needed, rather than change the submodule, when
landing your initial PR.


## Building and testing a tool

After adding the tool as a submodule, you'll need to make some changes to the
build system to build and test the tool, and to have it distributed with Rustup.

For all these changes, it is probably easiest to see how it is done for an
existing tool and try and copy that where possible. You can try grepping for
`rls` and `Rls` inside [src/bootstrap](https://github.com/rust-lang/rust/tree/master/src/bootstrap).

You'll need to implement `Step` for your tool in various places, then add your
tool to the list in `get_step_descriptions` in `src/bootstrap/builder.rs`.

To build the tool you'll need to add the tool as a workspace to `src/Cargo.toml`
and make several changes to `src/bootstrap/tool.rs`.

To run a tool's tests, you'll need to make changes to `src/bootstrap/check.rs`.

You probably want the 'tidy' script not to check your tool, to do that you'll
need to add the tool directory to the skip list in `src/tools/tidy/src/lib.rs`.


## Distribution

To include a tool as a Rustup component you have to ensure that a tarball of the
compiled tool is created and put in the right place, and that there is an entry
in the Rustup manifest (generated by the build system). You don't need to make
any changes to Rustup itself. You'll need to edit `src/bootstrap/dist.rs` and
`src/bootstrap/install.rs` to create the tarball, and
`src/tools/build-manifest/src/main.rs` to make an entry in the manifest.


## Tips

Rustbuild will update submodules as part of its build process. If you don't
commit a change, it will be overwritten. Therefore, you should either commit (to
both the tool, if necessary, and to Rust) after updating a submodule and before
building, or set `submodules = false` in `config.toml`.

After making changes to `Cargo.toml`, you need to build to ensure `Cargo.lock`
is updated. Changes to `Cargo.lock` must be committed along with `Cargo.toml`
changes.

To run tests for a tool, use `./x.py test src/tools/rls`. Note that it is
possible for tool tests to pass in their own CI, but fail when run inside the
Rust repo because of the different environment.


## People who can help you

Usernames are for irc/GitHub.

* Any questions - nrc/@nrc or ask in #rust-dev-tools
* Rustbuild - simulacrum/@Mark-Simulacrum, acrichto/@alexcrichton, or ask in #rust-infra
* Compiler issues - ask in #rustc, if you need a specific person try mw or nmatsakis.
* Rust distribution - acrichto/@alexcrichton
* Rustfmt or RLS - nrc/@nrc
* Clippy - Manishearth/@Manishearth or oli-obk/@oli-obk
