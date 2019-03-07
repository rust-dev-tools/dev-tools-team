# The Rustdoc 2019 Roadmap

As one of the core tools that affects how people perceive Rust both inside and outside the
community, it's important that we maintain rustdoc's reputation as one of the best API documentation
generators. There are a number of in-progress features and numerous incoming feature requests that
we need to balance between our limited resources and the existing tool design. That said, we believe
there are a number of improvements we can make in a year that will help us continue to deserve that
hard-earned reputation.

In keeping with the growing theme of "**maturity**" coming from around the project, this year the
Rustdoc Team will focus on *finishing what we've started*, *polishing what we have*, and *getting
rustdoc in more and more places*. Put more concretely, here are our priorities in 2019:

* Pushing existing unstable features to completion
* Improving documenatation tests
* Reviving JSON output
* Experimenting with an archive and server to emit fewer files

Each of these items is either an extension to an existing feature in rustdoc, or included to address
prominent issues faced by production users and large portions of the community. By pushing these
features, rustdoc can grow with the community as Rust gains more and more users.

To provide some extra detail for each item:

## Pushing existing unstable features

There are (at least) three high-profile unstable features for rustdoc that are partially
implemented. All these features are seeing a fair amount of use already in the community, and they
work well for most situations. But for each one, there's some implementation problem that is keeping
it from being pushed all the way to stabilization. For this year, the Rustdoc Team would like to
coordinate efforts to push these features to their fullest, and call for their stabilization:

* Intra-doc links
* Cross-platform docs with `#[doc(cfg)]`
* Documentation management with `#[doc(include)]`

## Improving documentation tests

Documentation tests (commonly called "doctests") are one of the most powerful features of rustdoc.
The ability to ensure that code samples written into documentation are always valid code creates
confidence in documentation that won't go out of sync with the code it's describing. That said,
there are still places where doctest code can be improved, or new features added to make them more
useful in situations where they're not available today. For this year, the Rustdoc Team would like
to create a "Doctests Working Group" to focus on improvements to the doctest system. Some initial
ideas put forth include:

* Cross-compilation of doctests
* Improvements in doctest error reporting
* Refactoring [code detection][] to properly handle more kinds of code

[code detection]: https://quietmisdreavus.net/code/2018/02/23/how-the-doctests-get-made/

## JSON output

Several years ago, rustdoc supported outputing JSON in addition to its usual HTML. However, the old
implementation of it was buggy and incomplete, so it was eventually removed. This hasn't stopped
people from desiring the feature, though. Having a machine-readable format of the API of a crate
that focused on its documentation would allow Rust documentation to be more easily integrated into
other documentation hosts, or to allow the creation of alternative "frontends" that consume the JSON
to create a different output from rustdoc's classic static HTML files. For this year, the Rustdoc
Team would like to investigate the revival of JSON output and create an initial implementation, to
allow users to experiment with the format.

## Experimental archive format output

One of the biggest performance problems facing rustdoc, especially on Windows, is the fact that it
generates at least one file per documentable "item" in the crate. This is a result of the fact that
rustdoc acts like a static site generator, creating a plain set of web pages from its input. This
combines with the design of the output site to give each item its own webpage to breathe to force
the creation of so many files. One solution to this problem is to make rustdoc output an
intermediate format, like the aforementioned JSON. This could offload the presentation layer to
something that can take an alternate direction with the design of the site.

Another way is to change the encoding of the files on disk. Last year, an [experimental web
server][] was created that used a custom archive format to stream compressed files directly without
decompressing the files and recompressing them again. It was made to serve rustdoc HTML output
effeciently, and demonstrates its use in documentation by compressing documentation created by
rustdoc. For this year, the Rustdoc Team would like to investigate extending its custom archive
format and emitting it natively, so that users can reap the benefits of the file server without
having to emit all the HTML files first.

[experimental web server]: https://github.com/killercup/static-filez
