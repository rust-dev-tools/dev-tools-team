# January 18, 2018

Triage week, irc meeting in #rust-dev-tools

## Notes

### ICE building docs of some crates in beta

* https://github.com/rust-lang/rust/issues/47639
* beta regression
* misdreavus to investigate

### RFCs

* checked in on multiple RFCs, comments on RFC threads

### Misc

* No meeting next week, then roadmap

## IRC log

```
9:00 PM <•nrc> OK, triage meeting - https://public.etherpad-mozilla.org/p/rust-dev-tools
9:00 PM <killercup> o/
9:02 PM <•nrc> We have a beta regression: https://github.com/rust-lang/rust/issues/47639
9:02 PM <steveklabnik> o/
9:02 PM <•nrc> imperio is assigned
9:02 PM <steveklabnik> imperio is also out until next week thanks to FOSDEM
9:02 PM <imperio> let me look
9:02 PM <steveklabnik> oh hey!
9:02 PM <imperio> yep :)
9:02 PM <steveklabnik> i thought you were gone, my bad :)
9:02 PM <imperio> ehy!
9:02 PM <imperio> no no
9:02 PM <imperio> I'm gone starting tomorrow
9:03 PM <imperio> but I'm super tired and trying to prepare my conference so don't expect me doing much
9:03 PM <•nrc> kennytm has identified a bad commit, but it looks suspicious
9:03 PM <•nrc> misdreavus or steveklabnik: would you be able to look into this before imperio is back?
9:04 PM <steveklabnik> misdreavus is better qualified than me, but also this is my job and not theirs :)
9:04 PM <kennytm> (nrc: it's legit)
9:04 PM <steveklabnik> so, if they're willing, that'd be best. otherwise, i can try to figure out whats up
9:04 PM <steveklabnik> no hard feelings either way!
9:04 PM <misdreavus> i haven't dug in much, so i don't really know the best avenue of attack
9:05 PM <misdreavus> there's also the fact that some recent nightlies are seemingly fixed?
9:05 PM <•nrc> kennytm: as in, you re-checked and you think the web asm PR is to blame?
9:05 PM <steveklabnik> yeah, but you've like, built rustdoc more recently than three months ago, heh
9:05 PM <misdreavus> lol
9:05 PM <kennytm> nrc: yes i've downloaded the CI artifacts and verified that e97ba83 causes the ICE
9:05 PM <kennytm> on macOS at least
9:05 PM <•nrc> kennytm: awesome, thanks!
9:06 PM <•nrc> misdreavus (et al): could adding a new target break rustdoc like this?
9:06 PM <steveklabnik> hm
9:07 PM <misdreavus> my immediate assumption
9:07 PM <kennytm> i think it is the change to the ReplaceBodyWithLoop stuff
9:07 PM <misdreavus> yeah, the "everybody loops" pass
9:07 PM <imperio> misdreavus: yep, it seems that one fix I wrote fixed it
9:07 PM <imperio> which is pretty funny haha
9:07 PM <misdreavus> wait, which one?
9:07 PM <imperio> good side effect
9:08 PM <imperio> no clue
9:08 PM <misdreavus> like, we should also bisect what fixed it so we can try to backport it
9:09 PM <imperio> misdreavus: actually that could also be one of your PRs too
9:09 PM <imperio> well, I'll let you look for it ;)
9:09 PM <misdreavus> heh
9:09 PM <•nrc> So, I'd like to move on, but it seems like bisect for the fix and try to backport would be a good strategy
9:10 PM — steveklabnik nods
9:10 PM <misdreavus> yes
9:10 PM <•nrc> misdreavus: if you could do that, that would be awesome!
9:10 PM <•nrc> kennytm or someone else from the infra team could probably help with the bisection if needed
9:10 PM <misdreavus> kennytm: can you try to bisect a fix? a broken/fixed pair of nightlies are in the comment thread
9:10 PM <steveklabnik> yeah, if imperio misdreavus want to give this a try, please do, and if you can't/takes too long, etc, let me know and ill give it a shot <3
9:10 PM <misdreavus> oh wait
9:11 PM <•nrc> No nominated PRs
9:11 PM <imperio> steveklabnik: consider me out for this one so it'll entirely depend on misdreavus 
9:11 PM <misdreavus> "rustc 1.25.0-nightly (8ccab7eed 2018-01-31)" also doesn't work ;_;
9:12 PM <misdreavus> eh, i'll take it out of chat for now
9:13 PM <•nrc> RFCs:
9:13 PM <•nrc> https://github.com/rust-lang/rfcs/pull/1133 is nominated for closing
9:14 PM <steveklabnik> :metal:
9:15 PM <killercup> *crickets*
9:15 PM <•nrc> https://github.com/rust-lang/rfcs/pull/1615 is still open
9:16 PM <•nrc> Oh great, tbu has updated it!
9:16 PM <•nrc> I missed that ping
9:17 PM <•nrc> to recap, this is an RFC about where tools (in particular rustup and Cargo) should put their files
9:17 PM <•nrc> if anyone interested could read the recent implementation and updates, that would be great!
9:18 PM <•nrc> https://github.com/rust-lang/rfcs/pull/2117
9:18 PM <•nrc> ping mw
9:19 PM <•nrc> I guess we are still waiting for someone to implement that
9:19 PM <•nrc> I wonder if anyone is interested in doing that
9:20 PM <steveklabnik> im betting we can get a volunteer
9:20 PM <steveklabnik> given so many people feel so strongly about it
9:23 PM <•nrc> https://github.com/rust-lang/rfcs/pull/2285
9:23 PM <•nrc> This RFC is an update to an existing one to reflect the implementation which actually happened
9:23 PM <•nrc> (rustdoc links)
9:24 PM <•nrc> ping killercup and Manishearth about this one
9:24 PM <steveklabnik> should we check of jntrnr
9:24 PM <•nrc> It looks like my review comments are addressed, thanks!
9:25 PM — killercup is reading manish's latest changes
9:25 PM <•nrc> So, where I'm a little confused is about the status of the feature
9:25 PM <•nrc> An RFC is usually a goal for what we want the feature to look like
9:25 PM <•nrc> in this case we're modifying an RFC to match what is implemented
9:25 PM <misdreavus> we accepted the rfc, but when implementing it we ran into difficulties getting it to match the rfc
9:26 PM <•nrc> but that is not what I'd like the feature to look like eventually, or at least I'd like to discuss that
9:26 PM <killercup> yeah it's about refining the rfc because of new ambiguities
9:26 PM <steveklabnik> nrc: it sounds like you'd like to object to merging here, then?
9:26 PM <•nrc> so I'm not sure whether we should make *this* RFC about just matching the implementation and land it quickly
9:26 PM <•nrc> or whether we should make it into a discussion about what we'd like to ahve
9:26 PM <steveklabnik> my understanding of this rfc was that there was pretty decent consensus that this *was* what the final impl should look like
9:27 PM <killercup> to me, this is just an update to make one of the cases the rfc proposed work
9:27 PM <killercup> this is about the "write an atcual markdown link to a rustdoc item"
9:27 PM <•nrc> we might have that discussion during stabilisation, but I don't think that is a thing for rustdoc, right?
9:28 PM <killercup> the currently not implemented but hopefully much more common case (implied ref links) is unchanged by this
9:28 PM <killercup> nrc: rustdoc might have a stabilization process
9:28 PM <killercup> this is behind a -Z/unstable flag iirc
9:28 PM <killercup> misdreavus correct me if i'm worng
9:29 PM <misdreavus> afaik it's not behind a command-line flag, just nightly-only
9:29 PM <misdreavus> like checking for error numbers in compile_fail doctests
9:29 PM <•nrc> what happens if there is a link in the docs and you run non-nightly rustdoc - it's just text?
9:29 PM <misdreavus> it treats it as link text
9:30 PM <killercup> yeah
9:30 PM <killercup> or, if you write an explicit link, it just doesn't work
9:30 PM <misdreavus> <a href="::my_crate::MyType">MyType</a>
9:31 PM <misdreavus> so, is this rfc pr just changing spaces to @-signs, and making "ambiguous namespace" errors explicit?
9:31 PM — misdreavus let Manishearth take over the implementation pr after a while
9:32 PM <killercup> misdreavus: basically, yeah, from what i recall these were the main changes
9:32 PM <misdreavus> where the former is because it was non-standard commonmark, and the latter because it was authorial intent?
9:33 PM <•nrc> it allows namespace-ambiguous links which we did not permit in the original RFC
9:33 PM <steveklabnik>  /win 19
9:34 PM <•nrc> which are very convenient but not forwards compatible
9:34 PM <•nrc> might be a good thing, but seems like a non-trivial change
9:35 PM <killercup> yeah. a) yes b) the rfc didn't specify whether to use warnings or errors. c) addition: no need to disambiguate stuff if there's only one match
9:35 PM <•nrc> well, it seems that there will be an opportunity to re-visit things during stabilisation, so we can treat this RFC as a bug fix, rather than an in-depth discussion
9:35 PM <killercup> we discussed the addition a bit and most people seem to think that clashes of names across namespaces are *really* rare
9:36 PM <misdreavus> and the current objection is one of process?
9:37 PM <•nrc> personally, I'm just not sure how hung up to get on this RFC - I'd be happy to quickly merge if we can discuss properly in stabilisation, I just didn't know if there will be a stabilisation period
9:37 PM <killercup> Manish's intend was to open this as a bugfix, not an actual RFC fyi
9:37 PM <misdreavus> nrc: my assumption was that we can treat "taking off the nightly-only gate" as its "stabilization"
9:38 PM <•nrc> ok, lets do that then
9:38 PM <misdreavus> cool
9:38 PM <•nrc> I'll summarise on the RFC, I'd encourage you to tick boxes on the basis of it being just a bug fix
9:38 PM <killercup> yay :)
9:39 PM <misdreavus> <3
9:39 PM <steveklabnik> :)
9:39 PM <•nrc> Can we close 2287 as subsumed by 2318?
9:39 PM <•nrc> I haven't read 2318 or the minutes from last week yet, sorry
9:40 PM <killercup> maybe but not really
9:40 PM <killercup> 2287 is about the API of the built-in benchmark thing
9:41 PM <killercup> 2318 is about a way to not need to have that built-in :)
9:41 PM <killercup> (and a lot of other things, obviously)
9:41 PM <•nrc> yeah, I was hoping we could say bench will be a test framework and we won't stabilise the current thing at all
9:41 PM <misdreavus> 2287 is kinda "stabilize what we have" and 2318 is "here's how we generalize it", aiui
9:41 PM <killercup> we could say "let's ship the bencher stuff as a crate once 2318" land, but might still want to have an rfc about the api for the nursery crate
9:42 PM <•nrc> I think it is better to have an implementation first, then an RFC, given that it sounds like people want change, not just stabilising what we have
9:42 PM <killercup> i'm fine with closing 2287, but fear that 2318 will take a long while to land
9:43 PM <killercup> 2287 is not really urgent or anything. benchmarking on stable is a solved problem with criterion from what i've heard
9:44 PM <•nrc> bench has been unstable for three years and I feel like this rush to stabilise it now is totally artificial (I agree it would be good to stabilise it, but it seems to have gone from 'not even on the radar' to 'highest priority' and I don't agree with that)
9:44 PM <steveklabnik> i agree
9:44 PM ← jntrnr left (sid714@moz-a8m5f4.0j4i.jtu0.0101.2620.IP): ""
9:44 PM <steveklabnik> it's a papercut for sure
9:45 PM <•nrc> ok, I'll write something up on the RFC there
9:45 PM <•nrc> That's all the triage
9:45 PM <•nrc> Does anyone have a nomination for next week's meeting?
9:46 PM <sfackler> test::black_box is the one bit that can't be exactly replicated outside on stable, but I don't know how far the approximations are from it
9:48 PM <killercup> sfackler: i was interested on that as well and found https://github.com/japaric/criterion.rs/commit/497ce56a0de8e15de792be61d98a15393208bd14
9:51 PM <killercup> i dont have nomination for next week's meeting myself but i think the fcp for the roadmap rfc will start then
9:51 PM <•nrc> OK, well, I'm away anyway, so I'd be happy to cancel
9:51 PM <•nrc> it would be good for everyone to look at the roadmap RFC
9:52 PM <killercup> maybe in two weeks it'd make sense to talk a bit about this team's roadmap for 2018?
9:52 PM <•nrc> anyone got anything else for today's meeting?
9:52 PM <steveklabnik> not i
9:52 PM <•nrc> killercup: good idea
9:53 PM <•nrc> OK, then the meeting ends
9:53 PM <•nrc> thank you everyone!
```
