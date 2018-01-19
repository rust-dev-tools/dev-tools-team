# January 18, 2018

Triage week, irc meeting in #rust-dev-tools

## Notes

### Stabilize allow_fail flag test feature

* https://github.com/rust-lang/rust/pull/46501
* decided to close and leave unstable
* should be addressed by custom test frameworks

### Show long-running tests in progress, even when multithreaded

* https://github.com/rust-lang/rust/pull/46990
* call for comments

### Add approximate suggestions for rustfix

* https://github.com/rust-lang/rust/pull/47540
* everyone was keen
* nrc to review


### Other

* Some discussion of RFCs
* Japaric to write up Xargo in Cargo thoughts in a Cargo issue. We'll have an
  experimental implementation, *then* an RFC
* next weeks meeting will be on testing again
* some discussion of implementing methods in rustdoc2


## IRC log

```
10:04 AM <•nrc> no issues, but a few PRs
10:04 AM <•nrc> https://github.com/rust-lang/rust/pull/46501
10:04 AM <•nrc> Stabilize allow_fail flag test feature
10:05 AM <•nrc> this is an imperio PR
10:05 AM <•nrc> and is testing related
10:05 AM <•nrc> original PR: https://github.com/rust-lang/rust/pull/42219
10:05 AM <•nrc> there was a fair bit of discussion there about whether this feature pulls its weight
10:06 AM <•nrc> my worry is that that has not been demonstrated yet
10:06 AM <•nrc> other than that, stabilisation seems ok
10:06 AM <•nrc> (but that is kind of a big thing)
10:06 AM <imperio> yes
10:06 AM <imperio> it's quite an old one
10:06 AM <imperio> and the unstable feature is even more old
10:07 AM <imperio> I was against it when the first author wanted to add it
10:07 AM <killercup> i see no reasons not to stabilize this, except that it makes the default test framework a bit more complex when in the future this could be implemented externally
10:07 AM <steveklabnik> killercup: same
10:07 AM <imperio> it's been added and since the time it's there, I think it's time to stabilize or remove it
10:07 AM <oli_obk_> I have never seen this feature before this PR. I was surprised it exists and am with those who don't really see the use of having something "potentially failing"
10:07 AM <imperio> oli_obk_: the debate on the original PR was mostly what you just said
10:08 AM <killercup> imperio: is this the same as wrapping the body of a test in catch_panic?
10:08 AM <•nrc> imperio: I don't think we need to stabilise or remove if there is a future path that adds this in a different way
10:08 AM <imperio> killercup: yes
10:08 AM <imperio> nrc: then it's a removal :)
10:08 AM <imperio> if it gets replaced, the current one is removed
10:08 AM <•nrc> I mean, it's an eventual removal , not an immediate removal
10:08 AM <imperio> I just want it to get "out of the way" at some point
10:09 AM <killercup> are there any users we know of? can we keep it in limbo until it can be replaced with an externally implemented thing?
10:09 AM <imperio> killercup: I know at least 2 people using it (excluding the feature's author)
10:09 AM <imperio> but that's all
10:09 AM <imperio> since it's unstable, I think it can only attract very few people
10:09 AM <•nrc> are they desperate to have this stabilised?
10:10 AM <imperio> they're nightly users so no
10:10 AM <killercup> doesnt seem like it
10:10 AM <•nrc> ok
10:10 AM <mw> I see no reason to do something now
10:11 AM <imperio> :-/
10:11 AM <•nrc> then I propose we leave this unstable, with a plan to let custom test frameworks handle it, concretely, we'll deprecate and then remove once custom test frameworks are supproted
10:11 AM <steveklabnik> sgtm
10:11 AM <mw> +1
10:11 AM <imperio> I'd like to have a definitive state on it but if keeping it unstable is fine then let's just talk about it later in the future
10:12 AM <imperio> (as nrc said)
10:13 AM <•nrc> https://github.com/rust-lang/rust/pull/46990
10:13 AM <•nrc> Show long-running tests in progress, even when multithreaded
10:14 AM <•nrc> not nominated, and I don't think we need to discuss, but if anyone has opinions they should comment
10:14 AM <•nrc> https://github.com/rust-lang/rust/pull/47540
10:14 AM <•nrc> Add approximate suggestions for rustfix
10:14 AM <•nrc> an implementation of a variation of an RFC: https://github.com/rust-lang/rfcs/pull/1941#issuecomment-299097152
10:15 AM <•nrc> (which I didn't realise when I nominated it)
10:15 AM <•nrc> so, I think this is probably OK to land, but needs to be more explicitly experimental
10:15 AM <•nrc> do others have opinions on this?
10:16 AM <oli_obk_> I do, but that's pretty obvious ;)
10:16 AM <killercup> i want this :)
10:16 AM <•nrc> a 'soft' question is about what `machine_applicable` means - does it mean might be applicable or always applicable? How much user-intervention is expected? What is the workflow for an IDE? For Rustfix?
10:17 AM <killercup> this is basically the 'whitelist' for rustfix
10:17 AM <•nrc> I'm not super-happy about this being opt-out, rather than opt-in, but I guess that is for back-compat?
10:17 AM <oli_obk_> nrc: yea, automatically means no user intervention needed
10:17 AM <xanewok> nrc: in such a situation, could rustfix be a possible driver for code action (lightbulb) in rls?
10:17 AM <killercup> Manishearth: ^
10:18 AM <oli_obk_> nrc: the plan is to vet all rustc suggestions at some point
10:18 AM <•nrc> xanewok: rustfix is an alternative client (i.e., one would use either rustfix or an IDE)
10:18 AM <•nrc> aiui
10:18 AM <oli_obk_> for now, clippy is the only one who's going to do anything about it
10:18 AM <oli_obk_> rustc is not impacted at all this way
10:18 AM <oli_obk_> any other way would have an impact on rustc
10:18 AM <•nrc> but the impact is that all rustc suggestions are treated as applicable - which seems wrong to me
10:19 AM <killercup> the reasoning for out-out was that most of rustc's suggestions are machine_applicable, but clippy e.e. would probably switch to the approx version until lint suggestions are proven good
10:19 AM <imperio> damn
10:19 AM <imperio> if I knew that my PR would make waves still now
10:19 AM <imperio> what a funny world
10:20 AM <•nrc> hmm, I guess this is an issue for rustfix to work out
10:20 AM <oli_obk_> nrc: yea, but we can swap this at any point in the future
10:20 AM <killercup> nrc: would you be okay to land this PR if it also switched all current suggestions to use span_approximate_suggestions?
10:20 AM <•nrc> I'm a bit uncomfortable about landing this with such loose semantics, but as long as its unstable, we can iterate
10:20 AM <oli_obk_> yea that was the idea, let rustfix worry about it for now
10:21 AM <killercup> yeah
10:21 AM <•nrc> killercup: we can land as is, as long as we can iterate, I think
10:21 AM <killercup> cool
10:21 AM <oli_obk_> killercup: we could make sure the json output exists only if -Zunstable-features is passed
10:21 AM <•nrc> OK, nobody seems to object to this, so let's land it - I still need to review, but I just want some more comments, etc., I think
10:23 AM <killercup> cool. fyi, i plan on working on a bunch more clippy/rustfix integration over the next weeks, and will probably also patch rls to have a 'clippy mode'
10:23 AM <•nrc> I think it should only be in the JSON if a tool specifically enables it. That works for clippy, but would mean rustfix couldn't experiment with built-in suggestions
10:23 AM <•nrc> killercup: how does that feel ^
10:23 AM <•nrc> ?
10:24 AM <•nrc> killercup: ++ for adding Clippy to RLS - have wanted that for a while :-)
10:24 AM <killercup> nrc: sounds good, rustfix currently calls clippy-driver internally
10:24 AM <•nrc> ok, cool
10:24 AM <•nrc> will comment in my review
10:25 AM <•nrc> RFCs, sigh, it always seems to be the same ones...
10:26 AM <killercup> oh, we also have https://github.com/rust-lang/rfcs/pull/2285
10:26 AM <oli_obk_> killercup: I planned to make rls-clippy a cargo feature like rls-rustfmt, so we might be able to have rustc produce rls even if clippy and rustfmt are broken
10:26 AM <killercup> oli_obk_: sweet
10:27 AM <killercup> oli_obk_: let me know when you work on this, i'll probably hack this in until then :)
10:27 AM <oli_obk_> killercup: don't let me hold you up though, I still need to flesh this out in rustc, adding clippy is entirely orthogonal. Once I get that setup to work with rustfmt, we can probably "just add" clippy
10:28 AM <killercup> oli_obk_: i fear this is what i'll be doing when i arrive friday before fosdem :D
10:28 AM <•nrc> japaric: ping
10:28 AM <•nrc> https://github.com/rust-lang/rfcs/pull/1133
10:28 AM <•nrc> I think the Xargo into Cargo RFC was an alternative to this right?
10:29 AM <japaric> er, yes I think so
10:29 AM <•nrc> how are things going with that? Is there anything we can do to get an RFC ready?
10:31 AM <japaric> what I recall of what I talked with acrichto was something like "we should just go and do it" as in go and implement the thing
10:33 AM <mw> that sounds very much like acrichto :D
10:33 AM <•nrc> do you agree? Or do you think there is value in having an RFC?
10:33 AM <•nrc> Personally, I tend to err on the side of an RFC here, but I'd also be fine with an unstable implementation, then an RFC
10:34 AM <•nrc> but I would like to move forward - I think it is really important work in its own right, but I also want to do something about 1133 :-)
10:36 AM <japaric> well, I'd go with an unstable implementation, gain experience and see if we really want to do all the other stuff that's in 1133 and that xargo doesn't
10:37 AM <japaric> 1133 wants to remove the sysroot altogether and have versioned core / std crates; I think the later is beyond dev-tools scope and more into the lib teams territory
10:38 AM <•nrc> ok
10:39 AM <•nrc> japaric: would you mind writing up a Cargo issue detailing the work to do an unstable implementation please? Then I think we can probably point at that from 1133 and postpone it until that is done, or separate out the work that people still want
10:40 AM <•nrc> There is https://github.com/rust-lang/rfcs/pull/2287 but I want to circle back to that in one minute, and talk about next week's meeting first, because...
10:40 AM <japaric> nrc: sure
10:40 AM <•nrc> I propose we have another testing meeting!
10:40 AM <•nrc> japaric: thanks!
10:41 AM <•nrc> I won't be able to make it (I will be at LCA), and it would be good if we could have a meeting with Manish involved, so I propose we pick a time which is good for Europe and India (and east coast for jonhoo, if possible)
10:41 AM <killercup> yeah, the testing discussion keeps going on i.rl.o
10:41 AM <•nrc> and those who want to can drill down into the design and proposals a bit more
10:42 AM <•nrc> I promise I'll have some comments before the meeting
10:42 AM <•nrc> and someone will need to keep notes
10:42 AM <•nrc> does that sound like a good idea?
10:42 AM <•nrc> the alternative is we cancel the meeting :-)
10:42 AM <killercup> sounds good
10:42 AM <killercup> oh, do you propose we do this _next week_?
10:43 AM <•nrc> yeah
10:43 AM <killercup> not sure if it'd be good to let the dust settle a bit and discuss Manish's erfc a bit more
10:43 AM <killercup> but i guess next week is better than in 3 weeks, sooooooo :)
10:45 AM <•nrc> Manish seems really keen to move to an RFC PR soon, I'd like to have another round of discussion first, so I think sooner is better
10:47 AM <•nrc> japaric, jonhoo, anyone else interested in testing - sound good?
10:47 AM <killercup> alright, i'm in :) do you want to pick a time now? 4pm utc = 11am new york = 9.30pm mumbai (i think manish is currently an hour before that tho)
10:48 AM <•nrc> let's do that by email, since Manish isn't here
10:48 AM <japaric> nrc: sgtm
10:48 AM <•nrc> cool
10:48 AM <•nrc> ok, given that, do we want to do anything about https://github.com/rust-lang/rfcs/pull/2287 now? (Stabilising bench)
10:49 AM <•nrc> or leave that for next week?
10:49 AM <killercup> can we leave it for next week? i have opinions that i've not yet written down :)
10:50 AM <•nrc> sure
10:50 AM <•nrc> OK, that's the agenda done
10:50 AM <•nrc> anyone got anything else we should discuss?
10:50 AM <steveklabnik> i dunno if my issues with new rustdoc and the rls are releavnt
10:50 AM <steveklabnik> that's the closest thing i have
10:50 AM <killercup> can we talk about https://github.com/rust-lang/rfcs/pull/2285 real quick too? (not as important though)
10:51 AM <•nrc> killercup: sure
10:52 AM <killercup> steveklabnik: do you want to go ahead? do have concrete issues?
10:52 AM <•nrc> steveklabnik: also sure! https://internals.rust-lang.org/t/rustdoc2-rls-analysis-and-the-compiler-help-wanted/6592
10:52 AM <steveklabnik> killercup: it's ^
10:52 AM <steveklabnik> nrc's already aware that it exists
10:52 AM <steveklabnik> but uh, i have to figure out some way around this for now
10:53 AM <steveklabnik> i started poking around at some of this
10:53 AM <steveklabnik> but it's pretty massive; like, i don't know the compiler's codebase or the rls'
10:53 AM <jonhoo> Thu 4pm UTC next week *should* work for me
10:53 AM <•nrc> the really easy path is to just handle inherent impls, where I believe most of the work is done and there is just the stuff in rls-analysis to do
10:54 AM <steveklabnik> yeah, that would be 100% okay
10:54 AM <•nrc> that would prove out method handling, but it would be very limited in terms of completeness of coverage
10:54 AM <steveklabnik> basically, i'm almost at the point where people can actually try new rustdoc
10:55 AM <steveklabnik> but if it can't say any methods, it's not gonna feel great
10:55 AM <steveklabnik> whereas if it only has some, that's like, way way closer
10:55 AM <•nrc> and the caveat is that you might need to rewrite that once we have proper impl handling, the APIs might have to change
10:55 AM <•nrc> OK cool, that seems like the path of least resistance
10:55 AM <steveklabnik> i'd be fine with that
10:55 AM <steveklabnik> i'm trying to dogfood/get something in people's hands that's tangible
10:55 AM <steveklabnik> there's gonna be tons of iterating
10:55 AM <•nrc> I'll need to check to see exactly what is done and if it still works - I haven't used that data for ages, so it might have rotted
10:56 AM <steveklabnik> so i half expect to re-write it on my side anyway ;)
10:56 AM <est31> https://github.com/euclio/rustdoc-static
10:56 AM <•nrc> heh
10:56 AM <est31> sadly this seems to have stalled :/
10:57 AM <•nrc> steveklabnik: I'll try and get to this later today, if not then I'll have time on Sunday
10:57 AM <steveklabnik> that would be *amazing* <3
10:58 AM <•nrc> by which I mean - find out what works and write some instructions, probably not actually implement anything, so not that amazing :-)
10:58 AM <steveklabnik> hehe
10:59 AM <steveklabnik> ill take anything i can get
10:59 AM <•nrc> killercup: we have two mins want to talk 2285 quickly?
10:59 AM <killercup> sure, thanks!
10:59 AM <steveklabnik> 👍
10:59 AM <killercup> so, https://github.com/rust-lang/rfcs/pull/2285 is an update to my intra docs links rfc
11:00 AM <killercup> basically, while implementing it, we found some issues
11:00 AM <killercup> it boils down to using spaces in URIs
11:00 AM <•nrc> iirc, the problem with the shorthand paths is future compat? Like if you add a name to the namespace, you break some random bit of rustdoc. Were there other issues?
11:00 AM <•nrc> using the @ seems fine
11:01 AM <•nrc> I still hate using 'struct' rather than 'type' with all my heart
11:01 AM <killercup> i'm fine with this update and would gladly merge it :)
11:01 AM <killercup> not sure how much process we have around updating rfc texts though
11:01 AM <killercup> nrc: you'll never have to type either in 99.95% of all cases when the rfcs is implemented fully
11:01 AM → mmacherey joined (Thunderbird@moz-un0sl0.dyn.telefonica.de)
11:02 AM <•nrc> and I'm pretty sure we agreed that macros shouldn't include the `!`, I'm surprised to see that in the RFC
11:02 AM <killercup> nrc: did we? that was in the original rfc
11:02 AM <•nrc> killercup: my problem with this is for tools, though where the 0.05% matters
11:03 AM <•nrc> yeah, I don't recall, I thought we agreed to change that in the original RFC, but maybe not
11:03 AM <killercup> nrc: oh, tools will be able to use type@foo, the differentiation is just for humans
11:04 AM <•nrc> that should probably be in the RFC
11:04 AM <•nrc> "Non-disambiguated paths cannot be used to link to macros"  - why?
11:04 AM <•nrc> I should just review the RFC, rather than commenting here, sorry
11:04 AM <killercup> nrc: it is in the rfc, it's the paragraph below manish's changed iirc
11:05 AM <killercup> nrc: it's fine :)
11:05 AM <•nrc> ok, we;re out of time (and I have another meeting)
11:05 AM <•nrc> thank you everybody!
11:05 AM <killercup> nrc: this is a minor update to the original rfc, though. what's the process here?
11:06 AM <•nrc> killercup: I'll comment on the RFC, but we should go to FCP pretty soon and have it land quickly
11:06 AM <killercup> nrc: +1
11:06 AM <killercup> see y'all fresh and early next week to another round of testing then ;)
11:07 AM <jonhoo> w00t testing \o/
```
