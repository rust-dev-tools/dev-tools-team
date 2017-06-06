# June 5th 2017

Triage week, irc meeting in #rust-dev-tools

## Notes

### Roadmaps

Brainstorming in https://github.com/nrc/dev-tools-team/tree/master/roadmaps, nrc requests team to contribute.


### Triage

* #41991 - still stuck on a Rustbuild issue (cfg(stage0) incorrect for Rustdoc). nrc to investigate
* https://github.com/rust-lang/rfcs/pull/1946 - some discussion on namespacing. Consensus to move towards FCP.
* https://github.com/rust-lang/rfcs/pull/1990 - question over whether this would be subsumed by `#[doc = include_str!("foo.rs")]`, otherwise ready for FCP


### Tools check-in

* nrc forgot this, sorry



## IRC log

```
<•nrc> hello everyone!
8:02 AM <fitzgen> hi!
8:02 AM <•nrc> this week is triage week
8:02 AM <•nrc> before we get into the triage though
8:02 AM <•nrc> I want to start making roadmaps for tools
8:02 AM <•nrc> so I added a roadmaps directory to the repo
8:02 AM <•nrc> and started brainstorming ideas for RLS and Rustfmt
8:03 AM <•nrc> it would be great if you could do the same for tools you work on
8:03 AM <steveklabnik> hey hey
8:03 AM <•nrc> don't worry about prioritising or justifying for now, we'll get to that, just brainstorming ideas would be good to start with
8:04 AM <fitzgen> link to the skeleton roadmaps you created?
8:04 AM <•nrc> if you have a roadmap for a tool already, you could just link to that, rather than starting fresh
8:04 AM <misdreavus> https://github.com/nrc/dev-tools-team/tree/master/roadmaps
8:04 AM <•nrc> https://github.com/nrc/dev-tools-team/tree/master/roadmaps
8:04 AM <mw> is this also relevant for debuggers ?
8:04 AM <•nrc> too quick!
8:04 AM <misdreavus> :3
8:04 AM <•nrc> mw: yep - anything and everything
8:04 AM <mw> ok
8:04 AM <•nrc> add a link if it is not on the list
8:05 AM <•nrc> we'll need to come back and prioritise, but to start with let's get as much covered as we can
8:05 AM <•nrc> ok, nominated issues: https://github.com/rust-lang/rust/issues?q=is%3Aopen+is%3Aissue+label%3AT-dev-tools+label%3AI-nominated
8:05 AM <•nrc> just 1
8:05 AM → matklad joined (matklad@moz-4m1.s0g.92.93.IP)
8:06 AM <•nrc> https://github.com/rust-lang/rust/issues/39726
8:06 AM <•nrc> nope, just forgot to un-nominate
8:06 AM <•nrc> no PRs
8:06 AM <•nrc> RFCs: https://github.com/rust-lang/rfcs/pulls?q=is%3Aopen+is%3Apr+label%3AT-dev-tools
8:07 AM <•nrc> let's talk 1946
8:07 AM <imperio> important one
8:07 AM <killercup> nrc: just replied to your comment a few minutes ago fyi
8:07 AM <•nrc> "My preference would be that no qualifier only works if there is a single namespace"
8:07 AM <•nrc> I think I phrased that badly
8:08 AM <•nrc> I meant eliding the qualifier only works if there is no ambiguity
8:08 AM <•nrc> maybe not so badly phrased after all]
8:08 AM <•nrc> or npt\
8:08 AM <•nrc> gah, I can't type today
8:09 AM <•nrc> I think most names exist only in a single namespace, right?
8:09 AM <killercup> sure
8:09 AM <•nrc> it is rare to have ambiguity
8:09 AM <killercup> yes, very rare i'd say
8:09 AM <•nrc> the point about not breaking with external links
8:09 AM <•nrc> you either get loud breakage
8:10 AM <•nrc> or silent breakage where the link changes to a different item
8:10 AM <killercup> yes
8:10 AM <•nrc> "Do you think this is a hard problem" - I think it is  a hard problem to map from namespaces to prefixes, the inverse is easy
8:11 AM <killercup> ah, that's true
8:11 AM <killercup> though, you already need that to build urls
8:11 AM <killercup> (ar least in some cases)
8:12 AM <•nrc> nope - namespace URLs will redirect to the prefix ones
8:12 AM <•nrc> (and with the rewrite I hope both will work, without the redirect)
8:12 AM <killercup> okay, interesting
8:13 AM <killercup> so, i was gonna suggest another way
8:13 AM <killercup> we have a (badly named) bad_style lint
8:13 AM <killercup> without a prefix, we could assume that the thing we are linking to follows that lint
8:14 AM <killercup> i.e., [Foo] is a const type, while [foo] is not
8:14 AM <•nrc> I guess these links would be case sensitive?
8:14 AM <killercup> that might give us a pretty good default/fallback
8:14 AM <matklad> modules and functions live in different namespaces, but are both lowercased
8:14 AM <killercup> nrc: yes, the need to be
8:15 AM <killercup> matklad: ah, good point
8:15 AM <•nrc> yeah, modules
8:15 AM <•nrc> so
8:15 AM <•nrc> I think this is kind of a detail and we can discuss on thread
8:15 AM <•nrc> larger picture
8:16 AM <•nrc> I think we should enter FCP for this
8:16 AM <•nrc> blocked on this detail, obvs
8:16 AM <•nrc> anyone object?
8:16 AM <steveklabnik> nope
8:16 AM <mw> sg
8:16 AM <•nrc> or have anything else we should discuss now or should block FCP?
8:16 AM <killercup> okay, i'll make a new rfc, let's change modules to 𝒄𝒖𝒓𝒔𝒊𝒗𝒆 :D
8:16 AM <jntrnr> my eyes!
8:17 AM <imperio> nrc: for me it's ok, except for the detail
8:17 AM <•nrc> RFC for modules in R-to-L script
8:17 AM — misdreavus is unfamiliar with FCP process
8:17 AM <•nrc> FCP = final comment period
8:17 AM <•nrc> described here:
8:18 AM → rvk joined (ravi@moz-are.72f.206.98.IP)
8:18 AM <•nrc> https://github.com/rust-lang/rfcs#what-the-process-is
8:18 AM <•nrc> we actually have a sign-off *before* entering FCP
8:19 AM <imperio> ah
8:19 AM <imperio> forgot that
8:19 AM <misdreavus> aha, right
8:20 AM <misdreavus> i was wondering how this detail would affect actually starting FCP
8:20 AM <•nrc> the idea is we block on specific things (just one here) and then going into FCP is automatic, then after 10 days (I think) the RFC gets accepted, unless someone objects
8:20 AM <misdreavus> cool
8:20 AM <misdreavus> then that's a :+1: from me
8:20 AM — •nrc will figure out how to make RFC bot work with the dev-tools team
8:20 AM <Manishearth> o/
8:20 AM <Manishearth> sorry im late
8:21 AM <•nrc> https://github.com/rust-lang/rfcs/pull/1990 also exists
8:21 AM <•nrc> but seems to be having active discussion
8:21 AM <•nrc> is there anything there we should discuss in this meeting?
8:21 AM → mib_123133 joined (Mibbit@moz-7sarhd.dip0.t-ipconnect.de)
8:21 AM <•nrc> steveklabnik: misdreavus ^
8:22 AM <killercup> nevertheless, except for the namespace resolution via prefixes, any other open points on 1946 we need to discuss?
8:22 AM <killercup> i'll try to write another summary on the namespace thing later
8:22 AM <misdreavus> in my comment on #1990 i asked "Where are paths in doc(include) attributes based?" and haven't gotten a response
8:22 AM <misdreavus> that's my only blocker on 1990
8:23 AM <•nrc> killercup IMO I think we just need to sort the namespaces thing and we're good to go
8:23 AM <•nrc> misdreavus: what does "based" mean here?
8:23 AM <killercup> (i'm not sure if i broke my irc client with unicode or if no one is saying anything xD)
8:23 AM <misdreavus> nrc: when i give a filename to doc(include), what folder does it look in?
8:23 AM <•nrc> ah
8:24 AM <steveklabnik> sorry spaced out for a second
8:24 AM <Manishearth> I'm very +1 on the external doc attribute thing
8:24 AM <steveklabnik> docs team was basically "we like it, some impl quesitons"
8:24 AM <•nrc> relative to the file seems a good choice, since relative to src root won't work
8:25 AM <•nrc> and I think relative to the project directory has issues with worspaces, maybe?
8:25 AM <misdreavus> yeah, that's where i'd like to see it
8:25 AM <misdreavus> (relative to the file, that is)
8:25 AM <•nrc> steveklabnik: any impl questions worth discussing here? Or better to resolve on thread?
8:25 AM <mw> how does that work for include!() ?
8:26 AM <mw> that should be consistent
8:26 AM <misdreavus> what happens if you include!() inside an include!()?
8:26 AM <•nrc> include is file relative
8:26 AM <killercup> what does include_str do?
8:26 AM <Diggsey> brson: got a few rustup PRs waiting on your review
8:26 AM <•nrc> i.e., we would be consistent
8:26 AM <steveklabnik> nrc: i don't think they need to be discussed really, mostly it's that it talks about "re-writing" doc attributes to ///
8:26 AM <misdreavus> imo it should follow the same pattern as include!() and friends
8:26 AM <steveklabnik> but currently, /// is sugar for doc attributes, so it feels backward
8:26 AM <brson> Diggsey: ok. it's on my list for this week
8:26 AM <Diggsey> kk, ty
8:26 AM <mw> include_str includes something as a string constant?
8:26 AM <•nrc> yeah
8:27 AM <steveklabnik> this feels like a minor thing to me and not particularly important; i don't think impl details are a huge deal for this RFC
8:27 AM <Manishearth> what's the final syntax we're settling on?
8:27 AM — killercup is _actually_ having irc connection issues, sorry
8:27 AM <Manishearth> I still like `#[doc(include=foo.md)]`
8:27 AM <•nrc> https://github.com/rust-lang/rfcs/pull/1990#issuecomment-305323449
8:28 AM <•nrc> is this an alternative?
8:28 AM <misdreavus> steveklabnik: yeah, the "compiles into" lines aren't as bad as i was thinking in the docs meeting, but i'm more hung up on paths
8:28 AM mib_123133 → killercup_
8:28 AM <•nrc> as in, if that worked, would this RFC be necessary?
8:29 AM <killercup_> yeah, that's what i've been wondering as well
8:29 AM <misdreavus> imo, yeah, it's an alternative
8:29 AM <steveklabnik> i think it is
8:29 AM <mw> interesting :)
8:29 AM <killercup_> it would be nice not to have a special case for that
8:29 AM — fitzgen has tried to do that too
8:30 AM <•nrc> I guess it would not work to do include_str!("foo.md#bar")
8:30 AM <misdreavus> if that's added, yeah
8:31 AM <misdreavus> iirc, the rfc as-is doesn't include that, tho
8:31 AM <killercup_> i don't actually like that feature, but that's another thing
8:32 AM <•nrc> ok, I'll comment on thread and find out if include_str! in attrs is likely to work ever
8:32 AM <•nrc> feels like it will be ready for FCP soon though
8:32 AM <misdreavus> yeah
8:33 AM <•nrc> ok, that's the triage list done
8:33 AM <•nrc> imperio: is the Pulldown/warnings thing still blocked on Rustbuild issues?
8:33 AM <imperio> nrc: normally no
8:33 AM <imperio> I commented my current (and old) issue
8:33 AM <imperio> a few times
8:33 AM <imperio> just in case :)
8:33 AM <imperio> but no one answered
8:35 AM <•nrc> this is https://github.com/rust-lang/rust/pull/41991 btw
8:35 AM — •nrc reads last comment
8:36 AM <•nrc> so cfg(not(stage0)) is not working?
8:36 AM <imperio> nrc: interesting right?
8:36 AM <imperio> nope
8:36 AM <imperio> just like it was rustc from stage0 used to build rustdoc in stage1
8:36 AM <imperio> very weird
8:38 AM <•nrc> hmm, I guess either rustdoc has weird staging or Rustbuild is not setting the stage properly for rustdoc?
8:38 AM <•nrc> I'll try and take another look
8:38 AM <•nrc> Anyone else got anything to discuss today?
8:39 AM <imperio> I try to find it myself but only found that it was supposed to be bound to the current build stage
8:39 AM ⇐ cmyr quit (cmyr@moz-7tugri.cpe.teksavvy.com) Ping timeout: 121 seconds
8:40 AM <•nrc> Next week is a focused meeting week
8:40 AM <•nrc> does anyone have anything they want to discuss?
8:40 AM <imperio> on which tool?
8:41 AM <•nrc> I listed a few ideas on the etherpad, please add more!
8:41 AM <•nrc> imperio: any tool! Or cross-cutting stuff
8:41 AM <•nrc> I guess I'd prefer not to do rustdoc again straight away
8:42 AM <imperio> nrc: btw, is it worth it trying to make my rustdoc pr merged before the next release or not?
8:42 AM <•nrc> I'm not sure when the next release is - soon, right?
8:42 AM <imperio> next week I think
8:42 AM <•nrc> we should try!
8:43 AM <misdreavus> this week, iirc?
8:43 AM <mw> https://rusty-dash.com/
8:43 AM <•nrc> given we want a warning cycle
8:43 AM <•nrc> so, this Friday
8:43 AM <imperio> nrc: then if you have time, can you focus on finding the cfg tage thing please?
8:43 AM <mw> but we may already have branched off?
8:43 AM <imperio> after that, I'll be able to quickly move on
8:43 AM <mw> brson: ^
8:43 AM <misdreavus> beta might already be off, yeah
8:44 AM <imperio> ah damn
8:44 AM <imperio> then no huryr
8:44 AM <imperio> *hurry
8:44 AM <misdreavus> well, stable might be, i forget which goes first
8:45 AM <misdreavus> https://forge.rust-lang.org/release-process.html if this is still accurate
8:45 AM <brson> if not then soon
8:45 AM <brson> acrichto: will know
8:45 AM <brson> those docs look relatively up to date
8:45 AM <acrichto> oh I should do that, no action on the upcoming release yet
8:46 AM <imperio> ah
8:46 AM <imperio> nrc: if we do it quickly then it should be fine :p
8:46 AM <•nrc> I'll look today
8:46 AM <brson> acrichto: removing this line broke windows-gnu builds https://github.com/rust-lang/rust/pull/42225/files#diff-1b71e145dde424ac83e674eef22373d8L710
8:46 AM → cmyr joined (cmyr@moz-7tugri.cpe.teksavvy.com)
8:46 AM <brson> i assume that is just for the sake of our packaged gcc on windows
8:46 AM <brson> and should have no effect anywhere else?
8:47 AM <brson> adding the bin path to PATH, which is now only done for -msvc
8:48 AM <•nrc> ok, for next week I propose two choices: plans for improving contribution or Rustup
8:48 AM <acrichto> brson: context?
8:48 AM <•nrc> brson: can we fill a meeting with Rustup stuff?
8:48 AM <brson> acrichto: https://github.com/rust-lang/rust/issues/42422
8:50 AM <acrichto> brson: ok makes sense, are you just gonna send a PR to add it back? (should we discuss this in pm?)
8:50 AM <brson> acrichto: i will work on a fix. i'm not sure if i'm just going to add it back. it doesn't quite work with how gcc returns it's set of env vars to set, to just add that line back in
8:51 AM <brson> nrc: we can if desired. i don't have anything pressing, but if you have rustup subjects you want to discuss i could use that to seed some agenda
8:51 AM ⇐ matklad quit (matklad@moz-4m1.s0g.92.93.IP) Quit: http://www.kiwiirc.com/ - A hand crafted IRC client
8:52 AM <•nrc> brson: I don't have anything urgent
8:53 AM <•nrc> I want to get a better handle on future plans, etc
8:53 AM <brson> acrichto: do you agree this line only matters for our bundled windows-gnu builds?
8:53 AM <•nrc> lets talk about contribution stuff
8:53 AM <•nrc> I think that is important and somewhat urgent
8:53 AM <brson> contribution for rustup?
8:53 AM <acrichto> brson: yes
8:53 AM <•nrc> brson: no, in general, for all tools
8:54 AM <brson> sure
8:54 AM <•nrc> I'll send an email
8:54 AM <•nrc> ok, I think that is it for this meeting
8:54 AM <•nrc> thank you everyone!
8:54 AM <•nrc> See you in one or two weeks time
8:55 AM <misdreavus> <3
8:55 AM <brson> thanks nrc
8:55 AM <Manishearth> nrc: brson now that TUF is almost there we probably should discuss that in the rustup meeting too
```