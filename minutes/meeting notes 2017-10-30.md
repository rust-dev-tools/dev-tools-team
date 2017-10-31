# October 30th, 2017

Triage week, irc meeting in #rust-dev-tools

## Notes

### Implement RFC 1990 - include external files in docs

* https://github.com/rust-lang/rust/pull/44781
* misdrevious to investigate expanding at macro-expansion time, rather than in rustdoc.

### impl period check-in

* There's been lots of action on the RLS, rustfmt, new rustdoc, and old rustdoc
* nrc plans to try and spark a second wave of involvement soon

### Next weeks meeting

Will be on Cargo plans and tool needs, Matklad to write an agenda.

### Meeting time

Has changed to 1pm PDT.

## IRC log

```
8:04 PM <•nrc> OK, then let's triage!
8:04 PM <•nrc> There are no nominated issues
8:05 PM <•nrc> one nominated PR: https://github.com/rust-lang/rust/pull/44781
8:05 PM <misdreavus> that's me
8:05 PM <misdreavus> i posted a summary comment since this has been open a while https://github.com/rust-lang/rust/pull/44781#issuecomment-338426364
8:06 PM <misdreavus> basically: i'm stuck and looking for opinions
8:06 PM — killercup reads
8:07 PM <misdreavus> acrichto commented in one infra ping that he "wouldn't personally bank on include_str! in an attribute ever working" so i dunno how feasible that is >_>
8:08 PM <•nrc> so, the current approach is that you preserve the attribute through the compiler and then rustdoc pulls in the file?
8:08 PM <misdreavus> yes
8:08 PM <•nrc> And this is causing a problem because the attribute might be 'moved' by a macro expansion?
8:09 PM <misdreavus> or a re-export
8:09 PM <•nrc> hmm, I don't know how docs for re-exports work
8:09 PM <misdreavus> the #[doc] attribute usually just gets handed off wholesale
8:09 PM <misdreavus> alongside any other attributes
8:10 PM <•nrc> could you expand the attribute to the file at macro expansion time instead of leaving it for rustdoc?
8:10 PM <misdreavus> intra-crate re-exports have more complicated mechanics, that are handled by #[doc(inline)]
8:10 PM <killercup> can we preprocess certain attrs before handing them off to rustdoc (but only then), like make the path absolute or include the file
8:10 PM <misdreavus> nrc: maybe? i don't know how much effort that woudl take, nor where that code is
8:11 PM <misdreavus> killercup: that soudns like hell for reproducible builds
8:11 PM <•nrc> I feel like that is probably the only way to work with macro 'hygiene'
8:11 PM <steveklabnik> heh :/
8:11 PM <misdreavus> "including the file" may work
8:11 PM <misdreavus> but again, requires basically an extra step within the compiler, outside of rustdoc
8:11 PM <killercup> misdreavus: you mean hell as in `include_str!("~/lol/hell")`? :D
8:11 PM <misdreavus> lol
8:11 PM <misdreavus> pretty much, eyah
8:12 PM <•nrc> we already normalise doc comments and doc attributes to attributes early, I'd expect we could include the file then?
8:12 PM <misdreavus> de-sugaring doc comments is just another function applied separately for things that want to handle them, iirc
8:13 PM <misdreavus> i may be thinking about somethign else tho
8:13 PM <•nrc> I think they are always desugared at some point
8:13 PM <misdreavus> i'm thinking of the "with_desugared_doc" function in libsyntax
8:13 PM <misdreavus> (which rustdoc uses to normalize doc comments)
8:14 PM <misdreavus> in fact i even had to modify that function for this pr https://github.com/rust-lang/rust/pull/44781/files#diff-21e1f9f42b7976b50c28979848df2e04
8:16 PM <•nrc> ok, so, I think that leaving this to rustdoc won't work at least with macro expansion, so you will need to move to either early expansion, or using some kind of macro hygiene with the attribute
8:16 PM <•nrc> but I don't know if the macro hygiene approach would work for re-exports
8:17 PM <misdreavus> -_-
8:17 PM <•nrc> you'd need to preserve the original span when doing the re-export handling
8:17 PM <misdreavus> as long as the span comes out fine when the file is loaded then that's not a problem
8:18 PM <•nrc> misdreavus: I would ping jseyfried and ask him about the two approaches - he knows the attribute and expansion code better than anybody
8:18 PM <misdreavus> and by "early expansion" would that involve loading the file in then? or just canonicalizing the path?
8:19 PM <misdreavus> nrc: alright, will do
8:19 PM <•nrc> misdreavus: I think you could do either, I *think* they are equivalent
8:20 PM <misdreavus> semantically i prefer loading in the file then, but i dunno if that's kosher with what people what libsyntax to do
8:20 PM <misdreavus> (or wherever that would happen)
8:21 PM <misdreavus> cf. my comment about reproducible builds earlier
8:21 PM <•nrc> I don't think it affects reproducability. jseyfried can probably tell you if there is a difference from the libsyntax perspective
8:22 PM <•nrc> OK, not many RFCs because impl period. Only one of note is https://github.com/rust-lang/rfcs/pull/1615 which is still open
8:22 PM <misdreavus> hmm, i guess the doc attributes would get discarded in the final binary
8:23 PM <killercup> misdreavus: does this compromise reproducible builds in a way that include_str doesn't already? doc comments can always be omitted in release builds i guess
8:24 PM <•nrc> I'm pretty sure nothing from a comment or attribute ends up in the final binary
8:24 PM <•nrc> I mean unless the attribute affects compilation, obvs
8:24 PM <misdreavus> killercup: it's the same as include_str if you just load the file in at compile-time, but if you just give it a full path then that path winds up in the attribute
8:24 PM ⇐ blank_name4 quit (blank_name@moz-jtlp8s.mi.frontiernet.net) Ping timeout: 121 seconds
8:24 PM <misdreavus> yeah, that makes more sense
8:24 PM <•nrc> The RFC is changing the paths for tools
8:24 PM <•nrc> I think that it just needs work on the RFC to see if it can all work. I'll ping tbu...
8:25 PM <•nrc> OK, any other triage issues people know about?
8:25 PM <misdreavus> that's all i had
8:26 PM <steveklabnik> there's some new rustdoc blockers/weridness in the rls
8:26 PM <steveklabnik> but we don't have to have meeting time about it
8:26 PM <•nrc> steveklabnik: 'new rustdoc' I guess?
8:27 PM <steveklabnik> yes
8:27 PM <steveklabnik> all enum variants are being reported as tuples, many reported twice, that kind of thing. just bugs
8:27 PM <•nrc> cool, lets discuss later
8:27 PM <steveklabnik> it's a sort of blocker on some of my work, which is why i bring it up; you and i can talk async though
8:27 PM → blank_name4 joined (blank_name@moz-ms6nj1.mi.frontiernet.net)
8:27 PM <•nrc> I plan to address some of the rustc bugs today, could you comment on any issues which are blocking you and I'll prioritise them
8:28 PM <steveklabnik> yes
8:28 PM <•nrc> thanks
8:28 PM <steveklabnik> <3
8:29 PM <•nrc> misc news - jntrnr is leaving the dev tools team. He has been transitioning off dev tools stuff to work on Servo, so he's vacating his spot on the team to reflect that
8:29 PM <•nrc> thanks jntrnr for all your work!
8:29 PM <misdreavus> <3 jntrnr 
8:29 PM <steveklabnik> <3 jntrnr
8:29 PM <killercup> jntrnr: <3 jntrnr 
8:32 PM <•nrc> I'd like to publish a new issue of the dev tools newsletter, so if people have news, please add it to https://github.com/nrc/dev-tools-team/issues/28
8:32 PM <steveklabnik> cool cool
8:33 PM <•nrc> that can include releases, impl period stuff, thanks to contributors, anything vaguely interesting
8:33 PM <•nrc> thanks!
8:33 PM <•nrc> finally, how's the impl period going?
8:34 PM <•nrc> I've been a bit slack on rustfmt and RLS stuff because I've been away, but I did notice a bunch of new contributors and we landed some good PRs
8:34 PM <•nrc> I'd like to try and reignite that fire a bit in the next week or so
8:34 PM <steveklabnik> we've had some great stuff in new-rustdoc; we have pluggable frontends, modulo rls bugs we now produce nested info for everything properly
8:34 PM <•nrc> wow, cool!
8:35 PM <misdreavus> had a couple new contributors to vintage-rustdoc but my two major rfc implementations stalled either before or during implementation >_>
8:36 PM <misdreavus> need to get back on it, actually
8:36 PM <•nrc> are there more issues for the new old rustdoc people to work on?
8:36 PM <misdreavus> there are a ton of issues on old rustdoc, but i haven't pulled out any new mentored ones
8:37 PM <jntrnr> thanks all!
8:37 PM — jntrnr disappears back into his meeting
8:37 PM <•nrc> misdreavus: if you have time, it might be worth 'recommeding' an issue or two to the new people
8:38 PM — killercup adds 'vintage rustdoc logo' to his list, after '"lifetimes are descriptive not prescriptive" beermat'
8:38 PM <misdreavus> nrc: yeah, that's what i was referring to when i said i needed to get back into it
8:38 PM <misdreavus> i've been distracted >_>
8:39 PM <misdreavus> killercup: add "neo-rustdoc" to that list, that's been my occasional nickname for steveklabnik's project
8:39 PM <killercup> misdreavus: oh yes!
8:39 PM <•nrc> misdreavus: ah, I see, no worries
8:39 PM <steveklabnik> ahhhhhhhhhh i like neo-rustdoc
8:40 PM <misdreavus> so when we do the official switch-over we need to commission a couple logos to commemorate it? >_>
8:41 PM <steveklabnik> heheh
8:41 PM <steveklabnik> they should be like NASA flight patches
8:41 PM <misdreavus> omg yes!!
8:42 PM <•nrc> ok, actually, that wasn't the final item, because finally finally we should decide on a topic for next weeks meeting
8:43 PM <•nrc> oli-obk posted on the etherpad about discussing tools and the Rust CI
8:43 PM <•nrc> that sounds like it is worth discussing
8:43 PM <•nrc> sound good to people?
8:43 PM <misdreavus> iirc we talked about that last time?
8:43 PM <killercup> oli_obk_: ^
8:44 PM <•nrc> oh you did? (Haven't read the minutes yet)
8:44 PM <mw> yup
8:44 PM <misdreavus> yeah
8:44 PM <•nrc> any open questions or all settled?
8:45 PM <mw> I think there was room for improvement
8:45 PM <misdreavus> 21:19 in the log here https://github.com/nrc/dev-tools-team/blob/master/minutes/meeting%20notes%202017-10-16.md
8:45 PM <•nrc> matklad: do you think it is worth talking about the proposed Cargo changes and tools interactions?
8:46 PM <matklad> Yeah!
8:46 PM <•nrc> ok, cool, does that sound like a good topic for everyone else?
8:46 PM <matklad> One thing cargo team needs to start the work is data about which info tools need from Cargo to work. 
8:46 PM <mw> +1
8:46 PM <matklad> I'll add q to etherpad
8:47 PM <japaric> +1
8:47 PM <•nrc> OK, great, let's do that next week
8:47 PM <•nrc> matklad: could you plan an agenda please?
8:48 PM <matklad> Yeah, I think I could!
8:48 PM <•nrc> thanks!
8:48 PM <•nrc> OK, meeting is complete. Thank you everyone!
```
