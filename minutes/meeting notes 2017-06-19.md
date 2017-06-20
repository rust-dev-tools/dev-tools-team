# June 19th 2017

Triage week, irc meeting in #rust-dev-tools

## Notes

### Triage

* https://github.com/rust-lang/rust/issues/42667

We decided that libsyntax should not be included with the rust-src component.
We'd be happy to create a new component rust-src-internal which includes the
compiler source, but it is not a priority. The reason for multiple components is
size.

There was some discussion of the role of the source component: whether users
should expect to be able to use it to rebuild the distro from source, or whether
it is there purely to support tools (Racer, RLS, IDEs).


* https://github.com/rust-lang/rust/issues/42617

After some time, we did not reach a conclusion. We agreed that *if* we support
doc comments on statements (and we acknowledge that removing the feature would
be effort, but not impossible) then the current situation with tests is bad. We
would prefer to run tests. If that is not possible, we should lint against them.
Next step is to decide if we want such doc comments at all.

I think it is interesting to consider the role of doc comments - are they purely
input for Rustdoc? Or are they a canonical form in their own right which can be
interpreted by Rustdoc or other tools.

* https://github.com/rust-lang/rfcs/pull/1946

Ready for FCP \o/


### Tools check-in

* Rustfmt using RFC style and libsyntax
* RLS supports formatting
* Rustup added support for version files
* teddriggs has been doing lots of good stuff on Racer
* New rustdoc is coming along - something to show soon
* Clippy moved to linting the markdown of doc comments with pulldown-cmark


## IRC log

```
8:00 AM <•nrc> this week is a triage week in irc
8:00 AM <•nrc> https://public.etherpad-mozilla.org/p/rust-dev-tools
8:01 AM <•nrc> There are no nominated issues this week
8:01 AM <•nrc> I'm just having a quick look through all the tagged issues: https://github.com/rust-lang/rust/issues?q=is%3Aopen+is%3Aissue+label%3AT-dev-tools
8:01 AM <mw> o/
8:02 AM <•nrc> https://github.com/rust-lang/rust/issues/42667
8:02 AM <misdreavus> o/
8:03 AM <•nrc> I would say the source bundle should not include compiler sources such as libsyntax because the audience is so small
8:03 AM <jntrnr> looks like by design?
8:04 AM <jwilm> Agree that libsyntax shouldn't be included
8:04 AM <japaric> is there a tool that needs libsyntax *source*?
8:04 AM <killercup> hm, i'd have guessed its the rustc source package
8:04 AM <•nrc> japaric I don't think so
8:04 AM <killercup> otherwise it should be rust-std-soruce or something like that
8:04 AM <misdreavus> what's the purpose of the rust-src package, again?
8:04 AM <•nrc> I think it is only useful for rustc devs, and they work on the source code anyway
8:05 AM <•nrc> misdreavus: for Racer, basically
8:05 AM <jwilm> If it ever became a need, another component could maybe be added like rust-src-internals
8:05 AM <•nrc> aiui
8:05 AM <oli_obk> misdreavus: racer being able to generate hints and xargo being able to generate libstd
8:05 AM <misdreavus> so mainly for libstd and its deps, then
8:05 AM <misdreavus> at least, that was its intent
8:05 AM <jwilm> misdreavus: previously, std completions from racer required manually downloading and installing rust src package
8:06 AM <misdreavus> i tried using racer in those days ._.
8:06 AM <•nrc> brson: should we close that issue?
8:07 AM <brson> if you think it's wontfix sure
8:07 AM <brson> i'm less certain
8:07 AM <brson> people do link against the rustc sources
8:07 AM <brson> this person did
8:08 AM <•nrc> brson: do you know why?
8:08 AM <brson> no
8:08 AM <brson> i might say that if we are distributing a lib in the rustlib dir then it should hvae source
8:09 AM <•nrc> I'd like to understand the use case better - why not link the libs? Or if they *need* source, why not `git pull` it?
8:09 AM <brson> i mean, personally, i was pretty happy that it was literally the entire rust repo in the source package and could be used to build rust...
8:10 AM <oli_obk> nrc: because it's hard to get the correct commit maybe
8:10 AM <•nrc> I guess there are two very different roles for the source package
8:10 AM <mw> what would be the downside of providing all the source?
8:10 AM <•nrc> oli_obk: yes. I want to know why that is important for someone
8:10 AM <•nrc> mw: size - it made a big difference
8:10 AM <oli_obk> nrc: code completion while building stuff depending on rustc internals
8:11 AM <•nrc> 36MB vs 1.3MB - https://github.com/rust-lang/rust/pull/41546
8:11 AM <mw> nrc: that *is* a big difference
8:11 AM <oli_obk> what about the `rust-src-internals` idea? Opt-in for everyone who needs it
8:11 AM <jntrnr> at that new size, shouldn't we just pull it in always?
8:12 AM <•nrc> jntrnr: pull what in?
8:12 AM <•nrc> with what?
8:12 AM <mw> rust's source is 247MB uncompressed? that seems much
8:12 AM <jntrnr> rust-src (isn't that what we were talking about?)
8:12 AM <misdreavus> i.e. make rust-src a default component in rustup?
8:13 AM <jntrnr> possibly, yeah
8:13 AM <•nrc> ah, right
8:13 AM <jntrnr> the other idea might be also to work with it in the compressed version rather than requiring it to be decompressed
8:13 AM <•nrc> I think that a separate issue
8:13 AM <jntrnr> it is
8:13 AM <jntrnr> I'm giving two points :)
8:13 AM <•nrc> (but agree we might want to)
8:14 AM <•nrc> mw: I assume that includes LLVM maybe?
8:14 AM <mw> so, from what I can see, rust's source is "only" 40 mb uncompressed
8:14 AM <•nrc> agree it seems pretty big
8:14 AM <mw> yes, llvm accounts for the rest
8:14 AM <•nrc> which  if the intention is that we want to be able to build from source, then we need to include
8:14 AM <•nrc> but if we want to enable code completion etc., we don't
8:15 AM <mw> compressed w/o llvm it's 13 MB
8:15 AM <mw> tar.gz
8:15 AM <•nrc> there was some talk of making rustc builds work with a compiled llvm which would spare us distributing that with the source?
8:15 AM <mw> which is still a lot more than 1.3 MB
8:16 AM <•nrc> so, I think if we want to distribute compiler source it should be an additional component
8:16 AM <•nrc> brson: do you agree ^?
8:16 AM <mw> sgtm
8:17 AM <brson> sure
8:17 AM <sfackler> you could leave the source zipped rather than tarballed and read directly out of it - that's what Java tooling tends to do
8:17 AM <sfackler> since .zip gives you access to individual files
8:19 AM <•nrc> I would be more concerned about the download size than the size on disk
8:19 AM <killercup> sfackler: i' pretty sure i've done that with tgz as well, tar tz or something
8:19 AM <sfackler> tar doesn't have a file index so you'd need to build one up in memory
8:20 AM <•nrc> as for whether we *should* offer rust-src-internals - I think we should, but I wouldn't be in a hurry to implement it, so p-low?
8:20 AM <•nrc> brson, acrichto ^
8:21 AM <brson> sgtm
8:21 AM <•nrc> ok
8:21 AM <•nrc> https://github.com/rust-lang/rust/issues/42617 had a bit of discussion
8:22 AM <•nrc> should we (a) ban doc comments on statements
8:22 AM <•nrc> or (b) run doc tests on such comments
8:22 AM <brson> omg
8:22 AM <brson> looks bonkers to me
8:23 AM <killercup> i was pretty surprised that you could add doc comments to those items tbh
8:23 AM <misdreavus> the reasoning to have them in the first place was so that tools like racer could pick up on them
8:23 AM <misdreavus> killercup: same
8:23 AM <•nrc> it seems like a natural (but surprising) extension of allowing attributes on statements
8:24 AM <brson> hm yeah, do these parse as attributes today?
8:24 AM <killercup> nrc: ah, that pov makes sense
8:24 AM <jwilm> Sounds like there's precident for other language tooling supporting it
8:24 AM <oli_obk> My vote is for allowing it but linting doctests, since they make little sense there.
8:25 AM <•nrc> IMO we should not ban - it would be a back incompat change and there should be a higher bar for that than 'this is a bit weird'
8:25 AM <misdreavus> the implementation note is whether these items are even visible at a level where rustdoctest could scan for the doc attributes
8:25 AM <•nrc> re doctests we could: run them, lint against them, ignore them (what we currently do, aiui)
8:26 AM <misdreavus> these doctests are currently ignored, correct
8:26 AM <•nrc> misdreavus: that wouldn't apply to a lint, though, right?
8:26 AM <misdreavus> nrc: yeah, if these were linted then the lint would just work as normal
8:26 AM <•nrc> ping steveklabnik who had opinions on thread
8:26 AM <brson> i think it's very weird to have documentation that is not displayed in the documentation
8:26 AM <killercup> i would keep it, but _also_ run doc tests ideally, because… well, why not?
8:27 AM <oli_obk> brson: it's less documentation and more commenting as I see it
8:27 AM <killercup> oli_obk: think of a doc test like "when we call it like this, this var gets set to 42, and thus the result is bla"
8:27 AM <brson> but these are doc-comments, designed specifically for display in rustdoc
8:27 AM <killercup> (in form of an assert)
8:27 AM <•nrc> brson: I think that depends on what the definition of "the documentation" is - they are disaplyed in some views of the documentation (e.g., in IDEs)
8:27 AM <misdreavus> the major note if we allow and run these tests, they only have access to public items from the crate
8:27 AM <oli_obk> killercup: interesting idea
8:27 AM <misdreavus> just as if it were a doctest on a private item
8:27 AM <jwilm> brson: they may not be displayed today, but it would open the door for some sort of side-by-side commented and code view in rustdoc
8:28 AM <misdreavus> some kind of rustw lens? i like it
8:28 AM <killercup> oli_obk: i was first thinking of it like some form of literate programming
8:28 AM <brson> ok. if this is considered docs then i think rustdoc should test it
8:28 AM <brson> since the purpose of doctesting is to make the docs present working code
8:29 AM <•nrc> sounds like implementation might be tricky though
8:29 AM <misdreavus> ^
8:30 AM <misdreavus> doubly so in rls-rustdoc
8:30 AM <misdreavus> rustc-rustdoc *may* be able to scan for it, if it can see items within function bodies
8:30 AM <•nrc> in particular, what is the env for executing the code?
8:31 AM <misdreavus> the cleaned items probably don't hold them, but that could be changed
8:31 AM <•nrc> should code above the comment be executed first?
8:31 AM <steveklabnik> oh hey sorry why does my friggin' IRC client not ping me
8:31 AM <misdreavus> imo, it's a doctest, it should be run like any other doctest
8:31 AM <misdreavus> i.e. no env but the crate itself
8:31 AM <•nrc> that would be simpler
8:32 AM <steveklabnik> my feeelings are basically "I wish this was never allowed but oh well"
8:32 AM <•nrc> yeah
8:33 AM <brson> it's possible to get rid of this feature without breaking code
8:33 AM <brson> just parse it as a warning forever
8:33 AM <brson> "looks like you wrote a doc comment here"
8:33 AM <•nrc> how about - we should run the tests, but if that is not possible (due to implementation) then we should lint against it
8:34 AM <misdreavus> sgtm
8:34 AM <steveklabnik> that seems reasonable
8:34 AM <killercup> nrc: sgtm
8:34 AM <brson> My preference is to stop parsing these as attributes and turn them into a warning forever.
8:34 AM <•nrc> anyone else agree with brson?
8:34 AM <brson> it seems like a really marginal feature
8:34 AM <brson> why add the complexity
8:34 AM <oli_obk> brson: the complexity is already there
8:34 AM <steveklabnik> brson: i'd love to do that if we thought it was feasable
8:35 AM <brson> oli_obk: but it doesn't work
8:35 AM <brson> it is half-implemented
8:35 AM <oli_obk> brson: only the doc tests
8:35 AM <brson> exactly
8:35 AM <•nrc> I'm on the fence - it seems like a marginal feature, but there is some utility (for IDEs, etc) and precedent from other langs (apparently)
8:35 AM <killercup> i dont hold a strong opinion on this. i wont be sad to see this go either way, and i probably wont ever use it…
8:36 AM <•nrc> and it does just fall out from attributes on statements
8:36 AM <misdreavus> i stated my opinion in-thread - i'm in favor, if racer/rls could pick up on them
8:36 AM <killercup> which other lang actually does this? let gather some facts
8:36 AM <•nrc> from thread: "JS and TypeScript"
8:37 AM <mw> when would this be useful? mostly for very large functions that don't fit on one screen?
8:37 AM — jntrnr perks up at mention of TypeScript
8:37 AM <killercup> nrc: there is no single canonical js api doc renderer, so i'll have a look at tsdoc
8:37 AM <oli_obk> mw: it would be useful for every variable that you put a comment infront normally
8:37 AM <•nrc> mw: yeah, I think so
8:38 AM <mw> oli_obk: but in which cases don't you just see the comment right there?
8:38 AM <killercup> there is no official ts api doc thing? :O
8:38 AM <oli_obk> true...
8:38 AM <•nrc> mw: I imagine more of a use case for nested functions, rather than statements?
8:39 AM <•nrc> I sometimes see large functions with a few nested functions, the uses of the fns are then a long way from the defs
8:39 AM <misdreavus> nrc: i hadn't even considered that :o
8:39 AM <mw> nested functions are not variables, right?
8:39 AM <mw> but closures are...
8:39 AM <•nrc> I'm assuming this applies to nested items as well as statements
8:40 AM <•nrc> this does seem really niche
8:40 AM <•nrc> the argument for seems to me that it is natural and not worth the effort to ban
8:41 AM <misdreavus> documenting nested functions can be fraught if i stick to my "no private items" rule in doctests
8:41 AM <•nrc> like it is a bit weird, but it is does not seem problematic
8:41 AM <misdreavus> the person who posted that issue is in #rust, if we'd like them to weigh in in here
8:41 AM <misdreavus> they asked about it while working on racer
8:42 AM <•nrc> ok, I think we've spent enough time on this for now. Let's continue on thread and re-visit
8:42 AM <misdreavus> (^^)b
8:43 AM <•nrc> I'll write up later
8:43 AM <•nrc> there is only one PR and that is waiting on the author - https://github.com/rust-lang/rust/pull/42219
8:43 AM <•nrc> RFCs: https://github.com/rust-lang/rfcs/pulls?q=is%3Aopen+is%3Apr+label%3AT-dev-tools
8:44 AM → teddriggs joined (teddriggs@moz-9rq.lf7.79.208.IP)
8:44 AM <•nrc> 1946 looks ready for FCP
8:44 AM <killercup> finally!
8:44 AM <killercup> :D
8:44 AM <misdreavus> lol
8:45 AM <misdreavus> +1 from me about fcp
8:45 AM <killercup> so, yeah, from my end it's ready. i think i've covered a points made in the comments so far
8:46 AM <killercup> there hasn't been much discussion about the syntax, but i guess that's something we could also revisit later on if there are pressing issues
8:47 AM <•nrc> I thought there had been some? It looks fine to me now
8:47 AM <•nrc> I summoned rfcbot, lets see if it works....
8:47 AM <killercup> nrc: …lately ;)
8:47 AM <•nrc> 1990 - there is an alternative syntax proposed, not much feedback
8:47 AM <misdreavus> i do not appear to be in the rfcbot roster ;_;
8:48 AM <steveklabnik> :(
8:48 AM <•nrc> that may be a doc team issue?
8:49 AM <•nrc> although I should make t-dev-tools-doc exist for rfcbot too
8:49 AM <•nrc> ok, tools check-in
8:49 AM <•nrc> who has news about their tools?
8:50 AM <•nrc> rustfmt moved to the new RFC style and to using libsyntax rather than Syntex
8:50 AM <•nrc> formatting in the RLS is ON
8:50 AM <Manishearth> no news on clippy
8:50 AM <Manishearth> AFAICT clippy is just waiting for the RLS stuff to move forward 
8:50 AM <killercup> can i just say, i really enjoy the rfc rustfmt style? big thumbs up on that
8:50 AM <•nrc> and xanewok has been implementing more Cargo stuff (working with binary projects better, etc.)
8:51 AM <•nrc> killercup: thank :-)
8:51 AM <•nrc> s
8:51 AM <brson> rustup has a big new feature https://github.com/rust-lang-nursery/rustup.rs/pull/1172
8:51 AM <brson> a .rust-version file
8:51 AM <brson> looking for feedback
8:51 AM <steveklabnik> i should have some amount of rustdoc demo very soon; i ran into some issues at the end of the last week that meant i didn't ship it yet.
8:51 AM <steveklabnik> i also don't want to get TOO hyped about it, it's just barely a skeleton
8:51 AM <killercup> oh, fyi, clippy moved to linting the markdown of doc comments with pulldown-cmark this week
8:52 AM <steveklabnik> brson: i will check it out
8:52 AM <Manishearth> oh, yeah, that
8:52 AM <•nrc> oh, and RLS is learning about borrowing - nashen88 has been doing some cool stuff there
8:52 AM <jwilm> I have a bit of news about Racer
8:53 AM <jwilm> Recently, an awesome person named TedDriggs has started contributing many fantastic patches to the project
8:53 AM <killercup> brson: i like the feature, but i also don't want it in another file and have been thinking about suggesting using cargo.toml [metadata] in a few tools 
8:53 AM <jwilm> A PR was just opened for restricted visibility completions by him as well
8:53 AM <•nrc> nice!
8:53 AM <killercup> brson: but that's a whole other thing, and i'm looking forward to a .rust-version file in the meantime :)
8:53 AM <misdreavus> ^ teddriggs is the one who posted the "doc comments on items" issue
8:53 AM <jwilm> All around, really great contributor. His work is doing a lot to move Racer forward ;)
8:54 AM <jwilm> I just wanted to acknoledge him here since he is doing such awesome work
8:54 AM <•nrc> :-)
8:54 AM <misdreavus> <3
8:54 AM <steveklabnik> :D
8:54 AM <teddriggs> thanks jwilm! :) you're making me blush right now.
8:54 AM <jwilm> glad to have you on board :)
8:55 AM <•nrc> fitzgen did some work on bindgen to improve the contributor docs - he wanted feedback, so would be great if anyone wanted to go through them (I plan to do it too)
8:56 AM <teddriggs> the community has been great thus far. I've started looking at nrc's work in rls-analysis lately too; trying to understand the long-term direction (and RLS architecture) better.
8:56 AM <•nrc> ok, finally, topic for the focused meeting next week
8:56 AM <•nrc> (teddriggs: cool, please ping me if you have any questions)
8:56 AM <•nrc> anyone want to nominate something for next week?
8:57 AM <•nrc> if not, then I would like to nominate cross-compilation support and Xargo
8:57 AM <•nrc> does that sound good?
8:57 AM <•nrc> japaric?
8:57 AM <steveklabnik> :+1:
8:58 AM <oli_obk> :+1: for cross compilation
8:58 AM <japaric> nrc: sgtm
8:58 AM — mw is traveling next week
8:59 AM <steveklabnik> oh right
8:59 AM <steveklabnik> i am not sure when my flights are
8:59 AM <•nrc> if you are interested in the meeting, then please read the Xargo roadmap - https://github.com/nrc/dev-tools-team/blob/master/roadmaps/xargo.md
9:00 AM <•nrc> brson: will you be available?
9:00 AM <brson> yes
9:00 AM <brson> nrc: are you coming to the ww?
9:00 AM <killercup> oh, i've been meaning to look into getting `cross` (not xargo) working on macos, maybe we can talk a bit about that tool as well
9:00 AM <japaric> killercup: sure
9:00 AM <•nrc> brson: I am, I'm travelling on Sunday, so I'll be in SF on Monday
9:01 AM <•nrc> killercup: do you have a link?
9:01 AM <killercup> nrc: https://github.com/japaric/cross/
9:01 AM <•nrc> thanks!
9:02 AM <•nrc> OK, time's up, thank you everyone!
9:02 AM <brson> maybe we can also talk about refreshing smoke next week
```