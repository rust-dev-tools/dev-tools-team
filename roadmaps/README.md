# Roadmaps

Roadmaps let tool maintainers prioritise their work, and advertise to new contributors and the wider community what tasks are high priority.

Tool roadmaps are currently at the 'brainstorming' stage - we're collecting a list of tasks that might be on the roadmap for the rest of the year without prioritising or judging eligibility. As such, you should read these documents with a large grain of salt they'll probably change a lot (or eat your laundry or whatever).

* [Bindgen](TODO)
* [Cargo](TODO)
* [Clippy](TODO)
* [IntelliJ](TODO)
* [The RLS and IDEs](rls.md)
* [Rustdoc](TODO)
* [Rustfmt](rustfmt.md)
* [Rustup](TODO)
* [Racer](TODO)
* [Xargo](xargo.md)


## General issues/cross-cutting concerns

See also [Rust roadmap](https://github.com/rust-lang/rfcs/blob/master/text/1728-north-star.md).

* Improve contribution

For most tools, I feel like their main problem is being under-staffed. Some are lacking more experienced contributors. I would like to increase the number of contributors and the level of contribution for all tools. This falls under the 'mentoring at all levels' category of the Rust roadmap.

* Discovery and delivery (roadmap - easy access to high quality crates, 1.0 level crates for essential tasks)

I want it to be easy for users to find and get started using high quality tools. Those tools should be 1.0 quality. This is a tools version of the roadmap goal of having 1.0-level crates.

* IDE support

This is straight from the Rust roadmap - "Rust should provide a solid, but basic IDE experience"

* Rustdoc

Rustdoc has been one of the better tools for Rust for years, and is probably the most widely used. However, over the last year or so, it has not got the attention it needs. I want to get it back on track.

* Better understand what devs want/what is missing

We've been playing catch-up with IDE support - it is currently one of the most desired features for Rust, but the core community was pretty late in prioritising development. I want to get ahead of the curve and be working on the next important tool before it becomes necessary and overdue. That means getting a better understanding of what is wanted or missing in the current world.

* Explorative: tools for learning

The first item on the Rust roadmap is "Rust should have a lower learning curve". I think tools can be a part of that, and I'm keen to see if there are innovative ways we can support this goal.

* Explorative: integration with other languages

This is an 'explorative' item on the Rust roadmap, and I think it could have an important tools component. Polishing and maintaining the bindgen experience is an obvious part of this, but perhaps there are opportunities for using tools to make the cross-language better in other areas too.


## Other areas of interest

* Debugging
* Testing/benchmarking
* Rustfix or similar tools (especially in support of epoch migration)
