# Septmeber 18th 2017

Triage week, irc meeting in #rust-dev-tools

## Notes

## Impl period

* is starting today!
* lots of tools things to work on
* https://www.rustaceans.org/findwork/impl
* https://gitter.im/rust-impl-period

### Triage

#### Stabilising `-Zprofile`

* https://github.com/rust-lang/rust/pull/44673
* generally in favour of the idea but stabilisation seems a little premature
* we'd like to get some experience with coverage tools to make sure we're doing the right thing
* usual concerns about LLVM dependence
* could have a better name and format

#### RFC: Add external doc attribute to rustc

* https://github.com/rust-lang/rfcs/pull/1990
* we decided that the base path should be the source root, and not to use a `doc_root` flag initially
* ready to merge with that

#### RFC: Attributes for tools, 2.0

* https://github.com/rust-lang/rfcs/pull/2103
* nrc to merge

#### RFC: Debuggable macro expansions

* https://github.com/rust-lang/rfcs/pull/2117
* we would like to get some experience with an implementation before accepting this RFC
* so noting our approval and in particular encouraging an implementation
* but not FCP'ing for now


## IRC log

```
8:02 AM <•nrc> Hello! Triage meeting time
8:03 AM <•nrc> https://public.etherpad-mozilla.org/p/rust-dev-tools
8:03 AM <•nrc> To start with, the impl period starts today!
8:03 AM <•nrc> No more RFCs!
8:04 AM <•nrc> More importantly, we are hoping/expecting that lots of people will turn up and want to help with tools
8:04 AM — misdreavus makes excited gestures
8:04 AM <•nrc> https://www.rustaceans.org/findwork/impl
8:05 AM <•nrc> https://blog.rust-lang.org/2017/09/18/impl-future-for-rust.html
8:06 AM <•nrc> We are trying out Gitter for communication for the impl period: https://gitter.im/rust-impl-period
8:06 AM <mw> note that gitter has an IRC bridge
8:07 AM <mw> which I personally find very helpful
8:07 AM <mw> https://irc.gitter.im/
8:07 AM <misdreavus> oh nice
8:07 AM <•nrc> We want to provide a stream of new projects and issues for people to work on, so if you get a chance to add things to findwork or tag issues, etc. over the next month or two, that would be awesome
8:08 AM <•nrc> one of the really important things to do is to label issues you're willing to mentor and to add instructions for them
8:09 AM <•nrc> please ping me if I can do anything to help or take a look  at anything, etc.
8:11 AM <•nrc> OK, triage
8:11 AM <•nrc> https://github.com/rust-lang/rust/issues?q=is%3Aopen+is%3Aissue+label%3AT-dev-tools+label%3AI-nominated
8:11 AM <•nrc> no nominated issues
8:11 AM <•nrc> PRs: https://github.com/rust-lang/rust/pulls?q=is%3Aopen+is%3Apr+label%3AT-dev-tools
8:12 AM <•nrc> https://github.com/rust-lang/rust/pull/44026
8:12 AM <•nrc> misdreavus: this is just waiting on review?
8:12 AM → vertexclique joined (vertexcliqu@moz-mtv228.dip.versatel-1u1.de)
8:12 AM <•nrc> ping steveklabnik for r? ^
8:12 AM <misdreavus> i think we talked about this one right after the last triage meeting
8:12 AM <misdreavus> but i don't remember what we said about it
8:13 AM <misdreavus> oh wait, you did leave a comment >_>
8:13 AM <misdreavus> so yeah, just needs review
8:14 AM <•nrc> cool
8:14 AM <•nrc> https://github.com/rust-lang/rust/pull/44673 is about stabilising -Zprofile I think
8:14 AM — misdreavus untags team
8:15 AM <misdreavus> soudns like we need FCP for stabilization?
8:16 AM <•nrc> does anyone use this flag
8:16 AM <•nrc> ?
8:17 AM <mw> apparently not
8:17 AM <•nrc> it was added at the start of June
8:17 AM <mw> acrichto: do you know about -Zprofile?
8:18 AM <misdreavus> presumably the PR author might? since they opened the stabilization pr
8:18 AM <•nrc> kennytm is using it in cargo cov
8:19 AM <•nrc> looks like the author is using it in grcov
8:19 AM <•nrc> I think with the change kennytm suggests it is probably a good idea to stabilise
8:20 AM <•nrc> anyone else have thoughts before I request fcp?
8:20 AM <steveklabnik> misdreavus: r+'d <3
8:20 AM <misdreavus> steveklabnik: <3
8:23 AM <japaric> nrc: my concern with these flags that directly expose some llvm feature is what does it mean to stabilize them. Are they supposed to also work with other backends like cretonne?
8:23 AM <killercup> nrc: am i right in assuming profile is a feature that we basically get from llvm? so it's more a question of it it's stable in llvm and we just expose it?
8:23 AM <•nrc> japaric: yeah, agreed I wrote a concern about that in my FCP comment
8:23 AM <•nrc> killercup: that is my understanding, yes
8:24 AM <killercup> ok
8:24 AM <japaric> nrc: +1
8:24 AM <•nrc> how stable something is in LLVM seems not something we can control though
8:24 AM <mw> I'm OK with stabilizing this while not guaranteeing the other backends will support it
8:24 AM <mw> *that other backends...
8:24 AM <•nrc> I basically agree with this approach, but I'm not sure how to formalise that?
8:25 AM <•nrc> or how to advertise it to users, more importantly
8:25 AM <acrichto> mw: I know vaguely of it
8:25 AM <acrichto> oh I don't think this should be stabilized at all yet
8:25 AM <acrichto> afaik it has seen very little testing
8:25 AM <acrichto> (although I could be wrong)
8:25 AM <acrichto> also I doubt the documentation is up to par w/ what we'd want
8:26 AM <mw> acrichto: I seem to remember that servo/firefox devs were interested in that as well, right?
8:26 AM <•nrc> it is used (from what I can tell) in cargo cov and grcov
8:27 AM <mw> maybe they've done some testing
8:27 AM <•nrc> documentation is a good point
8:27 AM <acrichto> ah ok, if it's got some usage I'd feel more comfortable
8:27 AM <acrichto> but my recollection of this feature is that it was added as an extremely thin layer over w/e llvm does
8:27 AM <acrichto> and we barely understood cross-platform compatibility or the integration into the rest of the ecosystem
8:27 AM <acrichto> in taht iirc it requires you to download an llvm tool to actually read the data
8:27 AM <acrichto> and we provide no means of acquiring such a tool
8:28 AM <acrichto> so it seems stable in terms of llvm won't yank it out from under our feet
8:28 AM <acrichto> but unstable in that it's still an "expert" feature requiring you to know everything about it to use it
8:28 AM <acrichto> although that's all relatively dated, so may not be true any more
8:28 AM <mw> maybe we should hold off on fcp until we've heard more from actual users
8:29 AM <mw> plus try to actively ping those users
8:30 AM <•nrc> rfc bot seems to be ignoring me, so perhaps that is a sign :-)
8:30 AM <acrichto> I think basically tl;dr; we need to be at a point where a fresh user can say "how do I profile rust code", they visit some docs, and then they're done
8:30 AM <acrichto> not sure if we're there yet
8:30 AM <acrichto> also iirc it's pretty subpar (the output)
8:30 AM <acrichto> in that you have to know all sorts of tricks to not get dead code to go away
8:30 AM <acrichto> and stuff like that
8:30 AM <acrichto> also now I may be confusing myself as to code coverage and profiling...
8:30 AM <mw> ok, sounds pretty clear that it's not ready yet then
8:31 AM <•nrc> yeah, this is about code coverage, which means -Cprofile may not be the best name
8:31 AM <•nrc> (I already registered a blocking concern about naming)
8:34 AM <•nrc> so, should we close the stabilisation PR and move discussion to the tracking issue?
8:34 AM <mw> sgtm
8:35 AM <japaric> +1
8:37 AM <•nrc> ok, RFCs, https://github.com/rust-lang/rfcs/pulls?q=is%3Aopen+is%3Apr+label%3AT-dev-tools
8:38 AM <•nrc> (just going to check over these as we're meant to be focussing on implementation now)
8:39 AM <•nrc> https://github.com/rust-lang/rfcs/pull/1990
8:39 AM <mw> I'd like to not stall gh2117 until after impl period as it's already open and rather straightforward
8:40 AM <mw> (but don't let me interrupt...)
8:40 AM <misdreavus> i think this still needs consensus/fiat on where the include directory is based?
8:41 AM <misdreavus> rfc1990, that is
8:41 AM <•nrc> yeah, still looking at that
8:41 AM <misdreavus> the discussion that happened in the last week didn't affect the rfc itself
8:42 AM <misdreavus> the last time it was brought up we had three different options and no discussion once everyone put their opinion in
8:42 AM <•nrc> https://github.com/rust-lang/rfcs/pull/1990#issuecomment-327073940
8:43 AM <•nrc> I think relative to src is the best solution here
8:43 AM <•nrc> and we can add `doc_root` later as an extension
8:43 AM <misdreavus> "clarification - rustdoc, not the compiler as I previously mentioned, correct?" this is correct
8:43 AM <misdreavus> nrc: sgtm
8:44 AM <•nrc> steveklabnik, imperio: thoughts?
8:44 AM — misdreavus ponders implementation details
8:44 AM <imperio> nrc: reading
8:45 AM <•nrc> I think this is ready to merge with that
8:45 AM <misdreavus> would adding `doc_root` need another rfc, or simply a follow-up pr?
8:46 AM <imperio> have we decided which folder would be the doc root?
8:46 AM <misdreavus> imperio: nrc just suggested using src/
8:46 AM <•nrc> I guess technically an RFC, but we could probably try something more lightweight like an issue + FCP
8:46 AM <imperio> ok, it seems fair
8:46 AM <imperio> I agree with src as root folder
8:47 AM <misdreavus> i.e. root it at wherever lib.rs is, or whatever file the crate root is
8:47 AM <•nrc> misdreavus: I think that if we think we will need doc_root, then we should add it to the RFC now and say it should have a separate feature flag.
8:47 AM <misdreavus> nrc: +1
8:47 AM <steveklabnik> nrc: hm
8:47 AM <misdreavus> if we're anticipating adding it later then we can just add it now
8:47 AM <•nrc> my view is that we probably won't want it, so it will be OK
8:47 AM <•nrc> OK not to add it now
8:48 AM <imperio> so I'm not sure to follow
8:48 AM <misdreavus> i can anticipate people wanting to use it
8:48 AM <imperio> do we add a root folder or not finally?
8:48 AM <misdreavus> especially with how many want to have docs/ parallel to src/
8:48 AM <misdreavus> imperio: no
8:49 AM <imperio> ok
8:49 AM <steveklabnik> i think src being the root makes the most sense
8:49 AM <•nrc> so, the alternatives we're considering are: src for the doc root, or src for the doc root by default + `doc_root` flag as an override
8:49 AM <•nrc> reading steve's comment it sounds like doc_root should be handled by the compiler?
8:50 AM <misdreavus> if i can bikeshed names: `doc-root` for cli flag, #![doc(include_root = "..")] for crate attribute
8:50 AM <misdreavus> supply one or the other, like we have for some other rustdoc flags
8:51 AM <•nrc> OK, since there is obviously some stuff to dicsuss for doc_root, I think we should not add it to the RFC now.  We can experiement in stabilisation if necessary
8:51 AM <•nrc> misdreavus: sound ok?
8:51 AM <misdreavus> +1
8:52 AM — misdreavus may take this on during impl period if someone else hasn't claimed it
8:52 AM <•nrc> cool!
8:52 AM <•nrc> https://github.com/rust-lang/rfcs/pull/2103 just needs merging
8:52 AM <•nrc> https://github.com/rust-lang/rfcs/pull/2117
8:52 AM <•nrc> mw ^
8:53 AM <•nrc> mw: do you think this is ready for FCP as is?
8:53 AM <•nrc> (we probably shouldn't given the impl period, but I think it is OK to cheat in this case - dev-tools didn't have the first half of the year to land RFCs)
8:53 AM <mw> I think a prototype for testing would be nice
8:54 AM <mw> don't know if that would stand in the way of FCPing
8:54 AM <•nrc> perhaps we could say we want to implement this as an experimental thing and FCP later?
8:54 AM ⇐ vertexclique quit (vertexcliqu@moz-mtv228.dip.versatel-1u1.de) Connection closed
8:54 AM <mw> sgtm
8:55 AM <•nrc> anyone else have thoughts on this one?
8:55 AM <•nrc> this is about making macros more debuggable
8:56 AM <•nrc> ok, I'll comment to that effect
8:56 AM <imperio> I never worked much with macro but any debug improvement has to be considered
8:56 AM <•nrc> anything we should cover from the tools?
8:56 AM <•nrc> I plan to make another issue of 'these weeks in dev tools' this week, so if you have news, please put it in the issue
8:57 AM <•nrc> https://github.com/nrc/dev-tools-team/issues/25
8:57 AM <acrichto> win 20
8:58 AM <•nrc> I'm going to be away from next week for 4.5 weeks. I propose that we cancel the proactive meetings, but keep the triage meetings. Does that sound good to people?
8:58 AM <mw> +1
8:58 AM <misdreavus> +1
8:58 AM <japaric> +1
8:58 AM <•nrc> I won't be around much on irc, but I'll be checking email - my nick at mozilla.com
8:58 AM <•nrc> so please email me if anythng comes up
8:59 AM <mw> enjoy your time off :)
8:59 AM <•nrc> ok, thanks everyone, we're done!
```
