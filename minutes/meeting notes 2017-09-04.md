# Septmeber 4th 2017

Triage week, irc meeting in #rust-dev-tools

## Notes

### Semver tool

* https://github.com/ibabushkin/rust-semverver
* GSoC project from twk, mentored by eddyb
* killercup and oli-obk to try it out and report back
* will then add to nursery and bring under dev-tools umbrella
* no plans to distribute with Rustup, etc. just yet


### Triage

#### Stabilise `compile_fail`

* https://github.com/rust-lang/rust/pull/43949
* Everyone present in favour of stabilisation


#### Stabilise `--print target-spec-json`

* https://github.com/rust-lang/rust/issues/38338
* Has had some use, nothing bad to say really, but seems like a band-aid solution
* Not much pressure to stabilise
* nrc to comment on issue


#### PR: `#[doc(masked)]`

* https://github.com/rust-lang/rust/pull/44026
* Hides traits from docs - used to prevent dead links in std docs
* Currently not optimal for all uses, but we'll land as is (and behind a feature flag) and iterate


#### RFC: platform-specific data directories

* https://github.com/rust-lang/rfcs/pull/1615
* nrc is trying to resurrect, comment if you have thoughts
* Hopefully ready for FCP soon


#### RFC: Add external doc attribute to rustc

* https://github.com/rust-lang/rfcs/pull/1990
* FCP is over
* Still need to decide on path base
* nrc to follow up


#### RFC: Attributes for tools, 2.0

* https://github.com/rust-lang/rfcs/pull/2103
* Entered FCP


## IRC log

```
7:18 AM <•nrc> triage meeting this week
7:18 AM <imperio>  /o
7:18 AM <imperio> great
7:18 AM <imperio> a few prs of mine are still waiting for people
7:18 AM <•nrc> and I guess it might be a bit quiet because labour day
7:19 AM <imperio> nrc: also, we need to talk a bit about what remains to be done on current rustdoc diff heper
7:19 AM <imperio> *helper
7:19 AM <•nrc> yes
7:20 AM <•nrc> jntrnr, japaric: ping meeting
7:20 AM <jntrnr> nrc: is holiday for folks in states
7:20 AM <jntrnr> guessing we're going to be missing people
7:21 AM — japaric is here :-)
7:21 AM <mw> not that many actually
7:21 AM <jntrnr> ahh, cool
7:21 AM <•nrc> OK, first thing before the triage things - eddyb has been mentoring a GSoC student who has been working on a semver tool
7:21 AM <jntrnr> I'm half paying attention this time. Trying to wrap up the survey results to go out tomorrow morning (yay!)
7:21 AM <eddyb> twk: ping
7:22 AM <twk> I'm here :)
7:22 AM <eddyb> (twk is the student in question)
7:22 AM <•nrc> twk: hi!
7:22 AM <twk> hi everyone!
7:22 AM <•nrc> do you have a link handy, sorry, don't have it to hand
7:22 AM <mw> hi
7:22 AM <twk> sure, sec
7:22 AM <eddyb> github.com/ibabushkin/rust-semverver/
7:22 AM <twk> https://github.com/ibabushkin/rust-semverver
7:22 AM <•nrc> thanks!
7:23 AM <•nrc> twk: so, current status is that it works and is useful, but is not complete?
7:23 AM <twk> well, it's feature complete as far as I am aware. some bugs and limitations remain, and features not strictly related to checking language features
7:24 AM <•nrc> oh great!
7:24 AM <twk> for instance, checking dependencies of a crate recursively would be a useful feature, but isn't done yet
7:25 AM <twk> the issues tagged with `enhancement` track those
7:25 AM <mw> nice, one could imagine running this automatically on `cargo publish`
7:25 AM <•nrc> dev-tools team: I would like to move this under the dev-tools umbrella - move it to the nursery, advertise it, etc. 
7:25 AM <killercup> wow this is awesome, twk! i've wanted a tool like this since at least 2014 :)
7:25 AM <•nrc> does that sound good to you all?
7:25 AM <twk> killercup: it's appreciated, thanks :)
7:25 AM <•nrc> twk: do you plan to continue to maintain the tool, now that twk is over?
7:25 AM <twk> I'm not over, but yes, I do.
7:25 AM <mw> :D
7:26 AM <•nrc> heh, sorry, I meant GSoC is over
7:26 AM <killercup> :D twoogle wummer of kode?
7:26 AM <•nrc> (it is early here :-p )
7:26 AM <•nrc> yeah, agreed, this is really cool!
7:26 AM <twk> currently busy for exams and stuff (for most of this month) - so I won't be able to spend as much time on it, but afterwards I'll be pretty free to tackle the remaining features
7:26 AM <twk> s/for/with/
7:26 AM <•nrc> twk: awesome
7:26 AM <twk> :)
7:27 AM <mw> does the tool depend on compiler internals a lot?
7:27 AM <eddyb> mw: vaguely yes
7:27 AM <twk> fairly. we use the type checker and the trait system
7:27 AM <killercup> +1 for move to nursery (though maybe i'd  like to actually test this first ;))
7:28 AM → oli_obk joined (smuxi@moz-ppe75h.hsi6.kabel-badenwuerttemberg.de)
7:28 AM <eddyb> killercup: acrichto did and found some bugs, so testing is welcome
7:28 AM <mw> so if the compiler changes, the tool needs to be updated?
7:28 AM <oli_obk> sorry I'm late
7:28 AM <eddyb> oli_obk: context: https://github.com/ibabushkin/rust-semverver/
7:28 AM <oli_obk> mw: yes, or disabled
7:28 AM <•nrc> mw, japaric, jntrnr: OK to move to nursery and welcome into our tool-y embrace? (I imagine we'd be  quite a way off distributing with Rustup or other 'official'-ness changes)
7:28 AM <twk> mw: so far developing against the most recent nightly has required 2-3 changes, most of them minor.
7:29 AM <•nrc> oli_obk: we're just talking about the nursery, not moving into the Rust repo
7:29 AM <twk> that's anecdotal evidence, I know
7:29 AM <oli_obk> nrc: ok, thx
7:29 AM <jntrnr> oh is this the checker?
7:29 AM <jntrnr> have we tried it out?
7:30 AM <eddyb> I think twk has a list of crates that have already been tested?
7:30 AM <•nrc> jntrnr: for some definition of 'we', yes
7:30 AM <jntrnr> lol, as long as a couple of us have tried it out and it works, then yeah I'm okay
7:30 AM <twk> serde works in the tests, some crates currently cause stack overflows, and some generate not-quite-deterministic output (but the cause is known and worked on)
7:30 AM <japaric> nrc: in favor but would like to see it get more testing -- though I suppose moving it to nursery would help with that
7:31 AM <mw> I'm OK with moving it to the nursery 
7:31 AM <eddyb> yeah I wish I had a better suggestion of a large crate than serde :P
7:31 AM <jntrnr> is there a capability bar we are applying?  is there a chance we will ship this as a default tool?
7:31 AM <eddyb> for the early testing
7:31 AM <•nrc> OK, let's a few of us try it out, and then we'll move it
7:31 AM <jntrnr> k
7:32 AM <mw> just in theory: would it be possible to implement this tool on top of the more stable APIs that the new rustdoc will be using?
7:32 AM <•nrc> jntrnr: the bar for the nursery has historically been quite low. Shipping with Rustup or whatever would be a long way off
7:32 AM <jntrnr> nrc: could be, though we waited a long time for rls
7:32 AM <•nrc> e.g., rustfmt has been in the nursery for a year, and is still not shipping by default
7:32 AM <jntrnr> so I guess let's do what you suggest
7:32 AM <jntrnr> have a couple people try it and give a thumbs up
7:33 AM <•nrc> other than killercup, anyone want to try it out and report back?
7:33 AM <oli_obk> where do we collect thumbs up?
7:33 AM — oli_obk will try it out
7:33 AM <eddyb> mw: it uses the type and trait system so doubtful?
7:34 AM <•nrc> thanks oli_obk !
7:34 AM <•nrc> killercup, oli_obk: could you report back to me by the end of the week?
7:34 AM <oli_obk> nrc: email?
7:34 AM <killercup> nrc: will do
7:34 AM <mw> eddyb: yes, I see that some things in the features list would be hard to do...
7:34 AM <•nrc> twk: I'll ping you about moving into the nursery and so forth early next week
7:34 AM <eddyb> mw: like, it does custom translation between generics and inside types and throw all that at an InferCtxt to check for mismatches
7:35 AM <twk> nrc: great!
7:35 AM <•nrc> oli_obk: sure, or irc, whichever is easier
7:35 AM <oli_obk> k
7:35 AM <eddyb> mw: a chalk interface would be cool though!
7:35 AM <•nrc> thanks!
7:35 AM <eddyb> mw: since a lot of this could be done with logic programming
7:35 AM <•nrc> ok, nominated issues
7:35 AM <•nrc> https://github.com/rust-lang/rust/issues/44237
7:35 AM <killercup> twk: can i ask some quick impl detail questions before looking at the code? :) my try at implementing a semantic diff tool was to use rustdoc's json output because it gave a pretty good description of the public api. now, the new rustdoc seems to require similar data and i think it uses rustc's save-analysis data (via rsl-analysis?). is that the same interface you are using?
7:35 AM <•nrc> rustdoc tests not being run - simulacrum is on this (thanks!)
7:36 AM <•nrc> https://github.com/rust-lang/rust/issues/38338
7:36 AM <•nrc> stabilize --print target-spec-json
7:36 AM <twk> killercup: no :P
7:36 AM <eddyb> killercup: no, this is what the compiler uses to look up names and types cross-crates
7:36 AM <•nrc> does anyone use this?
7:36 AM <•nrc> I didn't even know it existed, so I don't feel like I'm in a good place for an opinion re stabilisation
7:36 AM <eddyb> killercup: like, it's far more powerful than anything the compiler can export
7:37 AM <killercup> twk: eddyb: okay, might be interesting to see how much power this needs and if we can build one interface (read: break less often). but we can talk about that after the meeting
7:37 AM <japaric> nrc: I use that sparingly but I'm mainly a nightly user so I have no urge in stabilizing it
7:38 AM <eddyb> killercup: IMO the occassional breakage is 1000% worth it for the 100x in power
7:38 AM <mw> nrc: it seems to have a reasonable definition
7:38 AM <oli_obk> nrc: we use it (I think) in https://github.com/embed-rs/stm32f7-discovery/blob/master/stm32f7.json, but that's very nightly and breaks rarely enough (once every year)
7:38 AM <eddyb> killercup: like, unless you expose a datalog/chalk API, it won't really get anywhere close
7:38 AM <killercup> eddyb: heh, okay, i trust your judgment there :)
7:39 AM <•nrc> japaric, oli_obk: would you change it in any way? Could it be improved, or is it doing its job well?
7:40 AM <oli_obk> nrc: no opinion on it. it feels like a temporary solution though. Until something really worked out comes along
7:41 AM <•nrc> oli_obk: what is the long-term solution here?
7:41 AM <japaric> nrc: the fields of the json output are not stable because target specifications are not stable so it works but it breaks from time to time
7:41 AM <oli_obk> nrc: I just think there hasn't gone any real thought into the long-term plans on target specs. But I don't have a vision for it
7:42 AM <•nrc> hmm
7:42 AM <•nrc> do we have a tracking issue or anything for target specs?
7:42 AM <•nrc> (acrichto: this feels like it might be interesting for you, if you're around ^)
7:43 AM <•nrc> There was an RFC: https://github.com/rust-lang/rfcs/pull/131
7:45 AM <•nrc> Hmm, well, given that it seems to be a temporary solution and there is no great pressure to stabilise, I propose we don't for now. I'll comment on the issue
7:45 AM <japaric> nrc: +1
7:46 AM <•nrc> nominated PR: https://github.com/rust-lang/rust/pull/43949
7:46 AM <•nrc> stabilise `compile_fail`
7:47 AM <imperio> ah great :D
7:47 AM <imperio> finally
7:47 AM <oli_obk> I'm all for stabilizing it, if adding checks for concrete errors can be added backwards compatibly
7:47 AM <•nrc> I was basically trying to FCP this, needs signoff from the team
7:48 AM <•nrc> mw, killercup, jntrnr, japaric, steveklabnik: thoughts?
7:48 AM <misdreavus> o/
7:48 AM <jntrnr> looking
7:48 AM <killercup> woah, i just thought it was about compile_error!, which i was sure was stable in 1.20
7:49 AM <japaric> TIL that `compile_fail` exists in doc tests ...
7:49 AM <misdreavus> lol, i think a lot of people didn't know about this until i pointed it out :P
7:49 AM <jntrnr> reading through comments, did we address gankro's comment?
7:49 AM <misdreavus> about error codes?
7:49 AM <jntrnr> yeah
7:49 AM <misdreavus> those were left nightly-only
7:50 AM <jntrnr> sure, then I'm fine with it
7:50 AM <misdreavus> this would be only letting people have the ability to say "this doctest should not be able to compile"
7:50 AM <killercup> misdreavus: > Ok so I removed the error codes
7:50 AM <mw> lgtm
7:50 AM <misdreavus> killercup: the functionality is still there
7:51 AM <misdreavus> several doctests within the main docs use it
7:51 AM <imperio> I wanted to add error codes as well
7:51 AM <imperio> I don't agree with the arguments against it
7:51 AM <killercup> oh, then that comment is pretty misleading
7:51 AM <imperio> but too many people are opposed so let's see later on
7:51 AM <imperio> misdreavus: a lot actually
7:51 AM <misdreavus> yeah
7:51 AM <killercup> do we know the answer to "are error codes stable?"?
7:52 AM <imperio> all error codes have the error code check already
7:52 AM <imperio> killercup: not sure to understand your question
7:52 AM <•nrc> killercup: they really should be, but I think they are not
7:52 AM <misdreavus> it's about whether we can add "this specific error code always means this" to the stability guarantee
7:52 AM <misdreavus> right now, they are not part of that
7:53 AM <killercup> i would be quite surprised if we could guarantee that the same code fails to compile for the same reason in 1.0 and 1.20 actually
7:53 AM <imperio> misdreavus: hum normally yes
7:53 AM <misdreavus> also there's the argument that code could fail to compile for a different reason
7:53 AM <imperio> when we change the meaning of an error code, we create a new one
7:53 AM <misdreavus> or that code that fails to compile in one version starts compiling in a later one
7:53 AM <imperio> I had an argument with brson about this and then I came to agree with him
7:54 AM <•nrc> yeah, its kind of a grey area because we change the error code text without making a new error code, and there is no black/white line between that and changing the meaning of a code
7:54 AM <misdreavus> for that last one, the docs team asked to include a line to the compile_fail docs about it
7:54 AM <misdreavus> (that's been done and is part of this pr)
7:55 AM <•nrc> japaric: OK with you to stabilise?
7:55 AM <japaric> nrc: yeah, no objection
7:55 AM <killercup> fyi, i'm fine with stabilizing this with no error code check but i'm not sure how useful it is
7:55 AM <•nrc> thanks!
7:55 AM <imperio> killercup: a lot of people would disagree
7:55 AM <•nrc> just need to ping Steve, then this is ready to go
7:55 AM <imperio> it allows you to keep having a doc up-to-date
7:55 AM <killercup> is this rendered in a special way in rustdoc right now?
7:55 AM <imperio> nope
7:56 AM <killercup> hmmmmm
7:56 AM <imperio> why would it?
7:56 AM <misdreavus> the same way we would want `ignore` doctests to show up differently
7:56 AM <killercup> because in contrast to all other code examples it does NOT compile?
7:56 AM <misdreavus> if something is meant to not compile, it doesn't matter what prose is around it, people will *still* copy/paste from it
7:56 AM <killercup> imperio: ^ yes what misdreavus said
7:56 AM <imperio> misdreavus: we do? O.o
7:57 AM <imperio> ah
7:57 AM <imperio> we'll need to talk about it then...
7:57 AM <misdreavus> the discussion about applying a different style to `ignore` tests has come up before
7:57 AM <imperio> ah x 2
7:57 AM <misdreavus> by making compile_fail public, this could bring that discussion up again
7:57 AM <killercup> maybe a topic for the docs meeting :) (ping me when it starts even though i'm probably at a meetup)
7:57 AM <•nrc> I agree there probably should be rendering differences, but I don't think it needs to block stabilisation of the testing/semantic feature
7:58 AM <misdreavus> killercup: will do
7:58 AM <imperio> I have an idea in order to do it
7:58 AM <imperio> pretty simple to put in place
7:58 AM <•nrc> RFCs
7:58 AM <misdreavus> imperio: sweet :)
7:58 AM <•nrc> japaric: did you get feedback on your Xargo RFC?
7:59 AM <japaric> nrc: sorry, I haven't talk to the cargo team yet. Been meaning to review the Xargo RFC since it's been a while since I wrote it but haven't been able to find a time slot for that
7:59 AM <•nrc> https://github.com/rust-lang/rfcs/pull/1615 - is about using XDG for tools like Cargo, rustup. I've been resurrecting it, I hope we can FCP soon
8:00 AM <•nrc> we seem to be getting to the nitty-gritty, so I hope we can FCP soon. Now would be a good time to re-visit if you care about this stuff
8:00 AM <•nrc> japaric: np, let me know if I can do anything to help move it along
8:01 AM <•nrc> https://github.com/rust-lang/rfcs/pull/1990 - include docs - FCP is done, but we wanted to resolve the path rooting problem
8:02 AM <japaric> nrc: it's blocked on me so I just need to find some time to move it along
8:02 AM <•nrc> cool
8:03 AM <•nrc> if people have thoughts on the doc root, it would be good to comment on #1990 - it doesn't seem like it is naturally resolving
8:04 AM <•nrc> https://github.com/rust-lang/rfcs/pull/2103 tool attributes
8:04 AM <•nrc> japaric: just waiting on you ^ Did you have reservations?
8:05 AM <•nrc> https://github.com/rust-lang/rfcs/pull/2117 - Debuggable macro expansions - this exists, take a look if you can. I don't think we need to take any action right now
8:05 AM <japaric> nrc: I have yet to check the RFC in detail -- I think I can do that after this meeting
8:05 AM <•nrc> japaric: thank you!
8:06 AM <•nrc> OK, let's check-in on the rustdoc Pulldown/Hoedown thing
8:06 AM <•nrc> latest news is that the warning stuff landed \o/
8:06 AM <•nrc> thanks to imperio and simulacrum for lots of work there!
8:06 AM <misdreavus> what an ordeal o_O
8:06 AM <imperio> simulacrum is my hero
8:07 AM <•nrc> tracking issue for this whole thing is https://github.com/rust-lang/rust/issues/44229
8:07 AM <imperio> nrc: with your PR, what remains to be done?
8:07 AM <•nrc> I have a few little things to fixup from my PR - address some comments from ollie27, but they are minor and I'll do that today
8:08 AM <imperio> nothing else otherwise?
8:08 AM <imperio> the false positives have been removed?
8:09 AM <•nrc> what needs to be done (before we start the warning cycle): we need to assess the impact of the warnings - are people going to get hit with lots of false positives? Or overwhelmed with true positive warnings? We also need to do some PSAs to make sure this is well-advertised
8:09 AM <•nrc> imperio: improved, but not removed, afaict
8:09 AM <imperio> ok because I have a few ideas on how to fix this issue
8:10 AM <•nrc> https://gist.github.com/nrc/e4ba8c465a5174daa616361705bf14c1 is sample output for std
8:11 AM <•nrc> I think we're doing much better
8:11 AM <•nrc> seems like where we are getting a lot of noise is where we get a true error and then the diff gets 'out of sync' and reports some false positives
8:12 AM <•nrc> also, this kind of thing: https://gist.github.com/nrc/e4ba8c465a5174daa616361705bf14c1#file-rustdoc-warnings-2-txt-L159-L163
8:13 AM <•nrc> anyway, I feel like this is mostly unknown - the most important thing to me seems to be understanding how much of a problem this will be
8:14 AM <•nrc> it would be good to run the warnings on some 'big' crates and see what the output is like
8:14 AM <misdreavus> may be worth running this on rocket, that always seems like a stress test for rustdoc
8:14 AM <misdreavus> as one example
8:15 AM <•nrc> imperio, misdreavus: if you could do that, or mentor someone to do that (I think it might make a good first issue for rustdoc), that would be great. Otherwise, I can probably get to it next week
8:15 AM <misdreavus> to get some sample runs?
8:15 AM → plietar_ joined (plietar@moz-kvqs4v.ldb1.rbl2.23c4.2a00.IP)
8:15 AM <•nrc> misdreavus: yeah
8:15 AM <•nrc> rocket + a few other well-documented crates
8:15 AM <imperio> we should put a list somewhere
8:16 AM <misdreavus> alright, if i get to it i'll see about pulling up a list
8:16 AM <•nrc> cool, thank you!
8:17 AM <imperio> misdreavus: thanks
8:17 AM <imperio> I'll be able to test my ideas on it
8:17 AM <•nrc> imperio: anything else you want to discuss here?
8:17 AM <imperio> it was the only thing
8:17 AM <•nrc> cool
8:17 AM <imperio> I feel super relieved now that this is finally merged
8:17 AM <imperio> oh wait
8:17 AM <•nrc> :-)
8:17 AM <imperio> I think I have another pr requiring opinions
8:18 AM <imperio> ah no, not from dev-tools
8:18 AM <imperio> all good for me :)
8:18 AM <misdreavus> i did want to put one of mine to the team
8:18 AM <imperio> misdreavus: go ahead while we're around
8:18 AM <misdreavus> !gh 44026
8:18 AM <rustbot> [PR 44026] <open> [WIP] hide internal types/traits from std docs via new #[doc(masked)] attribute <https://github.com/rust-lang/rust/pull/44026>
8:18 AM <misdreavus> this one's in a bit of limbo atm
8:19 AM <misdreavus> basically, this is about taking out internal types from the std docs
8:19 AM <misdreavus> i did this by introducing a new doc attribute parameter
8:19 AM <•nrc> OK, next week is an agenda week. jntrnr suggested discussing  editor extensions and getting wider support for the RLS. We could invite people working on this or interested in it. I think this is a great idea! Is it good for the rest of you?
8:19 AM <imperio> misdreavus: cc me on it, I'll look tomorrow :)
8:19 AM <misdreavus> i put up a feature gate for it, and in the docs and tests for it i had the assumption that i could mask std like this and get everything that re-exported, and hit re-export bugs
8:20 AM <japaric> nrc: sgtm
8:20 AM <misdreavus> nrc: sounds good
8:20 AM <mw> nrc: +1
8:20 AM <killercup> nrc: +1
8:20 AM <misdreavus> so i'm wondering if doc(masked) can be merged as-is (and take out or modify the tests) or if we need to block this on solving re-exports
9:05 AM <killercup> misdreavus: i'm a bit confused about masked. can you link to an example of that? you `#[doc(masked)] extern crate libc;` – which rustdoc-rendered page is affected by that?
9:06 AM <misdreavus> killercup: the page for Copy is a big example of this
9:06 AM <killercup> nrc: btw, do you plan on releasing a new rls-vscode version? i'd love to get back onto the stable track of that :)
9:06 AM <misdreavus> today : https://doc.rust-lang.org/nightly/std/marker/trait.Copy.html#implementors
9:06 AM <misdreavus> with doc(masked) : https://tonberry.quietmisdreavus.net/std-43701/std/marker/trait.Copy.html#implementors
9:07 AM <killercup> misdreavus: ah, i see
9:08 AM <killercup> interesting feature. not sure if useful outside of std, but that shouldn't matter at first
9:09 AM <misdreavus> yeah, that's why i think it has merit even with re-export weirdness
9:09 AM <misdreavus> std is what it was made for, and it works there
9:21 AM <•nrc> misdreavus: I'm not clear - is this a probelm with the tests only, or with the feature?
9:21 AM <•nrc> killercup: yeah, I'll do another release this week
9:21 AM <•nrc> sorry, lost my internet for a while there
9:21 AM <•nrc> meeting is over, thanks everyone!
```
