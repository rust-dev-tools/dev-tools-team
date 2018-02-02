All proposals have essentially the same motivation

Goal is to allow any kind of test by enabling custom test frameworks
and integrating them into cargo.

Cargo integration is very important

# Custom test runners

1. Jon: proc macro generating main function from tokenstream for all
items annotated with proc-macro-specific attribute (no mutation of item)
2. Nick: runtime calls each test
3. Manish: essentially same as 1, but instead of rustc-magic use a
crates.io crate (can do arbitrary things with the crate during testing)

Jon: Doing 1. first, because 3. can later be added backwards compatibly
and everybody can focus on the common case

Manish: current framework is based on AST, which we don't want to expose

Jon: wary of expanding the scope to "everything"

Manish: Do we stabilize 1. and have to stabilize later a more general
interface?

Jon: Don't stabilize if 1. doesn't work

Pascal: Do both proposals allow test from different test frameworks in
the same file

Manish: yes, but not running both simultaneously

One test framework can invoke another test framework

Pascal: prefers limited framework (1.)

Manish: stabilization requires proc macros stabilization first anyway

Nick: Trait based instead of proc macros?

Manish: elaborate?

Nick: Instead of token streams, get trait objects

Jon: My original proposal was entirely trait based. You end up with a
bunch of unfortunate limitations. Multiple test runners complicated.
Annotating non-fns (modules, types) is not really possible in there.
The method on the trait would end up having to take token streams, so
you end up back at the proc-macro proposal

Jon: 1. does not allow modifying the token streams (unlike original
proposal), if you want modification, use proc macro. The output token
stream can only generate new code

Killercup: Can duplicate entire input-stream but slightly modified.

Nick: 2. is not the way to go

# Output formatters

Both proposals don't standardize the output, but rustc publishes a
crate that has default output to be used.

Killercup: add machine readable output to libtest
hard, happening in a PR right now
want to unify custom test framework output, so IDEs only need to
understand a single test output format (via rustc test output crate)

Discussion in forum: create super simple stable json output now, do
better stuff later

Killercup: dislike, because design space is too big

Jon: orthogonal topic to custom test frameworks
opt-in to standardized input for better integration, but allow println!

Killercup: as a first step, just 3 events (success, failure,
allowed_failure) + custom extra data field

Jon: don't constrain future test frameworks by design, but look at the
things that test frameworks output and create the design from that

Killercup: prefer having a design before having custom test frameworks

# cargo integration

Jon: both proposals have a way to specify in Cargo.toml custom test
kinds (by giving the crate name) and a set of files that use that test
framework

oliver: compiletest?

Manish: proposals should allow compiletest, but it's not explicitly
discussed.

# cfg

cfg attributes can be problematic, because they are evaluated before
proc macros

# doctests

Jon: how does that integrate with them? Pun until later, b/c special
anyway

# Meta

Jon: record chats and store somewhere?
Killercup: hangouts on air for recording and livestreaming to youtube

# Action items

Manish: Merge Both rfcs (into Jon's b/c execution contexts is a cool
name)

Jon: ~~write summary for each point~~ strike that, we have notes
