# August 21st 2017

Triage week, irc meeting in #rust-dev-tools

## Notes

brson is leaving the dev-tools team :-( acrichto and nrc will look after rustup
in the short-term and there will be more soon about long-term plans.

### Triage

#### Disabling of -Zunstable-options --pretty=expanded breaks cbindgen

* https://github.com/rust-lang/rust/issues/43697
* There are other options - use a rustc shim, use nightly, look at the AST
* No feeling that we need a stable way to emit macro-expanded source code
* See also https://github.com/rust-lang/rust/issues/43364


#### RFC: Add external doc attribute to rustc

* https://github.com/rust-lang/rfcs/pull/1990
* An open question about what paths should be relative to
* nrc to comment


#### RFC: Attributes for tools, 2.0

* https://github.com/rust-lang/rfcs/pull/2103
* Some debate about whether we should extend the proposal to lints, e.g.,
  allow `#[allow(clippy::some_lint)]`
* nrc to comment


### Misc.

* Should `doc(masked)` be behind a feature flag? Yes.
* japaric to consult Cargo team about Xargo in Cargo RFC.


## IRC log

```
8:01 AM <•nrc> agenda: ps://public.etherpad-mozilla.org/p/rust-dev-tools
8:02 AM <mw> o/
8:02 AM <•nrc> hmm
8:02 AM <•nrc> https://public.etherpad-mozilla.org/p/rust-dev-tools
8:02 AM <japaric> o/
8:03 AM <•nrc> Hi all
8:04 AM <jntrnr> are there really 259 issues to triage?  hmmm
8:04 AM <•nrc> no
8:04 AM <imperio> we like to see big
8:04 AM <•nrc> that is all open issue
8:04 AM <misdreavus> usually we pass up the full list and only go to nominated issues
8:04 AM <jntrnr> can we change the link to point at nominated?
8:04 AM <killercup> let's kill 3 up front so it's a nice even 256
8:04 AM <•nrc> so, before we get into issue triage
8:05 AM <•nrc> jntrnr: link below it
8:05 AM <jntrnr> lol
8:05 AM — jntrnr isn't totally awake yet
8:05 AM <•nrc> as you've probably seen on internals, brson is leaving the Rust project and moving on to other things :-(
8:05 AM <•nrc> which means he's leaving the dev-tools team
8:06 AM <misdreavus> <3
8:06 AM <•nrc> which is a bit sad, but nothing to worry about
8:06 AM <killercup> :( but also <3
8:06 AM <•nrc> for us, the big question is maintaining Rustup
8:07 AM <•nrc> in the short term acrichto and I will look after it
8:07 AM <•nrc> (acrichto we should talk about this offline this week some time :-) )
8:07 AM <•nrc> longer term, we'll need to figure something out...
8:07 AM <•nrc> if anyone has an interest in getting more involved with Rustup, please let me know!
8:07 AM <jntrnr> is there a lot of planned features, or is largely maintenance now?
8:08 AM <imperio> nrc: I *could* start taking a look
8:08 AM <imperio> if it seems doable, I'll certainly get involved
8:08 AM <•nrc> jntrnr: mostly maintenance. There are a few features 'kind of' planned, and we have to decide what the roadmap looks like
8:09 AM <•nrc> imperio: thanks! I'll keep you involved with discussion
8:09 AM <imperio> ok
8:09 AM <mw> would that problem be kind of alleviated by merging rustup into cargo?
8:09 AM <imperio> mw: how do you plan doing it?
8:09 AM <•nrc> mw: to some extent, yeah
8:10 AM <misdreavus> that feels like it would be a tall order
8:10 AM <•nrc> that is, in the long term
8:10 AM <mw> imperio: there are no concrete plans, but it has been talked about
8:10 AM <imperio> oh ok
8:10 AM <•nrc> but we'd need people to do actually do the work and I don't think the Cargo team have the bandwidth to do it alone
8:10 AM <mw> but  yes, it would be a long term thing
8:10 AM <•nrc> I have it in mind as something for the team to look at next year
8:11 AM <•nrc> so, we'll discuss it a bit when we come to making our 2018 roadmap
8:11 AM <imperio> nrc: btw, where are we on our roadmap? Are we on schedule?
8:11 AM <•nrc> (reasoning is that literally no-one has time/bandwidth to take it on this year)
8:11 AM <killercup> speaking of bandwidth, nrc, are there plans to fill brson's dev tool team seat? i.e. add a new member
8:12 AM <•nrc> imperio: yeah, I think so, no big changes since the roadmap meeting, we'll check in again around the end of the quarter
8:12 AM <•nrc> killercup: perhaps - we need to think about that
8:12 AM <jntrnr> btw +1 on merging them
8:12 AM <killercup> cool. so far I thought the team size was pretty arbitrary but this could be an opportunity to think about it
8:13 AM <jntrnr> I think that was on the long term plan
8:13 AM <jntrnr> oh right, I'm catching up reading messages as they come in :p
8:14 AM <imperio> killercup: do you have someone in mind?
8:14 AM <•nrc> killercup: IMO, there is not a desperate need - with all the peers existing and a few of them being involved in general matters as well as their own tools (thanks, btw, you are all awesome!) I don't feel like we are constrained
8:14 AM <imperio> jntrnr: you're far from us, lags are still a thing I guess :p
8:14 AM <jntrnr> lolz, it's mostly my brain lagging :p
8:15 AM <•nrc> but it is something we should definitely think about
8:15 AM <•nrc> I do think it is more urgent to plan out Rustup, so I'll focus on that first
8:15 AM <imperio> nrc: killercup: I'm not sure for the moment we need to. We'd need numbers to see which area lacks people (let's say which needs the more haha)
8:16 AM <killercup> imperio: nrc: i was just wondering, i don't see any immediate need. more tool peers/focus groups would be my next step
8:16 AM <misdreavus> was rustup officially under our umbrella?
8:16 AM <imperio> nrc: add me in this loop (I think you said it but forgot)
8:16 AM <imperio> misdreavus: yes
8:16 AM <•nrc> misdreavus: yes
8:16 AM <imperio> under brson's
8:16 AM <misdreavus> huh
8:16 AM <misdreavus> i didn't think i saw a specific peer for it so i never considered it
8:17 AM <misdreavus> but i guess brson was that peer?
8:17 AM <•nrc> sort of yeah
8:18 AM <•nrc> if anyone wants to talk to me about any of this stuff, please feel free - ping me any time
8:18 AM <•nrc> ok, triage
8:18 AM <•nrc> there is only one nominated issue https://github.com/rust-lang/rust/issues/43697
8:18 AM <•nrc> 'we' broke cbindgen by unstabilising pretty printing
8:18 AM <•nrc> fitzgen, emilio: do you know much about cbindgen?
8:19 AM <emilio> nrc: I know a bit about how it's used, yeah
8:19 AM <fitzgen> nrc: I have heard of it?
8:21 AM <•nrc> it sounds like they parse macro expanded code, which sounds terrible to me
8:22 AM <mw> so using a custom compiler driver needs nightly? that sounds a bit problematic
8:22 AM <mw> since build systems will often build tools like cbindgen, right?
8:22 AM <•nrc> there is also gh43364 which is basically requesting the same thing
8:22 AM <misdreavus> !gh 43364
8:22 AM <rustbot> [Issue 43364] <open> Stabilize --pretty=expanded <https://github.com/rust-lang/rust/issues/43364>
8:22 AM <•nrc> mw: they can use a rustc shim to get around the nightly requirement (like we will for the rls)
8:24 AM <•nrc> I pinged op
8:24 AM <•nrc> I feel very bad about stabilising the output of pretty expanded
8:24 AM <•nrc> does anyone like the idea?
8:25 AM <mw> not me
8:25 AM <imperio> I never used it so no opinion
8:26 AM <misdreavus> i got halfway to using it when i tried using syn for rustdoc but never got that far
8:27 AM <•nrc> ok, lets see if we get any more feedback there
8:27 AM <•nrc> there's only one PR and it is waiting on the author
8:27 AM <•nrc> RFCs: https://github.com/rust-lang/rfcs/pulls?q=is%3Aopen+is%3Apr+label%3AT-dev-tools
8:28 AM <•nrc> #1990 is currently in FCP
8:28 AM <•nrc> there is a question of where to base the paths
8:29 AM <misdreavus> yeah
8:29 AM <•nrc> what does crate-relative mean if you're not using Cargo?
8:29 AM <misdreavus> cwd, i guess?
8:30 AM <•nrc> that makes me a bit uneasy
8:30 AM <•nrc> I think I should write up some thoughts on the thread
8:31 AM <misdreavus> is this worth registering a conern with rfcbot
8:31 AM <misdreavus> fcp ends tomorrow
8:31 AM <misdreavus> oh wait, it's 10 days, isn't it
8:31 AM <•nrc> tl'dr: I prefer an absolute path, but the details are a bit sketchy
8:31 AM <•nrc> I'm not actually sure
8:31 AM <•nrc> yeah, can you register a concern please?
8:32 AM <misdreavus> can do
8:32 AM <•nrc> thanks!
8:32 AM <•nrc> https://github.com/rust-lang/rfcs/pull/2103
8:32 AM <•nrc> attributes for tools
8:32 AM <misdreavus> should i include this discussion in my comment?
8:33 AM <misdreavus> or would you like to do that
8:33 AM <•nrc> misdreavus: I can comment on thread
8:33 AM <misdreavus> okie doke, thanks
8:33 AM <•nrc> there is a bit of an open question for Clippy folk:
8:33 AM <•nrc> clippy::allow(some_lint) vs allow(clippy::some_lint)
8:34 AM <Havvy> As an end user, I'd rather the #[allow(clippy::some_lint)]
8:34 AM <•nrc> Manishearth: do you know how hard that would be to implement?
8:35 AM <•nrc> and in general, does anyone have any thoughts on this RFC? I'd like to FCP soon
8:36 AM <misdreavus> i haven't read it closely, but i like the idea
8:36 AM <Wraithan> Do folks use clippy in CI? If so do you use something to simulate -Werror behavior?
8:37 AM <vorner> Wraithan: I do. Let me look it up.
8:37 AM <imperio> nrc: reading the RFC, but for now I don't udnerstand the needs
8:37 AM <vorner> Wraithan: cargo clippy -- --deny clippy
8:38 AM <vorner> However, that errors only on the clippy ones, not on ordinary warnings.
8:38 AM <killercup> Wraithan: yes. diesel uses a cargo feature, cargo-edit uses `cargo clippy -- -D warnings`
8:38 AM <•nrc> imperio: the need is to allow tools to have attributes without the compiler erroring
8:38 AM <•nrc> e.g., #[rustfmt::skip]
8:38 AM <imperio> ooooooh
8:39 AM <imperio> you mean like the allow(unknown_lints) we have to put for clippy?
8:39 AM <•nrc> so, at the moment it doesn't really cover lints, but it probably should
8:40 AM <Wraithan> vorner, killercup: Those both help, thanks, seems -D warnings is what I'm looking for since that'll catch compiler lints too if understand correctly
8:40 AM <imperio> hum no
8:40 AM <•nrc> atm, it would allow clippy::allow(some_lint)
8:40 AM <imperio> so it's just prevent a tool from being run over an item?
8:40 AM <imperio> strange
8:40 AM <imperio> +to
8:40 AM <•nrc> well a tool can attach any meaning it wants to an attribute
8:41 AM <imperio> so it's just to not make rustc fails when parsing it?
8:41 AM <•nrc> it would allow, for example, #[rustdoc::document_as_pub] or something
8:41 AM <•nrc> yes
8:41 AM <imperio> now I understand :)
8:41 AM <Wraithan> killercup, vorner: Delightful got my first CI failure reported based on lints, thank you so much :)
8:42 AM <imperio> I vote for the #[tool::stuff] syntax
8:42 AM <•nrc> so, I'll talk to Clippy folk about lints and then FCP, I think
8:42 AM <•nrc> imperio: that is what is proposed
8:42 AM <killercup> Wraithan: cool! ping me when you want more ci failures! 😅
8:42 AM <•nrc> currently
8:42 AM <•nrc> there is also a request to extend it to lints
8:43 AM <•nrc> tools checkin - a reminder to put any tools news you have in https://github.com/nrc/dev-tools-team/issues/25
8:43 AM <imperio> nrc: I thought there was a debate over the syntax
8:43 AM <imperio> perfect if there isn't
8:43 AM <•nrc> imperio: there is only debate about the extension to lints, aiui, there is no debate about the basic syntax
8:43 AM <imperio> aaaah
8:44 AM <imperio> well, I actually don't understand
8:44 AM <imperio> since the tool does what it wants with the "parameter", what do we need to add?
8:44 AM <•nrc> japaric: re the Xargo in Cargo RFC, iiuc you were waiting on feedback from brson. Are you OK to post that RFC as is? Or would you like me or someone from the Cargo team to take a look?
8:45 AM <imperio> #[allow(clippy::whatever)], then clippy sees whatever and allows it
8:45 AM <imperio> did I miss something?
8:45 AM <•nrc> imperio: some way for the compiler to not throw an error about a bad lint
8:46 AM <japaric> nrc: I'd like to get some feedback from the Cargo team first since it's Cargo related :-)
8:46 AM <misdreavus> i'm in the middle of a pr to add a new parameter to the doc attribute, should i add a feature flag for it like was done for doc(cfg)?
8:46 AM <misdreavus> !gh 43348
8:46 AM <rustbot> [PR 43348] <merged> Expose all OS-specific modules in libstd doc. <https://github.com/rust-lang/rust/pull/43348>
8:46 AM <•nrc> japaric: ok, great, carol10cents might be a good person to ask
8:47 AM <imperio> misdreavus: oh damn, that's awesome!
8:47 AM <misdreavus> imperio: (that was the doc(cfg) pr, not mine >_>)
8:47 AM <japaric> nrc: ok, I'll ask Carol
8:48 AM <•nrc> misdreavus: what is your parameter?
8:48 AM <misdreavus> doc(masked), to hide types and traits from the tagged extern crate
8:48 AM <misdreavus> to hide libc/rand/compiler_builtins from leaking into the std docs
8:49 AM <•nrc> it would be insta-stable without a feature gate, right?
8:49 AM <misdreavus> aiui, yeah
8:49 AM <•nrc> do std docs get to use unstable features?
8:49 AM <•nrc> like the libs do
8:49 AM <misdreavus> yeah, since they're just written alongside std
8:50 AM <misdreavus> e.g. doc(cfg) was deliberately for std
8:50 AM <misdreavus> like doc(masked) will be
8:50 AM <imperio> nrc: yep, but not much
8:51 AM <•nrc> ok, then it sounds like a feature gate is a good idea so we can try it out before stabilising
8:51 AM <misdreavus> cool, i'll work on that before opening the pr
8:51 AM <misdreavus> thanks!
8:52 AM <imperio> misdreavus: if you need unstable checking, look at how I did for my unstable options
8:52 AM <•nrc> thank you!
8:52 AM <imperio> it's just a function check :)
8:52 AM <misdreavus> i'm cribbing everything from the doc(cfg) pr >_>
8:52 AM <•nrc> anyone else have tool-specific stuff they want to talk about?
8:52 AM <imperio> that's nice too
8:53 AM <imperio> nrc: nope, waiting for more news about rustup
8:53 AM <misdreavus> that's all i had, i still need to work through the rustdoc issue triage, probably slower than ideal
8:54 AM <•nrc> good on you, doing that :-)
8:54 AM <misdreavus> i'd like to get most-if-not-all the way through it by the impl period so i can solicit rustdoc contributors :P
8:55 AM <misdreavus> i... don't think i'll make it >_>
8:55 AM <imperio> misdreavus: I'm not sure it's fully worth it right now
8:55 AM <imperio> however I have a PR on rustdoc that needs review
8:55 AM — misdreavus checks their github email folder
8:55 AM <imperio> misdreavus: if you could take a look: https://github.com/rust-lang/rust/pull/43966 :)
8:56 AM <misdreavus> imperio: tab opened :)
8:56 AM <imperio> great, thanks
8:56 AM <imperio> it's not long
8:56 AM <imperio> but I didn't come up with a better solution for the moment
8:56 AM <imperio> any new idea would be appreciated
8:56 AM <imperio> (the code is just too ugly to be kept as is)
8:56 AM <•nrc> ok, last thing - next week's meeting
8:56 AM <•nrc> is scheduled for an agenda meeting
8:56 AM <•nrc> does anyone have anything they'd like to discuss?
8:57 AM <imperio> what is an "agenda meeting" ?
8:57 AM <•nrc> I do not, so if there are no suggestions, I'd like to cancel
8:57 AM <misdreavus> imperio: those vidyo meetings where we do things that aren't triage
8:57 AM <•nrc> where we discuss a single topic (like the rustdoc rewrite last week)
8:57 AM <imperio> ah ok
8:57 AM — imperio writes somehwere "agenda meeting == vidyo meeting"
8:58 AM <•nrc> I expect we'll want a rustup meeting soon, but not next week
8:58 AM <imperio> otherwise nothing from me
8:58 AM <•nrc> if anyone wants to cover something, let me know in the next day or two, otherwise I'll cancel
8:58 AM <•nrc> ok, meeting over, thank you everyone!
```
