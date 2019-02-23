# IDEs and editors - 2019 planning
Looking back at 2018, our team managed to ship RLS (Rust language server) out of preview state with the Rust 2018 edition release, spawn a separate IDE project dubbed Rust Analyzer that’s focused on latency and lazy computation, prepare Racer for the Rust 2018 edition, and more!

We’re already more than a little into 2019 so now it’s a good time to plan ahead and see what this year may look like for IDEs and editors! Below you can find what we’ll have in store for the relevant tools.

## RLS 2.0 (…?)

Fresh out of Rust All Hands, which was held and attended by the Rust team in Berlin two weeks ago (Feb 2019), we’re excited to say that we spent a considerate amount of time talking about the future of the Rust IDE support and the fruit of that is formation of an experimental RLS 2.0 compiler working group:

https://github.com/rust-lang/compiler-team/tree/master/working-groups/rls-2.0

Initially built on top of rust-analyzer, the primary goal is to discover an ideal infrastructure on top of which we can build an IDE-aware compiler, while also exploring how we can split `rustc` into more manageable chunks with better-defined boundaries.

To put more context into why this is exciting and needed work, it’s good to explain how the RLS currently works. Right now, to get semantic knowledge about a certain crate (e.g. to be able to find all references, go to definition, rename a symbol) we execute `rustc` in a special mode that dumps all the necessary information after the compilation is finished, which is then indexed by the RLS.

This can piggyback on the existing incremental compilation infrastructure to quickly rebuild the necessary information, however the “frontend” of the compiler (parsing, macro expansion, name resolution and partially HIR lowering) is not yet incremental and on-demand, which means `rustc` always has to perform these passes in full, leading to increased IDE latency and doing unnecessary work.

Incrementalizing the compiler frontend proved to be a hard and deeply-coupled problem, which is what the team aims to tackle in order to achieve an IDE-aware compiler.

Hopefully, this will also allow to indirectly improve already existing tools, such as Rustfmt, Racer or even existing RLS itself. More info in the [link](https://rust-lang.zulipchat.com/#narrow/stream/185405-t-compiler.2Frls-2.2E0/topic/scope.20and.20how.20to.20proceed) above, join us on [Zulip](https://rust-lang.zulipchat.com/login/#narrow/stream/185405-t-compiler.2Frls-2.2E0/topic/scope.20and.20how.20to.20proceed)!

## Rust Analyzer

Rust Analyzer is an experimental modular compiler frontend for Rust. Aleksey Kladov ([@matklad](https://github.com/matklad/)) wrote an [extensive post](https://ferrous-systems.com/blog/rust-analyzer-2019/) about Rust Analyzer in 2018 and 2019 already, so be sure to check that out, instead!

## RLS (current)

While we plan to explore and experiment with another approach to IDE-first compiler, we continue to actively maintain and to improve the existing RLS (dubbed “1.0” now).

Over time, we accrued a lot of infrastructure debt - we focused on the features and fixing bugs but we never came around to bringing the testing/development infrastructure up to par.
Because of that, we plan to improve our story around that. Short-term, we’d like to simplify the development process and improve the debuggability, including better error reports.

For development, we plan on centralizing development in a monorepo (pulling external RLS-specific crates such as `rls-analysis`), doing a thorough issue triage and focusing on mentoring more issues.
In addition to that, we plan on writing a reusable declarative test harness for LSP and an async LSP client, which should facilitate the testing process and make it available for other LSP servers in development.

For debuggability, we plan to surface internal errors to the user more gracefully (think [squiggles in Cargo.toml](https://github.com/rust-lang/rls/pull/1089) for Cargo errors), as well as introducing a debug mode, which sets up e.g. logging out of box and records used toolchain and tool versions, ready to report in an [issue](https://github.com/rust-lang/rls/issues).

Feature-wise, we’d like to explore enhancing Racer completion with rustc’s type information, hopefully offering a more complete (pun not intended) experience.
Improving latency is inherently tied to incrementalizing the existing Rust compiler frontend pipeline (referred to as end-to-end queries) and is now separated as part of the aforementioned experimental RLS 2.0 working group, which means we won’t prioritize this work as part of the RLS effort.

## IntelliJ Rust

**Code analysis** is big on our agenda for this year. As you might already know, we don’t use RLS/Racer or rustc whatsoever in IntelliJRust. Instead, our lexing, parsing, type inference, and all the code analyzing stuff is written from the ground up. It lets us set up prominent code completion, on-demand code analysis, and have low latency even on a large codebase. The drawback of this is that from time to time it may behave a little differently to rustc. Typical examples of bad behavior include false positives, some names are unresolved, or some types may not infer.  
This year this situation is going to change drastically. We’re making our on-the-fly analysis operation as close to rustc as possible. The plan is to put together an analysis testing system for real-life projects (somewhat similar to Crater), that will give us an indication of what the most common issues from real projects are for us to prioritize. This should help us to make our code analysis features gradually better over the course of the year.

**Macros** are still a tough nut to crack. Despite IntelliJRust resolving names declared by a macro without any hassle, it still totally ignores impl declarations and doesn’t have any highlighting, completion, or go-to-definition action inside of a macro. We are planning significant all-around improvement for macros this year. For instance, recognizing an impl declaration from within a macro is in active development right now, and should be available pretty soon! 

**General performance** can never be snappy enough, and it is in our scope this year to see what we can do about it. Considering that macro processing in its current state is the biggest cause of slowdown, enhancing this alone will have a big effect on reducing lag. Additionally, we are going to rewrite our resolve engine. It is already fairly old and has some specific cache problems which are affecting the overall performance. 

**Auto-import driven code completion** can be expected really soon!
With this feature, code completion works with names which are out of scope. The corresponding module will be automatically imported as soon as a completion suggestion is applied.

Being an open-source project means our endeavors are shared by awesome contributors from all over the world. You are welcome to [join](https://github.com/intellij-rust/intellij-rust) our cause!
Otherwise, consider supporting us by tuning in for updates:

- https://intellij-rust.github.io/
- https://twitter.com/intellijrust

See you soon in 2019!

