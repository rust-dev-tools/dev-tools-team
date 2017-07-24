# July 24th 2017

Triage week, irc meeting in #rust-dev-tools

## Notes

### Triage

#### Move Rustdoc to a different phase in Rustbuild

* https://github.com/rust-lang/rust/issues/43284
* General approval
* Hope it might help with the Pulldown/Hoedown warning cycle PR

#### Remapping paths - stabilisation

* https://github.com/rust-lang/rust/issues/41555
* https://doc.rust-lang.org/unstable-book/compiler-flags/remap-path-prefix.html
* Allows re-mapping of paths in debuginfo and error messages. Useful for reproducible builds, among other things.
* General sense of approval for stabilisation
* Prefer adopting GCC's syntax for command line args, rather than current -Z flags.
* Stick with string matching (c.f., paths) due to possible platform-specific issues.

#### RFC 1946 - Rustdoc - links

* FCP is done, ready to merge \o/

### Tools check-in

* Rustdoc - v2 implementation continues, has some new contributors
* Rustdoc - Pulldown/Hoedown warning cycle still stuck from landing on technical issues
* Rustdoc - Misdreavus experimented with a Syn-powered Rustdoc - https://github.com/QuietMisdreavus/crystal-rustdoc
* Clippy moved to the nursery
* RLS VSCode extension is in the VSCode marketplace - https://marketplace.visualstudio.com/items?itemName=rust-lang.rust
* Performance improvements for the RLS
* New bindgen release coming up
* japaric has drafted a 'Xargo in Cargo' RFC


## IRC log

```
8:03 AM <•nrc> first thing on the agenda is https://github.com/nrc/dev-tools-team/blob/master/overview.md - I mentioned this before, but reminder that it exists. If you want to change or improve anything I've written, or fill in any of the gaps, that would be awesome
8:04 AM <misdreavus> just sent in a PR to add lines about rustdoc
8:04 AM <misdreavus> :P
8:05 AM <•nrc> the plan is that this will evolve into an official tools summary for the Rust website
8:05 AM <•nrc> thanks misdreavus !
8:05 AM <jwilm> nrc: thanks for the reminder; meant to add a note about YCM
8:05 AM <Manishearth> o/
8:05 AM <•nrc> jwilm: great - also want to check you're happy with my description of Racer
8:05 AM <jwilm> nrc: racer description was great
8:05 AM <jwilm> s/was/is/
8:06 AM <•nrc> cool, thanks for looking it over!
8:07 AM <•nrc> looking at the open issues...
8:08 AM <•nrc> https://github.com/rust-lang/rust/issues/43284 looks quite significant for Rustdoc folks
8:08 AM <•nrc> but it seems you've seen it and approve
8:08 AM <•nrc> it sounds like a great idea to me
8:08 AM <•nrc> only nominated issue is https://github.com/rust-lang/rust/issues/41555
8:09 AM <•nrc> mw: you want to give an introduction?
8:09 AM <mw> yes
8:09 AM <mw>  https://doc.rust-lang.org/unstable-book/compiler-flags/remap-path-prefix.html
8:09 AM <mw> here is the documentation for the feature
8:10 AM <mw> it allows to remap paths in debuginfo and error messages
8:10 AM <mw> which is useful for reproducible builds
8:10 AM — •nrc reads
8:10 AM <mw> the feature has been implement as a set of -Z flags for a while
8:10 AM <mw> the people that asked for it initially have been using it
8:11 AM <Manishearth> btw can we not remove -Z flags on stable?
8:11 AM <mw> seems to work for everyone
8:11 AM <Manishearth> I think -Zunstable-options -Zfoobar is okay to have
8:11 AM <•nrc> Manishearth: I think -Z are not usable on stable since the last release
8:11 AM <mw> the question here is whether to stabilize in the current form
8:12 AM <mw> the -Z flag would become a -C flag
8:12 AM <•nrc> mw: if we didn't use -Z or -C we could use a single argument rather than a from and a to arg
8:12 AM <•nrc> would that be better?
8:12 AM <mw> why would that make a difference?
8:12 AM <Manishearth> nrc: i know, I'm saying we shouldn't have done that
8:13 AM <mw> it would be nice if we could have just one argument,
8:13 AM <mw> but it's hard to separate the from and to part
8:14 AM <•nrc> hmm, I see
8:14 AM <•nrc> seems a bit fragile alternating arguments
8:14 AM <mw> fragile in what way?
8:15 AM <•nrc> re the string/component matching - mw - do you have any examples of where it might be less predictable?
8:15 AM <•nrc> fragile in that usually it is ok for tools to reorder CLI args and if that happens here you'll get incorrect results
8:15 AM <mw> Windows drive letters and UNC paths might become tricky
8:16 AM <•nrc> ok
8:16 AM <mw> ok, that's a good point
8:16 AM <mw> although positional arguments are not unheard of
8:17 AM <mw> and one can re-order quite a bit without changing semantics
8:17 AM <mw> but not arbitrarily, that's true
8:17 AM <mw> GCC uses a `=` to separate the from and the to part
8:18 AM <•nrc> and does GCC use strings or paths or something else?
8:18 AM <mw> strings
8:18 AM <mw> strings are very predictable, but can be more cumbersome
8:18 AM <•nrc> does Clang do the same thing?
8:19 AM <mw> I think it's CLI compatible with GCC
8:19 AM <•nrc> I imagine that this is mostly used by tools rather than humans so cumbersome is not too bad?
8:19 AM <mw> that is my guess too
8:19 AM <mw> it's a rarely used feature
8:20 AM <mw> with path components one has to analyze paths, possibly cross-platform
8:20 AM <mw> that is not easily done
8:20 AM <•nrc> my thoughts are that we should stick with strings and we should move to a -- flag which uses = like gcc/Clang - seems better than the current system and closer to prior art
8:20 AM <•nrc> other than that, I'm happy to stabilise
8:20 AM <•nrc> anyone else have thoughts on this?
8:20 AM <mw> I would be OK with that
8:21 AM <•nrc> (I'd also be happy to stabilise as a -C, but then we can't do the = thing (AIUI), which seems not as good)
8:21 AM <•nrc> oh and I think it should be one of the 'extended options' or whatever we call them
8:21 AM <mw> not sure if it would be possible
8:21 AM <•nrc> given that this is "rarely used"
8:22 AM <mw> agreed
8:22 AM <•nrc> '--help -v'
8:22 AM <•nrc> anyone else?
8:22 AM <misdreavus> nrc++
8:23 AM <•nrc> mw: could you write this up on the issue please?
8:23 AM <mw> will do
8:23 AM <•nrc> thanks!
8:23 AM <•nrc> there's a nominated PR - https://github.com/rust-lang/rust/pull/43067
8:23 AM <mw> do we do something like a final comment period?
8:24 AM <imperio> hu?
8:24 AM <imperio> that's strange
8:24 AM <•nrc> mw: I guess we should
8:24 AM <mw> nrc: ok
8:25 AM <misdreavus> looks like this pr was brought up two weeks ago?
8:25 AM <•nrc> we discussed 43067 last time
8:25 AM — •nrc un-nominates
8:25 AM <•nrc> there is some more discussion...
8:26 AM <•nrc> hmm, pnkfelix is away for a while now
8:26 AM <•nrc> so I guess we should find a new reviewer, anyway, I don't think we need to discuss here
8:26 AM <•nrc> I'll do something with that
8:27 AM <•nrc> RFCs
8:27 AM <•nrc> Woo! 1946 is done! I'll merge that later
8:28 AM <misdreavus> \o/
8:28 AM <•nrc> 1990 has a new comment
8:28 AM <killercup> \o/
8:28 AM <•nrc> Rustdoc folk should keep an eye on that thread
8:29 AM <•nrc> so before we do the tools checkin - I want to confirm a topic for next week
8:29 AM <•nrc> I'd like to discuss a team roadmap
8:29 AM <imperio> we are
8:29 AM <•nrc> is that OK with everyone?
8:29 AM <killercup> +1
8:29 AM <misdreavus> (^^)b
8:29 AM <mw> +1
8:30 AM <•nrc> imperio: 👍
8:30 AM <imperio> nrc: ?
8:30 AM <•nrc> in reply to "we are"
8:30 AM <imperio> ah ok
8:31 AM <•nrc> re roadmap meeting, I'll come up with an agenda and email it around later this week
8:31 AM <•nrc> if anyone has anything they want to cover let me know
8:31 AM <•nrc> ok, tools check-in
8:32 AM <imperio> well, we still have the markdown interpreter PR for rustdoc
8:32 AM <•nrc> yes
8:33 AM <•nrc> my understanding there is that the current approach is stuck by the limitations of the bootstrap
8:33 AM <imperio> alex proposed to put it outside of standard build, no?
8:34 AM <misdreavus> iirc that's what https://github.com/rust-lang/rust/issues/43284 was for
8:34 AM <imperio> and I approved :D
8:35 AM <•nrc> I believe that should help (at least it would move it into the same phase as the RLS and that uses proc macros)
8:36 AM <imperio> good
8:36 AM <imperio> then we just have to wait for it
8:36 AM <•nrc> although I'm not 100% clear on the architecture of the warning code
8:36 AM <•nrc> we still couldn't use proc macros in rustdoc
8:37 AM <•nrc> that is, iiuc, librustdoc cannot depend on html_diff
8:37 AM <imperio> yep
8:38 AM <•nrc> so it would have to become a separate utility of some kind
8:38 AM <•nrc> ?
8:38 AM <imperio> and except if you know someone who's willing to write an html parser...
8:39 AM <•nrc> imperio, misdreavus: how badly would this go wrong if we did a very naive comparison using text?
8:39 AM <imperio> pretty badly
8:39 AM <•nrc> I expect we'd get a lot of false positives
8:39 AM <imperio> you can have very different text for same display
8:39 AM <misdreavus> the initial comparison work choked on additional spaces being put in somewhere
8:39 AM <•nrc> I imagine we could strip whitespace pretty easily
8:39 AM <misdreavus> i don't remember precisely
8:39 AM <•nrc> would that get us very far or not really?
8:40 AM <imperio> no, that's part of the problem
8:40 AM <•nrc> how so?
8:40 AM <imperio> you need to be able to parse html to strip whitespaces
8:40 AM <misdreavus> iirc the spaces were in places that were "harmless" to the actual rendering
8:40 AM <misdreavus> or putting linebreaks in different places or something like that
8:40 AM <•nrc> imperio: is that true even if you don't want to render the HTML?
8:41 AM <misdreavus> trivial stuff if you're parsing it semantically, but will trample a naive text comparison
8:41 AM <imperio> my problem with playing with html like this is clearly false positive/negative
8:42 AM <imperio> and iirc, both markdown renderer output different tags
8:42 AM <misdreavus> steveklabnik did the initial comparison work and did a diff by git commit history
8:42 AM <misdreavus> the diff was huge but full of trivial stuff
8:42 AM <•nrc> oh, that would be a much bigger problem
8:42 AM <imperio> but if doing a very naive comparison is fine for you, then I'm all for it
8:42 AM <•nrc> I'm trying to get an idea of possible alternatives. It sounds like it would not be fine :-(
8:43 AM <imperio> we all thought
8:43 AM <imperio> I lost quite the amount of time on it already
8:43 AM <misdreavus> plain/naive text comparison was what we tried first :/
8:44 AM <•nrc> ok
8:45 AM <•nrc> hmm, so hopefully 43284 will help us out here and I'll try and figure out if we can get that done quickly or if we can do anything else here
8:45 AM <•nrc> do people have other tools news?
8:45 AM <•nrc> I put the VSCode extension in the marketplace - https://marketplace.visualstudio.com/items?itemName=rust-lang.rust
8:46 AM <•nrc> I tweeted, but have avoided an official announcement while we iron out some bugs
8:46 AM <imperio> oh nice
8:46 AM <•nrc> On the RLS side, there are some performance improvements either done or in process
8:47 AM <imperio> ah nice too
8:47 AM <imperio> things are improving! :D
8:47 AM <misdreavus> i got an ill-timed itch and started sketching a syn-based doc parser last week
8:47 AM <•nrc> misdreavus: tell us more!
8:47 AM <misdreavus> didn't get far when i started talking with people about it in #rust-internals and realized how much extra work it would need to do
8:47 AM <misdreavus> !gh QuietMisdreavus crystal-rustdoc
8:47 AM <rustbot> https://github.com/QuietMisdreavus/crystal-rustdoc
8:48 AM <misdreavus> the thought was, since it has access to all the cfg tags, it can really easily add those to the output
8:48 AM <•nrc> as in, it would handle code that was cfg'ed off?
8:48 AM <misdreavus> the flipside of that, is that you get zero help from the compiler and have to reimplement some stuff yourself
8:48 AM <•nrc> istm that handling the more complex impls would be very difficult?
8:49 AM <misdreavus> nrc: yeah, since syn is a separate parser from the compiler, it doesn't get to the step where the compiler would strip the cfg stuff out
8:49 AM <misdreavus> yeah, it would lead to some duplication in fairly benign cases
8:49 AM <misdreavus> also, macros and build-scripts stopped me dead in my tracks
8:49 AM <•nrc> ah, yeah, I can imagine
8:49 AM <misdreavus> expanding macros and handling cfgs are done at the same time in the compiler
8:50 AM <misdreavus> so there's no way to get "just expand the macros but leave in the non-relevant cfgs"
8:50 AM <•nrc> on the debugging front, I finally wrote up some notes from a discussion with mw - https://github.com/nrc/dev-tools-team/pull/19 - I need to update with a lot of comments from interested parties
8:51 AM <•nrc> misdreavus: that is good to know
8:51 AM <•nrc> Clippy moved to the nursery \o/
8:51 AM <misdreavus> i should probably write this up into a readme so the repo has a proper grave marker
8:52 AM <misdreavus> \o/ (re: clippy)
8:52 AM <•nrc> any other Clippy news?
8:52 AM <•nrc> Manishearth: ^
8:52 AM <Manishearth> nrc: not really
8:52 AM <Manishearth> nrc: we're waiting for y'all to give us the go ahead to make it rustuppable, yeah?
8:52 AM <Manishearth> i mean, we could 1.0 it too
8:52 AM <•nrc> Manishearth: yeah
8:53 AM <•nrc> interesting
8:53 AM <•nrc> you'd want to do some kind of lint audit before 1.0, right?
8:53 AM <•nrc> any news from Rustup, Racer, bindgen, Xargo?
8:54 AM <•nrc> brson, jwilm, fitzgen/emilio, japaric ^ ?
8:54 AM <fitzgen> bindgen is about to have another release, mostly bug fixes
8:54 AM <•nrc> japaric, brson: did you get a chance to start the minimal 'Xargo in Cargo' RFC?
8:55 AM <japaric> I sent a draft of the "Xargo in Cargo" RFC to brson; waiting for feedback
8:55 AM <•nrc> fitzgen: cool!
8:55 AM <Manishearth> nrc: sort of
8:55 AM <Manishearth> nrc: but i would actually prefer to ship it before we 1.0 it
8:55 AM <brson> no news from me
8:55 AM <•nrc> Manishearth: that makes sense to me too
8:55 AM <fitzgen> going to start maintaining a users.rust-lang.org releases thread for marketing
8:55 AM <fitzgen> but thats all the news we have really
8:55 AM <fitzgen> still no updates on moving to nursery
8:56 AM <•nrc> fitzgen: great! I've been meaning to start collecting tools release notes for a combined announcement, but haven't had a chance yet
8:56 AM <fitzgen> yeah, we don't have release notes yet, so ;)
8:56 AM <•nrc> I keep hearing lots of interest in bindgen stuff from Gecko media/layout folk btw
8:57 AM <fitzgen> heh, mostly cause stylo builds keep breaking ;)
8:57 AM <•nrc> :-p
8:58 AM <•nrc> ok, we're out of time. Thank you everyone!
```
