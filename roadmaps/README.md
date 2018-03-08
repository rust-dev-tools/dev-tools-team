# Roadmaps

Roadmaps let tool maintainers prioritise their work, and advertise to new contributors and the wider community what tasks are high priority.


## dev-tools 2018 roadmap

### Ship it!

We want to ship high quality, mature, 1.0 tools in 2018. Including, 

* Rustfmt (1.0)
* Rust Language Server (RLS, 1.0)
* Rust extension for Visual Studio Code using the RLS (non-preview, 1.0)
* Clippy (1.0, though possibly not labeled that, including addressing distribution issues)


### Support the epoch transition

2018 will bring a step change in Rust with the transition from 2015 to 2018 epochs. For this to be a smooth transition it will need excellent tool support. Exactly what tool support will be required will emerge during the year, but at the least we will need to provide a tool to convert crates to the new epoch.


### Cargo

The Cargo team will have their own roadmap. Some things on the radar from a more general dev-tools perspective are integrating Xargo and Rustup into Cargo to reduce the number of tools needed to manage Rust projects.


### Custom test frameworks

Testing in Rust is currently very easy and natural, but also very limited. We intend to broaden the scope of testing in Rust by permitting users to opt-in to custom testing frameworks. This year we expect the design to be complete (and an RFC accepted) and for a solid and usable implementation to exist (though stabilisation may not happen until 2019).The current benchmarking facilities will be reimplemented as a custom test framework. The framework should support testing for WASM and embedded software.


### Doxidize

Doxidise is a successor to (and reimplementation of) Rustdoc. This year there should be an initial release and it should be practical to use for real projects.


### Maintain and improve existing tools

Maintenance and consistent improvement is essential to avoid bit-rot. Existing mature tools should continue to be well-maintained and improved as necessary. This includes

* debugging support,
* Rustdoc,
* Rustup,
* Bindgen,
* editor integration.


### Good tools info on the Rust website

The Rust website is planned to be revamped this year. The dev-tools team should be involved to ensure that there is clear and accurate information about key tools in the Rust ecosystem and that high quality tools are discoverable by new users.


### Organising the team

The dev-tools team should be reorganised to continue to scale and to support the goals in this roadmap.


## Previous years

* [2017](dev-tools-2017.md)
