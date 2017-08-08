# August 7th 2017

Triage week, irc meeting in #rust-dev-tools

## Notes

### Triage

#### Stabilise `-Zsave-analysis`

* https://github.com/rust-lang/rust/issues/43606
* Manish also mentioned `-Ztimepasses`
* General support for stabilisation (but not from compiler team last week)
* Alternative is a rustc shim to allow such options for tools, nrc to investigate
* See https://github.com/nrc/rls-rustc

#### RFC: Add external doc attribute to rustc

* https://github.com/rust-lang/rfcs/pull/1990
* Discussed `#[doc(include = "docs/example.md")]` vs `#[doc = include_str!("docs/example.md")]`
* Preferrence for the former, even if the latter exists
* nrc to propose FCP

#### RFC: Custom attributes

* https://github.com/rust-lang/rfcs/pull/1755
* nrc to attempt to resurrect
* useful for stabilising rustfmt

#### Rust docker image

* https://github.com/docker-library/official-images/pull/3273
* sfackler is trying to get a Rust image upstreamed
* moved the repo to the nursery

### Tools check-in

* rustdoc issue triage - https://gist.github.com/QuietMisdreavus/3c0702517bea40711ed9baf513971143
* nrc found a terrible performance issue with Rustfmt, but topecongiro already has a fix in a PR <3
* nrc landed some perf improvements to the RLS, and plans to do a release of the vscode extension this week
*  Jetbrains announced that Rust support is now official in IntelliJ


## IRC log

Note times are early by about 30 minutes.

```
7:27 AM <•nrc> https://public.etherpad-mozilla.org/p/rust-dev-tools
7:27 AM <mw> o/
7:27 AM <•nrc> brson, jntrnr: ping for triage meeting
7:27 AM <•nrc> japaric is away
7:27 AM <jntrnr> here
7:28 AM <•nrc> ok, nominated issues - https://github.com/rust-lang/rust/issues?q=is%3Aopen+is%3Aissue+label%3AT-dev-tools+label%3AI-nominated
7:29 AM <•nrc> gh 43606
7:29 AM <Manishearth> can we also stabilize -Ztimepasses?
7:29 AM <•nrc> steve is away, unfortunately
7:29 AM <•nrc> anyone else have thoughts
7:29 AM <Manishearth> (I think the -Z flag is worth reviewing once)
7:29 AM <Manishearth> nrc: the json output uses the jsonapi spec and is backcompat, right?
7:30 AM <•nrc> compiler team were not keen about this - basically about adding another stability constraint to the compiler
7:30 AM <•nrc> thus searching for alternatives
7:30 AM <•nrc> Manishearth: -Ztimepasses is probably going away, it doesn't make sense in the incremental compilation world
7:31 AM <•nrc> Manishearth: json output for save-analysis
7:31 AM <•nrc> ?
7:32 AM <mw> can we emit that instability warning as a string in JSON data?
7:32 AM <•nrc> mw: which instability warning?
7:32 AM <misdreavus> i like the idea of making it `--emit analysis` but not guaranteeing the output
7:32 AM <mw> or don't many JSON parsers support comments anyway?
7:33 AM <•nrc> I think JSON doesn't support comments, in general?
7:33 AM <misdreavus> --emit mir is a thing, isn't that not guaranteed stable?
7:33 AM <mw> a warning like we have for MIR output
7:33 AM <•nrc> ah, I hadn't seen it
7:34 AM <•nrc> we could add a 'dummy' field to the top-level json struct
7:34 AM <•nrc> but parsers would just ignore it, so I don't know how helpful that would be in practice
7:34 AM <Manishearth> nrc: why doesn't it make sense?
7:34 AM <Manishearth> nrc: actually, so, for save-analysis I don't think we MUST have backcompat
7:34 AM <Manishearth> since we're bundling these tools with rustc anyway
7:35 AM <•nrc> because the phases are no longer well-defined
7:35 AM <Manishearth> nrc: I had a discussion with aturon about compiler flag stabilization during the epoch rfc (epoch introduces a backcompat hazard in the flags)
7:36 AM <Manishearth> and we sort of rested on "rustc flags may not be stable, cargo flags are", and rustc is more of a plumbing tool that other tools talk to
7:36 AM <Manishearth> so i think we can get away with not stabilizing the save-analysis format, provided all official consumers of save-analysis are rustup-able
7:37 AM <•nrc> Manishearth: back compat is more for if a random person builds a tool based on --emit analysis, rather than the tools we bundle with rustup 
7:37 AM <•nrc> I agree, back compat is not an issue internally
7:37 AM <Manishearth> nrc: sure, I think we can defer that till later; we can add a help message that --emit analysis is not stable as a format
7:38 AM <Manishearth> burn that bridge when we get to it kind of situation
7:38 AM <•nrc> cool
7:38 AM <•nrc> I'll write up a summary on the issue
7:39 AM <•nrc> I'll also play with the shim compiler idea before we go further towards stabilisation
7:39 AM <mw> how would that interact with rustdoc?
7:39 AM <•nrc> the shim?
7:39 AM <mw> wouldn't that mean that people have to compile their own compiler again?
7:39 AM <mw> yes
7:40 AM <•nrc> they would only have to re-compile the shim, which would link against the dynamic libs they already have
7:40 AM <•nrc> so it would be a few seconds build, not an hour and they wouldn't need the Rust repo
7:40 AM <•nrc> so, in theory, it sounds ok
7:40 AM <mw> that sounds like a good solution then
7:40 AM <•nrc> I'm a little unsure about the practice
7:41 AM <•nrc> yeah, if it works, I think it is a good idea!
7:42 AM <•nrc> there's just one PR, and that is waiting on the author now
7:42 AM <•nrc> RFCs, there is still 1990
7:42 AM <•nrc> https://github.com/rust-lang/rfcs/pull/1990
7:43 AM <•nrc> lets discuss that briefly again
7:43 AM <misdreavus> ah yeah
7:43 AM <•nrc> to summarise, this is for including a file in a doc comment
7:43 AM <•nrc> feeling on the RFC was positive
7:43 AM <•nrc> and we're basically down to syntax
7:44 AM <•nrc> #[doc(include = "docs/example.md")]
7:44 AM <•nrc> vs
7:44 AM <•nrc> #[doc = include_str!("docs/example.md")]
7:44 AM <imperio> I prefer the first one
7:44 AM <•nrc> the latter is expected to just work, the former is the RFC suggestion
7:45 AM <•nrc> so, to be clear, the choice is both, or just the second one
7:45 AM <•nrc> we could add the first, basically as sugar for the second
7:45 AM <•nrc> the question, I think, is whether the prettier syntax is worth having two different ways to do the same thing
7:46 AM <killercup> i fell like it comes down to whether we want to make this a prominent feature or one that feels a bit like a hack we get for free
7:46 AM <misdreavus> ^ yeah, that's basically my comment
7:46 AM <•nrc> so, I think I want to question the 'bit like a hack' framing
7:46 AM <mw> I'm fine with adding the first option as sugar for the second. it does look nicer.
7:47 AM <killercup> say, we change our default suggestion on how to write docs to always have a doc/ directory with an readme.md that should always be included as root module docs, then special syntax is fine
7:47 AM <killercup> if this is an edge case 2% of people use… meh
7:47 AM <•nrc> so usually, in language design when something like this works, we call it elegant and orthogonal, so I'm curious why people (and quite a few people, so I'm not saying you're wrong :-) ) think in this case it is hacky
7:47 AM <misdreavus> it feels like a happy accident
7:48 AM <mw> yes
7:48 AM <misdreavus> it would also be the only attribute that it would work in, right?
7:48 AM <killercup> you saw my very first comment in the thread that i've been trying to use the 'hack' (sorry, bad word probably) for years? :D
7:48 AM <misdreavus> (and the only macro that would work in it?)
7:49 AM <•nrc> misdreavus: I would expect it to work for most attributes and for all suitable macros, though it might be the only macro which actually makes sense in that position
7:49 AM <killercup> misdreavus: i'd expect it to work in all attr that expect a str literal
7:49 AM <misdreavus> then that's a way wider scope than what this rfc is suggesting
7:49 AM <misdreavus> unless getting it to work in #[doc] lets it work elsewhere too
7:49 AM <killercup> did i misunderstand? include_str == reject rfc
7:50 AM <•nrc> misdreavus: yes, but I think that it naturally falls out from the macro design
7:50 AM <•nrc> killercup: that is a somewhat open question
7:50 AM <•nrc> I think we want the feature, for sure, in some form
7:50 AM <•nrc> and what we do with the RFC in the include_str is a procedural question
7:51 AM <•nrc> but I'd like to focus on the syntax question first
7:51 AM <misdreavus> would making include_str work require another rfc to "fix the bug"?
7:51 AM <•nrc> misdreavus: no, I think it is just an implementation issue
7:51 AM <•nrc> ok, let me rephrase
7:52 AM <misdreavus> (that was under the assumption that we would rework the rfc or close it)
7:52 AM <•nrc> given that we expect the second option to exist, is there anyone who objects to supporting the first option as well?
7:52 AM <•nrc> (because it seems quite a few people want the first option)
7:52 AM <misdreavus> i would prefer the first option to exist
7:53 AM <•nrc> ok, so I'll take silence as acquiescence from everyone else :-) 
7:54 AM <misdreavus> hehe
7:54 AM <aret> okay
7:54 AM <killercup> :+1:
7:54 AM <•nrc> I'll propose fcp with the current (first) syntax. Implementation-wise that will probably be sugar for the second syntax
7:54 AM <mw> sg
7:54 AM <misdreavus> (^^)b
7:55 AM <Manishearth> nrc: FWIW the custom driver plan forward for tools is a good one IMO
7:55 AM <Manishearth> provided we can tweak the compiler's libraries to be more amenable for this
7:55 AM <•nrc> Manishearth: cool
7:56 AM <•nrc> I have some thoughts, we should take later about amenability of compiler libs
7:57 AM <•nrc> I would like to resurect https://github.com/rust-lang/rfcs/pull/1755 - custom attributes soon, we want to stabilise Rustfmt this year and having attributes would be good for that
7:57 AM <aret> ok
7:57 AM <•nrc> I remember the RFC needed changes, but anyone has thoughts on custom attirbutes for their tools, please comment on that RFC or let me know
7:57 AM <•nrc> sfackler has made a Rust docker image - https://github.com/docker-library/official-images/pull/3273
7:58 AM <•nrc> sfackler: is there anything we should discuss regarding that?
7:58 AM <•nrc> I think it would be great to have
7:58 AM <sfackler> one of the docker people just replied and seems on board
7:58 AM <sfackler> they would prefer the repo to move over to rust-lang
7:59 AM <•nrc> I'd be happy to move it to the nursery
7:59 AM <•nrc> we generally don't move stuff straight to rust-lang, but we could discuss with core team if you think that is necessary?
7:59 AM <sfackler> sure, that would work as well I think. I think they just want it to be some official org
7:59 AM <•nrc> ok
8:00 AM <•nrc> any objections to moving it to the nursery?
8:00 AM <•nrc> or other thoughts?
8:00 AM <aret> Doesnt seem so
8:01 AM <killercup> i just skipped the docs. do we want to suggest running the user's app in the compiler container?
8:01 AM <killercup> i've chatted a bit with people running diesel apps on docker
8:02 AM <killercup> and the consensus was to have one image with the compiler and deps produce an image with only the app and runtime deps
8:02 AM <sfackler> the docs are copied over from the gcc and golang images - I think it's a kind of weird thing to do, but I think those examples are just there as the easiest possible thing to get people going
8:02 AM ⇐ aret quit (aret@moz-r73bcd.lbvlil.sbcglobal.net) Quit: Leaving
8:02 AM <sfackler> I'll ask the docker people about that though
8:03 AM <killercup> yeah, that's just a docs issue. the images themselves are fine :)
8:03 AM <sfackler> but I kind of want to adhere to the standard conventions of similar images to avoid surprises
8:04 AM <killercup> absolutely. i'd personally also wonder how i should install external deps (or which ones are already present) but that can all be documented elsewhere
8:05 AM <•nrc> so, we'd add a single repo for the docker image to the nursery, and the docs would live in the docker repo?
8:05 AM <•nrc> is there anything else we need to do from our end?
8:05 AM <sfackler> yep, that's my understanding
8:06 AM <•nrc> and will this be discoverable via docker or will we need to advertise it on our website, etc?
8:06 AM <sfackler> it'll be in hub.docker.com, like e.g. https://hub.docker.com/_/gcc/
8:06 AM <sfackler> I think official images rank first in search results, so if you search "rust" there you should end up seeing it
8:07 AM <•nrc> nice
8:08 AM <•nrc> sfackler: I think you should be able to do the move to nursery, but let me know if I need to do anything there
8:08 AM <sfackler> doing it now
8:08 AM <•nrc> :-)
8:08 AM <sfackler> i'll give Core admin rights on it
8:08 AM <•nrc> ok, tools check-in
8:08 AM <sfackler> done!
8:08 AM <•nrc> nice!
8:08 AM <•nrc> so brson and japaric are both away, so no update on the Xargo RFC
8:09 AM <misdreavus> about a week and a half ago, i started taking a machete to the A-rustdoc issues https://gist.github.com/QuietMisdreavus/3c0702517bea40711ed9baf513971143
8:09 AM <•nrc> I see from the rustdoc2 repo that there is steady progress
8:09 AM <misdreavus> i got distracted by solving some of them myself, but the first few are on there
8:09 AM <jntrnr> oh my
8:09 AM <•nrc> misdreavus: nice!
8:09 AM <misdreavus> hence my recent flurry of adding stuff to rustdoc :P
8:09 AM <killercup> misdreavus: <3
8:09 AM <misdreavus> gotta make steve's job harder for the rewrite :3
8:10 AM <•nrc> I found a terrible performance issue with Rustfmt, but topecongiro  already has a fix in a PR <3
8:11 AM <•nrc> I landed some perf improvements to the RLS, and plan to do a release of the vscode extension this week
8:11 AM <•nrc> and Jetbrains announced that Rust support is now official
8:11 AM <•nrc> in IntelliJ
8:12 AM <•nrc> anyone else have any news?
8:12 AM <killercup> (works really well, btw!)
8:14 AM <•nrc> ok, last item on the agenda is next weeks topic
8:15 AM <•nrc> I'd like to talk Rustup, but brson is not here to approve
8:15 AM <•nrc> anybody got anything else they think is ripe for an agenda meeting next week
8:16 AM <•nrc> ok, I'll follow-up with brson and email an agenda topic
8:16 AM <•nrc> anyone got anything else we should cover?
8:18 AM <•nrc> ok, meeting adjourned :-)
8:18 AM <•nrc> Thank you everyone!```
