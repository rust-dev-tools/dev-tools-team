# October 16th 2017

Triage week, irc meeting in #rust-dev-tools

## IRC log

```
[2017-10-16 21:00:13] <misdreavus> o/
[2017-10-16 21:00:53] <japaric> o/
[2017-10-16 21:00:53] <mw> hey everyone, meeting time!
[2017-10-16 21:01:17] <steveklabnik> o/
[2017-10-16 21:01:23] <mw> let's wait another couple of minutes for people to show up
[2017-10-16 21:01:23] <imperio> o/
[2017-10-16 21:01:28] <matklad> o/
[2017-10-16 21:02:29] <mw> alright, do we have oli_obk_  here today?
[2017-10-16 21:02:36] <projektir> o/
[2017-10-16 21:03:21] <mw> maybe not yet
[2017-10-16 21:03:50] <mw> if they show up, maybe they'll want to talk about tool sub-projects in the rust repository
[2017-10-16 21:04:00] <mw> as listed in the etherpad
[2017-10-16 21:04:31] <mw> otherwise I think you folks have nominated a few things, so let's go through these
[2017-10-16 21:04:44] <mw> https://github.com/rust-lang/rust/issues?q=is%3Aopen+is%3Aissue+label%3AT-dev-tools+label%3AI-nominated
[2017-10-16 21:04:51] <mw> these are the regular nominated issues
[2017-10-16 21:04:58] <mw> of whihc there's only one:
[2017-10-16 21:05:08] <mw> https://github.com/rust-lang/rust/issues/36342
[2017-10-16 21:05:19] <mw> we talked about this last time briefly
[2017-10-16 21:05:30] <mw> I've since chatted with aturon
[2017-10-16 21:05:49] <mw> the lang team will discuss this in a meeting of theirs before we can make progress here
[2017-10-16 21:06:20] <mw> next up we have some PRs:
[2017-10-16 21:06:22] <mw> https://github.com/rust-lang/rust/pulls?q=is%3Aopen+is%3Apr+label%3AT-dev-tools
[2017-10-16 21:06:43] <mw> let's start with gh44138
[2017-10-16 21:06:47] <mw> https://github.com/rust-lang/rust/pull/44138
[2017-10-16 21:06:57] <mw> Deprecate several flags in rustdoc
[2017-10-16 21:07:10] <mw> steveklabnik: is there still something left to do here?
[2017-10-16 21:07:11] <misdreavus> this was brought up last time, looks to be all set as far as team interaction is concerned
[2017-10-16 21:07:26] <steveklabnik> yeah i believe that it's all set
[2017-10-16 21:07:28] — steveklabnik re-reads
[2017-10-16 21:07:45] <misdreavus> kennytm made a comment linking to your previous note about making sure it gets tests
[2017-10-16 21:07:49] <misdreavus> **tested
[2017-10-16 21:07:52] <steveklabnik> yeah
[2017-10-16 21:07:57] <steveklabnik> i will do so
[2017-10-16 21:08:13] <mw> ok, otherwise I think the team is onboard with this
[2017-10-16 21:08:25] <mw> modulo nrc but I don't think he'd object
[2017-10-16 21:08:40] → oli_obk joined (smuxi@moz-ppeie4.hsi14.kabel-badenwuerttemberg.de)
[2017-10-16 21:08:50] <oli_obk> aloha
[2017-10-16 21:08:56] <mw> oli_obk: hi
[2017-10-16 21:09:15] <steveklabnik> yeah, i originally discussed this stuff with nrc
[2017-10-16 21:09:28] <mw> oli_obk: would you like to talk about miri and other tools in a minute?
[2017-10-16 21:09:30] ⇐ oli_obk quit (smuxi@moz-ppeie4.hsi14.kabel-badenwuerttemberg.de): Connection closed
[2017-10-16 21:09:40] <mw> gone again :)
[2017-10-16 21:09:52] <mw> let's continue with PRs meanwhile: https://github.com/rust-lang/rust/pull/45324
[2017-10-16 21:10:14] → oli_obk joined (smuxi@moz-ppeie4.hsi14.kabel-badenwuerttemberg.de)
[2017-10-16 21:10:16] <mw> enabling warnings about pulldown changes in rustdoc, if I'm not mistaken
[2017-10-16 21:10:19] <oli_obk> mw yes
[2017-10-16 21:10:51] <mw> oli_obk: great. we'll finish discussing PRs and then you're up if that's ok with you
[2017-10-16 21:10:57] <misdreavus> yeah, this is part of {!gh 44229}
[2017-10-16 21:10:57] <rustbot> [Issue 44229] <open> Tracking issue (Rustdoc): hoedown -> pulldown migration <https://github.com/rust-lang/rust/issues/44229>
[2017-10-16 21:11:04] <misdreavus> so, recap:
[2017-10-16 21:11:18] <imperio> mw: yep
[2017-10-16 21:11:23] <misdreavus> i did some investigation on some common crates to see whether we could find any more false positives after the first set
[2017-10-16 21:11:32] <misdreavus> any warnings i saw were all legit differences
[2017-10-16 21:11:56] <mw> ok, sounds good
[2017-10-16 21:11:59] <misdreavus> so, with that in mind, we catalogued the ones we've seen, imperio wrote up a blog post with the big/common ones, and we're ready to flip the switch to print the warnings all the time
[2017-10-16 21:12:27] <imperio> ~o~
[2017-10-16 21:12:39] <misdreavus> this will still use hoedown by default to render the markdown, but it will warn users about the pending changes
[2017-10-16 21:12:45] <mw> do we need to do anything else to raise awareness before enabling warnings unconditionally?
[2017-10-16 21:12:59] <imperio> mw: well, generally people don't care about docs
[2017-10-16 21:13:07] <imperio> the ones who care already know
[2017-10-16 21:13:07] <misdreavus> short of blasting the blog post as far and wide as we can, this is the best way
[2017-10-16 21:13:17] <imperio> and yes, the blog post
[2017-10-16 21:13:24] <imperio> otherwise I don't see any better way?
[2017-10-16 21:13:32] <misdreavus> afaik it hasn't made a publicity round, so i want to sync it with the pr
[2017-10-16 21:13:33] <mw> do the warnings give a link to the tracking issue?
[2017-10-16 21:13:35] <misdreavus> yes
[2017-10-16 21:14:04] <mw> enabling the warnings seems like the next  logical step then
[2017-10-16 21:14:07] <misdreavus> cool
[2017-10-16 21:14:17] <mw> any concerns from anyone?
[2017-10-16 21:14:22] <mw> steveklabnik maybe?
[2017-10-16 21:15:39] <mw> imperio, misdreavus: should we just assign the PR to steveklabnik to give the approval?
[2017-10-16 21:15:48] <mw> that should cover the main stakeholders
[2017-10-16 21:15:51] <imperio> no clue
[2017-10-16 21:15:55] <misdreavus> sure
[2017-10-16 21:16:07] <mw> alright, I'll do that
[2017-10-16 21:16:09] <imperio> nrc and misdreavus were the ones working on it the most
[2017-10-16 21:16:11] <misdreavus> if no one from dev-tools proper has an objection then i'm okay with that
[2017-10-16 21:16:20] <imperio> but steveklabnik is a good option as well
[2017-10-16 21:16:35] <misdreavus> just to rope in all the rustdoc peers
[2017-10-16 21:17:42] <steveklabnik> i don't have any specific concerns
[2017-10-16 21:17:53] <misdreavus> :shipit:
[2017-10-16 21:18:06] <mw> steveklabnik: assigned to you for r+ing
[2017-10-16 21:18:07] <imperio> ~(-.-)~
[2017-10-16 21:18:21] <mw> very good
[2017-10-16 21:18:22] <steveklabnik> cool; my day is almost over, so i'll put it on my tomorrow list
[2017-10-16 21:18:23] <misdreavus> gonna flood twitter explaining this one all over again :3
[2017-10-16 21:18:25] <steveklabnik> but tomorrow for sure :)
[2017-10-16 21:18:51] <mw> misdreavus: excellent :)
[2017-10-16 21:19:08] <mw> ok, that's today's PRs, I think
[2017-10-16 21:19:22] <imperio> misdreavus: thanks, I'm completely out of twitter so really appreciate :)
[2017-10-16 21:19:23] <mw> oli_obk: are you ready?
[2017-10-16 21:19:26] <oli_obk> jop
[2017-10-16 21:19:36] <mw> great, you have the stage
[2017-10-16 21:19:40] <misdreavus> imperio: that's me, the rustdoc ambassador to twitter :P
[2017-10-16 21:19:42] <oli_obk> so. I'm not sure if anyone has heard of "toolstate.toml"
[2017-10-16 21:20:08] <oli_obk> it's the cool new thing that exists so we can add *nightly*-using tools to rust ci
[2017-10-16 21:20:29] <imperio> misdreavus: the rustdoc ambassador is a nice title
[2017-10-16 21:20:35] <imperio> :p
[2017-10-16 21:20:44] <oli_obk> usually that's a big problem, because PRs will break the tool, and then you need to first fix the tool and update the tool's submodule in the rust repository
[2017-10-16 21:21:04] <oli_obk> the toolstate.toml allows you to disable a tool without fixing it
[2017-10-16 21:21:16] <steveklabnik> nice
[2017-10-16 21:21:20] <oli_obk> let me grab the link, one sec, so you can see how it looks
[2017-10-16 21:21:53] <oli_obk> https://github.com/rust-lang/rust/blob/master/src/tools/toolstate.toml
[2017-10-16 21:22:11] <oli_obk> as you can see on the bottom, we have clippy, miri, rls and rustfmt
[2017-10-16 21:22:40] <oli_obk> you can configure each tool as "Broken", "Compiling" or "Testing"
[2017-10-16 21:22:57] <oli_obk> the tool is built and tested irrelevant of the toolstate
[2017-10-16 21:23:14] <oli_obk> the only difference is whether CI and local testing will error
[2017-10-16 21:23:35] <oli_obk> if your tool is set as building, and it tests successfully, your build will error
[2017-10-16 21:23:54] <oli_obk> if the tool is set to testing, but doesn't test successfully, the build breaks
[2017-10-16 21:24:08] <oli_obk> so you have some control e.g. for clippy, which usually just breaks in the tests
[2017-10-16 21:24:33] <oli_obk> if you break a tool, please ping the ppl mentioned in the toolstate.toml and open a PR that fixes the problem
[2017-10-16 21:24:46] <mw> re: "the tool is built and tested irrelevant of the toolstate" -- what does that mean?
[2017-10-16 21:24:49] <oli_obk> the tool owners take care of reenabling the tool
[2017-10-16 21:25:03] <oli_obk> mw: it means that CI wants to know whether the setting is correct
[2017-10-16 21:25:15] <oli_obk> so if you choose "Broken" but it compiles, that is wrong
[2017-10-16 21:25:25] <mw> ok, so you need an "exact match"?
[2017-10-16 21:25:33] <oli_obk> mw: pretty much
[2017-10-16 21:25:42] <mw> ok, that makes sense, thanks!
[2017-10-16 21:26:04] <mw> how long has this been active?
[2017-10-16 21:26:07] <oli_obk> Now, our goal is that we never have to turn a tool to Broken or Compiling
[2017-10-16 21:26:14] <oli_obk> mw: just a few weeks
[2017-10-16 21:26:34] <oli_obk> but clippy and miri have mostly been "Broken" out of $reasons
[2017-10-16 21:26:51] <oli_obk> basically there were some integration issues with the stages
[2017-10-16 21:26:57] <oli_obk> but we have that working again
[2017-10-16 21:27:34] <oli_obk> ok, so that's the gist of how it is working atm, any questions before I continue with the issues and the plan?
[2017-10-16 21:27:52] <steveklabnik> none here
[2017-10-16 21:28:26] <mw> do we have an error message informing people about toolstate.toml?
[2017-10-16 21:28:32] <oli_obk> jep
[2017-10-16 21:28:45] <mw> ok, very good
[2017-10-16 21:28:51] <oli_obk> if the state mismatches reality, you get pointed to the file
[2017-10-16 21:29:04] <oli_obk> I don't think the pinging has been reliable though
[2017-10-16 21:29:16] <oli_obk> not sure how to improve that
[2017-10-16 21:29:38] <mw> nrc is also a bit of a bottleneck
[2017-10-16 21:29:42] <oli_obk> PRs have been successful though, so that kinda works
[2017-10-16 21:29:48] <mw> as he's the only contact for two of the four tools
[2017-10-16 21:30:07] <oli_obk> mw: well technically you should be open a PR fixing the issue if you broke the tools
[2017-10-16 21:30:21] <oli_obk> but yea, one person is a single point of failure
[2017-10-16 21:30:50] <mw> it sounds like this could be automated 
[2017-10-16 21:30:55] <mw> in theory at least
[2017-10-16 21:31:12] <mw> to have some kind of dashboard that says: this PR broke this tool
[2017-10-16 21:31:44] <oli_obk> servo has sth detecting unsafe code
[2017-10-16 21:31:57] <oli_obk> we could have a similar thing for touching the toolstate.toml
[2017-10-16 21:32:14] <mw> yes, maybe something to keep in mind
[2017-10-16 21:32:27] <mw> ok, those were my questions
[2017-10-16 21:32:50] <oli_obk> great! that is a good idea, one less thing that can go wrong
[2017-10-16 21:33:11] <oli_obk> next thing that is wrong: rls and rustfmt can't be changed to Broken XD
[2017-10-16 21:33:25] <mw> wat :)
[2017-10-16 21:33:27] → Zoxc_ joined (Zoxc@moz-t8u5l9.93-89-45.enivest.net)
[2017-10-16 21:33:43] <mw> so they always have to work?
[2017-10-16 21:33:43] <oli_obk> well. they are distributed with rustup
[2017-10-16 21:33:50] <oli_obk> mw: yea, and that is a huge pain
[2017-10-16 21:34:04] <oli_obk> because it puts even more strain on nrc for moving PRs to branches
[2017-10-16 21:34:07] <oli_obk> I'll explain
[2017-10-16 21:34:27] <oli_obk> so. You break a tool and you want to do a lockstep upgrade without setting the state to "Broken"
[2017-10-16 21:34:53] <oli_obk> That is possible. Technically it's enough to open a PR and then point rustc's git submodule for that tool to the PR
[2017-10-16 21:34:59] <oli_obk> but PRs are rather volatile
[2017-10-16 21:35:07] <oli_obk> one force push and CI breaks.
[2017-10-16 21:35:16] <oli_obk> Rust CI. Breaking because of a force b
[2017-10-16 21:35:21] <oli_obk> push to a private repository
[2017-10-16 21:35:43] <oli_obk> because PRs are in private repositories, even if duplicated in the tool repository
[2017-10-16 21:35:56] <oli_obk> if you force push a PR branch, the commit disappears from the tool repository
[2017-10-16 21:36:00] <mw> that also sounds like one can slip in unapproved changes (or at least not approved by the tool owners)
[2017-10-16 21:36:09] <oli_obk> mw: that too
[2017-10-16 21:36:24] <oli_obk> but I consider that less of an issue, because it should be reviewed by Rust reviewers
[2017-10-16 21:36:50] <oli_obk> the big issue is breaking CI and breaking historical commits
[2017-10-16 21:37:08] <oli_obk> the solution is to change the PR into a branch on the tool repo
[2017-10-16 21:37:18] <oli_obk> unfortunately that needs to be done manually by the tool owner
[2017-10-16 21:37:26] <oli_obk> there's no github button or bot that helps us there
[2017-10-16 21:37:38] <oli_obk> this is annoying. not hard, but still somewhat annoying
[2017-10-16 21:37:53] <oli_obk> This is actually the reason why toolstate.toml exists
[2017-10-16 21:37:54] ⇐ Zoxc_ quit (Zoxc@moz-t8u5l9.93-89-45.enivest.net): Ping timeout: 121 seconds
[2017-10-16 21:38:08] <oli_obk> because now we can skip those steps
[2017-10-16 21:38:37] <oli_obk> we open a PR to the tool and once the nightly hits, tool CI passes, the PR is merged, and anyone can open a new PR to rustc pointing to tool master
[2017-10-16 21:38:39] <mw> but I guess at some point miri will be even more essential than rls/rustfmt
[2017-10-16 21:39:01] <oli_obk> mw: no, miri is only in there because we don't want future const eval features to stagnate
[2017-10-16 21:39:09] <oli_obk> miri core is already being integrated into rustc itself
[2017-10-16 21:39:13] <oli_obk> not as a submodule
[2017-10-16 21:39:24] <mw> ok, that sounds very reasonable
[2017-10-16 21:39:45] <oli_obk> the rest of miri stays in the submodule, especially the crazy parts like C-function emulation
[2017-10-16 21:40:17] <oli_obk> so anyway. Lockstep upgrades are annoying, but are mitigated by toolstate.toml
[2017-10-16 21:40:25] <mw> "and once the nightly hits, tool CI passes, the PR is merged" -> does this happen automatically?
[2017-10-16 21:40:31] <oli_obk> mw: no
[2017-10-16 21:40:41] <oli_obk> the PR goes through a normal review cycle in the tool
[2017-10-16 21:40:46] <oli_obk> it mayb even be rebased and force pushed
[2017-10-16 21:40:50] <mw> ok
[2017-10-16 21:40:52] <oli_obk> that's perfectly fine
[2017-10-16 21:41:20] <oli_obk> I don't think we should automate the toolstate process
[2017-10-16 21:41:28] <mw> ok, so it sounds like toolstate.toml has a big improvement
[2017-10-16 21:41:45] <oli_obk> instead we should focus on improving the lockstep process, because there's always the escape hatch to toolstate.toml
[2017-10-16 21:42:05] <oli_obk> mw: it's the sole reason miri and clippy are allowed in Rust's CI
[2017-10-16 21:42:19] <oli_obk> so that is an enormous win for us (clippy & miri devs)
[2017-10-16 21:42:35] <mw> definitely
[2017-10-16 21:43:03] <oli_obk> The most important next thing is to make rustup have "failing" components
[2017-10-16 21:43:20] <oli_obk> that's where I'm gonna need help. Both in rustup and in rustc distribution generation
[2017-10-16 21:43:45] <mw> can you explain what that means exactly, failing components
[2017-10-16 21:43:53] <oli_obk> right
[2017-10-16 21:44:27] <oli_obk> so you "rustup component add rustfmt", which means that every `rustup update nightly` will also update your local `rustfmt`
[2017-10-16 21:44:53] <oli_obk> if we change `rustfmt` to `Broken` in `toolstate.toml`, what will happen?
[2017-10-16 21:45:23] <oli_obk> next time you do `rustup update nightly`, should there be no `rustfmt`? Should the `rustup update nightly` tell you you can't update without removing `rustfmt`?
[2017-10-16 21:45:52] <oli_obk> Asking around, I got positive feedback for the latter
[2017-10-16 21:46:16] <mw> so you'd opt into removing the component?
[2017-10-16 21:46:18] <oli_obk> but once we have multiple tools, and you have all of their components installed, how long will you have to wait for a new nightly where all your tools work?
[2017-10-16 21:46:42] <oli_obk> mw: no, you'd not update until a new nightly comes around which has the tool
[2017-10-16 21:46:49] <oli_obk> but that has the mentioned problem
[2017-10-16 21:47:17] <mw> yes, this sounds like a non-trivial problem
[2017-10-16 21:47:18] <oli_obk> There's a lot of open questions and probably no clear solution that everyone will prefer
[2017-10-16 21:47:19] * twk1 is now known as twk
[2017-10-16 21:47:39] <mw> is there a central place where this is discussed?
[2017-10-16 21:47:50] <oli_obk> Not to my knowledge
[2017-10-16 21:48:06] <oli_obk> I wanted to bring it up in a dev tools meeting so we can discuss how and where to continue
[2017-10-16 21:48:51] <mw> to me this sounds like something that needs an offline discussion
[2017-10-16 21:48:59] <oli_obk> I mean we can have a central issue in the dev-tools-team repo
[2017-10-16 21:49:09] <mw> yes, something like that
[2017-10-16 21:49:12] <oli_obk> but we need focussed issues for rustup and rust dist I guess
[2017-10-16 21:50:01] <mw> yes, although the general strategy should be clarified before going into detail what this means for specific tools
[2017-10-16 21:50:21] <oli_obk> I'll create the dev-tools-team issue, so we have a starting point
[2017-10-16 21:50:36] <mw> great
[2017-10-16 21:50:39] <oli_obk> specific tools being rustup?
[2017-10-16 21:50:50] <mw> yes, essentially
[2017-10-16 21:50:54] <oli_obk> ok ^^
[2017-10-16 21:51:04] <mw> I don't know if it would also touch cargo
[2017-10-16 21:51:28] <mw> and the tools that are the components might also have specific requirements
[2017-10-16 21:51:53] <mw> e.g. rustfmt might keep working while clippy absolutely needs a specific compiler version
[2017-10-16 21:52:01] <oli_obk> ah
[2017-10-16 21:52:13] <mw> that should be part of the discussion, is what I mean
[2017-10-16 21:52:27] <oli_obk> yea, I'll add that, good point
[2017-10-16 21:52:42] <mw> excellent
[2017-10-16 21:53:01] <mw> please put a link on the etherpad when you're done
[2017-10-16 21:53:05] <oli_obk> ok
[2017-10-16 21:53:27] <oli_obk> So I think that's pretty much it about toolstate.toml
[2017-10-16 21:53:38] <oli_obk> I don't know if it's planned to add any other tools than those four
[2017-10-16 21:53:40] <mw> tausend dank, oli_obk :)
[2017-10-16 21:53:55] <oli_obk> but it's really easy to add more, so don't hesitate to bring it up
[2017-10-16 21:54:08] <oli_obk> :)
[2017-10-16 21:54:13] <mw> this is definitely something we'll talk about quite a bit more in the future
[2017-10-16 21:54:33] ⇐ evolarium quit (jordan@moz-hbvtr4.gk4g.8lvb.0880.2604.IP): Quit: WeeChat 1.6
[2017-10-16 21:54:47] → evolarium joined (jordan@moz-hbvtr4.gk4g.8lvb.0880.2604.IP)
[2017-10-16 21:54:53] <mw> alright, is there any tools news anybody would like to share before we call it a day?
[2017-10-16 21:55:48] <matklad> There's a "blog post" about intellij rust state: https://users.rust-lang.org/t/intellij-rust-0-2-0-released/13419 :) 
[2017-10-16 21:56:14] <mw> matklad: oh, nice!
[2017-10-16 21:56:29] <matklad> Nothing major actually happened this week, just a summary of changes since like a year ago :) 
[2017-10-16 21:57:30] <mw> it's awesome that this exits!
[2017-10-16 21:57:41] <mw> it does Rust a lot of good, I think
[2017-10-16 21:57:55] <steveklabnik> a very minor note
[2017-10-16 21:58:03] <steveklabnik> new rustdoc, even though it's still just a baby
[2017-10-16 21:58:08] <steveklabnik> has grown an alternative frontend
[2017-10-16 21:58:16] <steveklabnik> we're working out the "swap frontends" api now
[2017-10-16 21:58:31] <mw> cool
[2017-10-16 21:58:54] <mw> is there ETA for the new rustdoc, btw?
[2017-10-16 21:59:49] <mw> ok, time's up
[2017-10-16 21:59:56] <mw> thanks everyone for coming!
[2017-10-16 22:00:02] <steveklabnik> no eta
[2017-10-16 22:00:12] <steveklabnik> it's a long-term, research-y project
[2017-10-16 22:00:27] <mw> next time nrc will be back, I think
[2017-10-16 22:01:03] <imperio> mw: nope, 26
[2017-10-16 22:01:49] <mw> I think the next meeting will be on the 30th
[2017-10-16 22:02:08] <mw> unless you all really want to do one next week, that is
[2017-10-16 22:02:50] <misdreavus> sgtm
[2017-10-16 22:03:03] → Zoxc_ joined (Zoxc@moz-t8u5l9.93-89-45.enivest.net)
[2017-10-16 22:04:14] <mw> ok, thanks everyone!
[2017-10-16 22:04:22] <misdreavus> <3 mw
[2017-10-16 22:04:37] <japaric> thanks mw
```
