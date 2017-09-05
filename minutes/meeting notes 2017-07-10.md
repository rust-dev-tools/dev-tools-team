# July 10th 2017

Triage week, irc meeting in #rust-dev-tools

## IRC log

```
7:59 AM <•nrc> so, nominated issues - just one - https://github.com/rust-lang/rust/issues/26338
7:59 AM <•nrc> I don't have much of an opinion on this
8:00 AM <•nrc> ping acrichto ^ seems like a tooling/platform question, if such a team existed
8:00 AM <•nrc> anyone else have an opinion on that?
8:00 AM <acrichto> seems fine to defer
8:00 AM <•nrc> nominated PRs: https://github.com/rust-lang/rust/pulls?q=is%3Aopen+is%3Apr+label%3AT-dev-tools
8:00 AM <acrichto> e.g. not super pressing
8:01 AM <brson> not very interesting to me
8:01 AM <•nrc> p-low by the sounds of it
8:02 AM → oli_obk_ joined (smuxi@moz-ppe75h.hsi6.kabel-badenwuerttemberg.de)
8:02 AM <llogiq> is anyone actually using this?
8:02 AM <misdreavus> presumably so, since it was sourced from an SO question
8:02 AM <•nrc> https://github.com/rust-lang/rust/pull/42684
8:02 AM <•nrc> brson: could you say what was discussed about this before please?
8:03 AM <•nrc> PR adds #[serial] to sequentialise tests
8:03 AM ⇐ oli_obk quit (smuxi@moz-ppe75h.hsi6.kabel-badenwuerttemberg.de) Ping timeout: 121 seconds
8:03 AM <•nrc> seems like a good idea to me
8:03 AM <•nrc> at least superficially
8:03 AM <steveklabnik> better than ruby's
8:03 AM <steveklabnik> heh
8:04 AM <steveklabnik> i wish we had someone focused on custom test runners :(
8:04 AM <•nrc> +1
8:04 AM <brson> we have not been very sympathetic in the past to code that is not threadsafe
8:04 AM <brson> it's easy to hack around it with a mutex
8:04 AM <brson> which is the recommendation today
8:04 AM <brson> or to write better code
8:04 AM <•nrc> is the recommendation documented?
8:05 AM <brson> surely not
8:05 AM <mw> seems a bit ad hoc
8:05 AM <•nrc> lol
8:05 AM <killercup> so a #[serial] test would make the test runner schedule it when no other tests are running (prevent other test threads from starting) but still run tests in random order?
8:05 AM <•nrc> killercup: yes, I believe so
8:05 AM <mw> the #[serial] attr, I mean
8:05 AM <brson> this mostly comes up with bindings to bad c libraries
8:05 AM <killercup> i think 'serial' may imply an ordering
8:05 AM <brson> though rustup also has serial testing because it interacts with the environment in horrible ways
8:05 AM <•nrc> mw: yeah, I agree - although all our test attributes are a bit ad hoc IMO
8:06 AM → oli_obk joined  ⇐ oli_obk_ quit  
8:06 AM <•nrc> yeah, the RLS had serial testing for a while because it wrote to a shared directory on disk
8:06 AM <Diggsey> brson: clearly we should run rustup tests in fresh docker instances :P
8:06 AM <brson> not having #[serial] is a strong signal to think harder about your architecture
8:06 AM <•nrc> it was annoying to tell users they had to run `cargo test` with an env var
8:07 AM <brson> we might expect people to just punt on bad code and serialize their tests if it's too easy
8:07 AM <•nrc> brson: how would one use a Mutex to solve this? Tests have no shared state, aiui
8:07 AM <brson> static mutex
8:08 AM <•nrc> do we guarantee to always run tests in the same process? I guess in practice we do
8:08 AM <mw> I'm leaning towards not wanting to support this "officially"
8:09 AM <•nrc> mw: this == serial?
8:09 AM <mw> yes
8:09 AM <brson> nrc: interesting question
8:09 AM <brson> there are cases where we might expect the tests to be run out of process
8:09 AM <brson> i think at this point we would expect them all to be in one process though
8:09 AM <brson> because of that issue
8:09 AM <•nrc> ok
8:10 AM <•nrc> I feel we should really document the Mutex thing
8:10 AM <brson> maybe this is worth an rfc?
8:10 AM <misdreavus> OTOH, people ask for a way to add test setup code all the time
8:10 AM <•nrc> steveklabnik: is it covered in the book by any chance?
8:10 AM <steveklabnik> it's not
8:10 AM <mw> custom test runners, as steveklabnik mentioned, seem like a good idea to give control over all of these aspects
8:10 AM <misdreavus> depending on how this is specified or implemented, this could be a way to hack that in
8:10 AM <steveklabnik> book is pretty bare-bones here
8:10 AM <misdreavus> for better or worse
8:10 AM <brson> a lot of our answers to any improvements to tests are to wait for custom test runners
8:11 AM <•nrc> yeah
8:11 AM <brson> though those are always on the horizon and nobody working on them
8:11 AM <sfackler> I think custom test runners wouldn't be much more than a crate level attribute proc-macro
8:11 AM <steveklabnik> sfackler: yeah that's what stainless is basically
8:11 AM <steveklabnik> well, it's not a proc-macro
8:11 AM <•nrc> I would feel better about a better test framework rather than adding something ad hoc here (given the work around)
8:11 AM <mw> +1
8:12 AM <killercup> do you think custom test runners are a thing we should be working on this year?
8:12 AM <killercup> i don't think there is an up-to-date rfc or anything
8:12 AM <misdreavus> i fear saying "wait for custom runners" would be viewed as a way to defer discussion indefinitely
8:12 AM <steveklabnik> i know of one community member who is very interested in them
8:12 AM <steveklabnik> but hasn't really found the time to truly dig in yet
8:13 AM <misdreavus> especially if there's no current action on it
8:13 AM <•nrc> I think we should be working on them soon, not sure about this year (given other priorities)
8:13 AM <•nrc> but testing/benching def needs some love
8:13 AM <killercup> steveklabnik: the person who wrote the tap-json rfc?
8:13 AM <steveklabnik> killercup: nope; maintainer of rspec
8:13 AM <steveklabnik> (in ruby)
8:13 AM <killercup> steveklabnik: ah cool
8:13 AM <steveklabnik> he has resorted to testing his rust code with ruby and isn't pumped about it haha
8:14 AM <steveklabnik> he's interested in doing the work it's just, life happens and obviously that's already a huge OSS commitment
8:16 AM <killercup> do you think proc macros will allow writing a #[test]/describe! think in the future in an external crate?
8:16 AM <killercup> s/think/thing
8:17 AM <•nrc> ok
8:17 AM <•nrc> https://github.com/rust-lang/rust/pull/42973
8:17 AM <matklad> re test framework: would be really useful for custom test reporters in IDEs.
8:17 AM <brson> yes
8:18 AM <matklad> currently not possible, because you can't both run tests in parallel and collect stdout/stderr.
8:18 AM <brson> matklad: current plan is to use tap-json for that
8:18 AM <llogiq> also things like mutation testing.
8:18 AM <matklad> That would be perfect!
8:18 AM <killercup> oh yes
8:18 AM <steveklabnik> i feel like this is a bug fix
8:18 AM <•nrc> #42973 seems like a bug fix, but it is a breaking change, technically
8:18 AM <steveklabnik> every bug fix is a breaking change in that sense
8:18 AM <killercup> nrc: so i was mildly surprised you consider the offsets stable api but i can see why
8:18 AM — steveklabnik summons that xkcd
8:19 AM <•nrc> well, all of JSON errors is a stable API
8:19 AM <•nrc> afaik no-one is using these offsets
8:19 AM <mw> is there any way to actually use byte_pos now?
8:19 AM <•nrc> anyone know of users?
8:19 AM <oli_obk> were the offsets usable before?
8:19 AM <mw> it's relying on compiler-internal information
8:19 AM <•nrc> mw: if you use libsyntax, I would think
8:19 AM <•nrc> to get a codemap
8:19 AM <killercup> is anyone we know using these offsets? i think they also need to use the same crate-as-bytes representation internally
8:19 AM <mw> nrc: right
8:19 AM <brson> seems ok to break if coordinate with all the users, who should be only a handful
8:20 AM <•nrc> I'm not aware of any users
8:20 AM <•nrc> rls and rustw only use the line/column info
8:20 AM <killercup> i only know i wrote a workaround for rustfix :D
8:21 AM <mw> +1 doing this
8:21 AM <•nrc> any ideas for finding users, if there are any?
8:21 AM <matklad> IJ uses lines&cols as well
8:21 AM <mw> the majority of rust-tooling is yet to be written ;)
8:21 AM <oli_obk> A quick google for "byte_start rust" gave only killercup as a user, everything else was json dumps or internals docs
8:22 AM <killercup> https://github.com/search?l=&q=byte_start+language%3ARust&ref=advsearch&type=Code&utf8=✓
8:22 AM <matklad> langauge:Rust should not be there probably, it's JSON
8:23 AM <killercup> true. still a bunch of people writing their DiagnosticSpan 
8:23 AM <matklad> (at least, we read, but don't use `byte_start` from Kotlin)
8:23 AM <•nrc> https://github.com/rust-lang/rust/pull/43067
8:25 AM <•nrc> there do seem to be a couple of legit users there
8:25 AM <killercup> +1, especially: "they're in format of linker flags"
8:25 AM <•nrc> neither looks maintained though
8:26 AM <llogiq> @killercup we used to use the positions in clippy (ages ago) to point to parts of string literals.
8:26 AM <killercup> llogiq: oh, what are you using now? (and which link deals with string literal _parts_?)
8:26 AM <killercup> s/link/lint
8:28 AM <•nrc> so, the issue with #43067 is that there may be tools using the current format
8:28 AM <•nrc> that blocked 40076
8:28 AM <•nrc> assuming nothing has changed, it should probably block this too
8:29 AM <•nrc> (the JSON format PR was closed, afaik, nothing else similar has landed)
8:30 AM <steveklabnik> so
8:30 AM <steveklabnik> alex said that
8:30 AM <steveklabnik> but then rillian said "yeah that's me"
8:30 AM <steveklabnik> and that he wasn't using it anymore?
8:30 AM <mw> could we add an option to not show this note?
8:32 AM <•nrc> we could add an option to show the old format, so any tools that do exist can use it?
8:32 AM <•nrc> I feel like this doesn't need to be opt-in if rillian is the only *known* user
8:33 AM <mw> seems ok to me
8:33 AM <•nrc> it does seem like a real improvement and would be a shame to punt it if there are only speculative users
8:33 AM <•nrc> acrichto: thoughts?
8:33 AM <mw> if we do this though, we should add an option that takes the format as an argument
8:34 AM <•nrc> mw: do you have a suggestion for an option name?
8:34 AM <acrichto> this is for the listing staticlibs?
8:34 AM <acrichto> (sorry I'm not actively watching this)
8:34 AM <•nrc> yes
8:35 AM <acrichto> is the proposed way to close the issue understood?
8:35 AM <mw> nrc: nope, unless --print options can be parameterized
8:36 AM <•nrc> acrichto: istm there are two alternatives - we block this on some tool-friendly format (e.g., JSON) or we accept with an opt-in to get the old format back
8:36 AM <mw> --print staticlib-deps-legacy-format :)
8:36 AM <acrichto> yes, and to me at least those seem isomorphic in the amount of work involved?
8:37 AM <acrichto> e.g. let's just add `--emit link-info=path/to/info` or something like that
8:37 AM <acrichto> and that is just a bunch of linker flags in there
8:37 AM <•nrc> adding a new format seems more work than keeping an existing one, but otherwise, yes
8:38 AM <•nrc>  `--emit link-info=path/to/info`would give the old info or the new one?
8:39 AM <acrichto> some new format
8:39 AM <acrichto> just lnker flags in a file
8:39 AM <acrichto> which you can cat into your build system
8:39 AM <•nrc> isn't the problem this PR is trying to solve that the output from a regular build is annoyingly verbose? That would not solve that issue
8:40 AM <acrichto> if we add `--emit` we change the default output
8:40 AM <acrichto> we have to have some solution for these authors
8:41 AM <•nrc> so the new format would be opt-in, not opt-out
8:41 AM <acrichto> no I think we could change the default
8:41 AM <mw> and the old output would go away?
8:41 AM <acrichto> anyone using this is doing build system integration, which means they've already pinned their rustc version
8:42 AM <acrichto> once we have a machine readable format I think we could do w/e we want with the default compiler output
8:43 AM <•nrc> so, assuming the current output is machine-readable enough, the proposal is we make that available with an --emit option, and change the default output to this PR?
8:43 AM <mw> so, to be clear: by default there is no output at all, but one can get the information via --emit, if needed
8:43 AM <acrichto> er no, what I'm saying as I've proposed long ago --
8:43 AM <acrichto> a) add `--emit link-info` which is just a file of linker flags
8:43 AM <acrichto> b) change the default output to do w/e we want, but we shoudl still have it
8:44 AM <acrichto> and that's it
8:44 AM <acrichto> rillian implemented (a) a long time ago and it just never landed
8:44 AM <acrichto> all we need to do is revive that pr
8:45 AM <mw> but one point of critique was that one cannot get rid of this output, or am I misreading something here?
8:45 AM <oli_obk> we could add `--emit link-info=none`
8:45 AM <•nrc> I think we could, if there is a suitable (i.e., machine readable) replacement
8:45 AM <acrichto> I think getting rid of the output easily isn't something we should try to cater to
8:46 AM <acrichto> this is valuable output that needs to be integrated *somehow*
8:47 AM <•nrc> hmm, so does the output of this PR match what you would expect in a " file of linker flags", other than not being in a file?
8:47 AM <acrichto> I haven't read this PR cloessly enough to see
8:47 AM <mw> acrichto: but wouldn't that integration happen via --emit link-info? or are you worried about discoverability?
8:47 AM <acrichto> mw: yeah that's what I'm thinking
8:48 AM <mw> acrichto: discoverability?
8:48 AM <acrichto> er sorry, yes integration via --emit link-info
8:48 AM <acrichto> and then when you pass --emit link-info we don't print anything
8:49 AM <mw> ok, ... --emit link-info=none seems like a nice way to opt out then
8:49 AM <mw> as oli_obk suggested
8:50 AM <•nrc> so, then this PR seems a step in that direction in that it changes the format to the one acrichto wants, although it doesn't emit to a file
8:50 AM <•nrc> can we accept this and leave comments about the next steps?
8:50 AM <acrichto> er no, we shoudl not accept this pr
8:50 AM <•nrc> the burden for tools would be that they have to adapt, but the info is still there and in an easier to digest format
8:50 AM <acrichto> we need a solution for poeple using the output today
8:51 AM <acrichto> nvmd I don't really mind too strongly
8:51 AM <mw> I'd prefer asking for the PR to be changed
8:52 AM <mw> to implement --emit link-info 
8:52 AM <mw> maybe with a link to rillian's closed PR?
8:52 AM <•nrc> I don't think I understand - under your proposal they would need to change to look at the specified file and we'd remove the current output. Or is there a time gap - so --emit link-info exists for a while and *then* we remove the current output
8:52 AM <•nrc> aiui, this PR does the same as the closed PR except the output is in a note, not in a file?
8:54 AM <mw> either we should keep the current output for a while and *add* the --emit option, so we are backwards compatible
8:54 AM <•nrc> ok, we're running out of time - I'll comment on the PR, acrichto/mw can correct me if I get something wrong
8:54 AM <•nrc> quickly, RFCs
8:54 AM <•nrc> https://github.com/rust-lang/rfcs/pull/1946 is just waiting on brson for FCP
8:54 AM <•nrc> https://github.com/rust-lang/rfcs/pull/1990
8:54 AM <•nrc> sounds like the alternative would work
8:54 AM <•nrc> should we close this RFC?
8:55 AM <•nrc> I guess we could ping the author and see if they want to rewrite or close
```