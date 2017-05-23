# May 22nd 2017

Triage week, irc meeting in #rust-dev-tools

## Notes

### Triage

* https://github.com/rust-lang/rust/issues/28544

Discussion about showing name of test before test starts to make it easier to find tests with segfaults. Made more complex by multi-threaded tests and `--no-capture`. There was some desire for printing test names before testing, but fear about the technical difficulty. It would also make #28544 harder. Given that `ok`/`fail` are now coloured, we decided that this issue is not so important and closed it.

* https://github.com/rust-lang/rust/pull/41700

Overall sentiment was to approve. Some worry about what "stable" means here. In particular if the HTML output changes and a user's CSS breaks, is that a stability violation? We decided that no, it isn't. That should be documented and then the PR should be merged.

* https://github.com/rust-lang/rfcs/pull/1946 and https://github.com/rust-lang/rfcs/pull/1990

General positivity, no detailed discussion.


### Tools check-in

* nrc mentioned that Rustfmt is moving from Syntex to libsyntax
* emilio promised more next time on Bindgen, nothing this week
* IntelliJ are implementing some of their own type inference and have Clippy support.
* Clippy is "chugging along", waiting on decision about Rustup and CI from core team.
  - no objections to moving Clippy to rust-lang-nursery (though some discussion of what that means re Clippy 1.0)

### Outstanding

We did not get to:

* Questions about RFC process (see yesterday's email)


## IRC log

```
7:13 AM <•nrc> yay, first dev-tools meeting!
7:13 AM <misdreavus> it's like a wave in a stadium, lol
7:14 AM <•nrc> matklad: missed some confusion over the meeting time, I think steveklabnik is 'sort of here' :-)
7:14 AM <steveklabnik> yeah my 1-1 with aaron is on vidyo
7:14 AM <steveklabnik> so i won't be paying mucha ttention
7:14 AM <steveklabnik> but am vaguely around/
7:15 AM <•nrc> mw can't make it today
7:15 AM <•nrc> nor manishearth
7:15 AM <•nrc> brson: ping
7:15 AM <•nrc> emilio: ping
7:15 AM <jntrnr> do we have an agenda?
7:15 AM <•nrc> oh and fitzgen is away
7:15 AM <brson> nrc pong
7:15 AM <oli_obk> jntrnr: on etherpad
7:16 AM <•nrc> agenda and notes etherpad: https://public.etherpad-mozilla.org/p/rust-dev-tools
7:16 AM <•nrc> so, welcome! Thanks for joining up everyone!
7:16 AM <Manishearth> i can make it
7:16 AM <Manishearth> fuck the irs
7:16 AM <•nrc> woo!
7:16 AM <imperio> ah right
7:16 AM → llogiq joined (llogiq@moz-581.j04.146.5.IP)
7:16 AM <imperio> we need to discuss about https://github.com/rust-lang/rust/pull/41700 too
7:16 AM <Manishearth> but it was rescheduled for tomorrow
7:17 AM <imperio> since it's not up to the doc team anymore
7:17 AM <llogiq> hi folks.
7:17 AM <•nrc> Manishearth exists in the eyes of the dev-tools team, if not the irs
7:17 AM → eijebong joined (eijebong@moz-6c82jn.adsl-surfen.hetnet.nl)
7:17 AM <emilio> nrc: pong, I'm around.
7:17 AM <llogiq> I'm only a bot that posts 'whatreyoudoin' threads to reddit.
7:18 AM <llogiq> do I exist?
7:18 AM — oli_obk can confirm the existance of llogiq
7:18 AM <•nrc> So, I think this time works - everyone who is missing it can make the regular time, I think, hopefully we can work soemthing out with Steve
7:18 AM <•nrc> llogiq: do you think?
7:19 AM <killercup> Manishearth: if the irs recognizes you as a dev tool artist, do you still need to pay taxes?
7:19 AM <llogiq> I think I do. ;-)
7:19 AM <misdreavus> llogiq: i mean, your actions have had an effect on my existence, at least in the form of lines appearing on my irc client :3
7:19 AM <•nrc> this week is triage, next week will be a Rustdoc-focused meeting on Vidyo (I'll send instructions)
7:19 AM <•nrc> then triage again, etc.
7:19 AM <•nrc> any questions about the meeting format?
7:19 AM <llogiq> that'll do. OK folks, what points do we have to triage?
7:20 AM <Manishearth> wfm
7:20 AM <oli_obk> is it desired for non-rustdocians to be at the rustdoc focused vidyo?
7:20 AM <•nrc> The only nominated issue is - https://github.com/rust-lang/rust/issues/28544
7:21 AM <misdreavus> it's very fortunate that next monday is a holiday in the us, otherwise i'd have to figure out what vidyo is and how to smuggle my way in while at dayjob
7:21 AM <jntrnr> lgtm
7:21 AM <Manishearth> same q as oli + if so, can you send invites to all, explicitly mentioning the core agenda?
7:21 AM <•nrc> oli_obk: the idea is that for the Vidyo meetings, only the Rustdoc folk and 'core' dev-tools people attend
7:21 AM <imperio> nrc: there is https://github.com/rust-lang/rust/pull/41700 too
7:21 AM <oli_obk> nrc: ok, sgtm
7:21 AM <•nrc> to try and keep focussed and so forth
7:21 AM <•nrc> imperio: patience :-) PRs are next
7:21 AM <Manishearth> nrc on that issue, i think progress is more valuable
7:22 AM <imperio> oh my bad
7:22 AM <•nrc> Manishearth: 28544
7:22 AM <•nrc> ?
7:22 AM — imperio goes sitting in the back patiently
7:22 AM <Manishearth> if you have a segfaulting test its important to know what the currently running test was
7:22 AM <Manishearth> yeah
7:22 AM <Manishearth> i would like to see both these issues solved at once
7:23 AM <•nrc> Manishearth: do you feel it's still an issue now that the ok/fail is in colour (re last comment)?
7:23 AM <misdreavus> i like nagisa's suggestion of printing the name then writing over it with the test status on the left
7:23 AM <Manishearth> nrc not the issue itself, just the progress thing i mentioned
7:23 AM <Manishearth> yes +1
7:23 AM <emilio> yeah, I agree with manish here, the test name should be printed before the test runs IMO (even if it's using a escape sequence hack so it's printed in the right position)
7:24 AM <killercup> do we do escape code magic in any other official cli currently?
7:24 AM <matklad> and will this magic work on windows?
7:24 AM <misdreavus> does rustup print progress bars?
7:25 AM <•nrc> brson? ^
7:25 AM <misdreavus> answer: not per se, but yes it does overwrite the terminal line
7:25 AM <killercup> misdreavus: it does but it looks additive (print + flush)
7:25 AM <brson> test can't be printed before it runs when running in parallel
7:25 AM <brson> when running in single-threaded test names are printed before running
7:25 AM <Manishearth> brson sure it can, you just need a lot more machinery
7:25 AM <brson> rustup prints dl progress, but not bars
7:25 AM — misdreavus just ran a "rustup update" on their server
7:25 AM <eijebong> misdreavus: I think it's a problem with CI logs, I implemented that some times ago with '\r' for a python project and gitlab-ci logs don't like that, they print both lines and it's a bit confusing
7:25 AM <Manishearth> brson basically youd have to flush on each progress event. tricky
7:26 AM <brson> i'm just saying how it is
7:26 AM <•nrc> yeah, that seems there is a lot of scope for bugs
7:26 AM <•nrc> although Cargo does this with compiling right?
7:26 AM <•nrc> As in prints before it compiles
7:26 AM <•nrc> However, that has confused lots of people, I think
7:27 AM <killercup> so the issue is printing the name before the stdout of the child? or is a segfault written differently Manishearth?
7:27 AM <brson> cargo doesn't print a status of each unit after they are finished
7:27 AM <•nrc> oh yeah, right, that is the scary bit
7:27 AM <•nrc> ok, it sounds like we're all basically in agreement, modulo some details
7:28 AM <•nrc> does anyone think this should be higher priority than p-low?
7:28 AM <•nrc> (I do not, personally)
7:28 AM <emilio> I don't think so
7:28 AM <•nrc> Wait, actually, if we don't print the test name before the test, then we could move to the proposed layout without making the progress issue any worse
7:29 AM <•nrc> And without having to do any fancy console stuff
7:29 AM <•nrc> Manishearth: thoughts?
7:30 AM <Manishearth> nrc sgtm
7:30 AM <matklad> this would make `--nocapture` confusing?
7:30 AM <•nrc> matklad: why?
7:31 AM <matklad> You see some output, but you don't know which test is this.
7:32 AM <•nrc> matklad: I think that is fixable if we change the ordering of printing test names, then do the test, then print the result?
7:33 AM <killercup> please also note that if we change this, we should also adapt the layout of doctests
7:34 AM <matklad> that is, different format for `--nocapture`, with result after? Yep, that'll fix it.
7:34 AM <•nrc> Let me put this another way - nobody likes change, if we change this people will complain that their tests look different. So we should do this if we think it will make things better, not just because it will not make things worse (and to be clear, this == just this issue not the ordering thing). My feeling is that with the coloured ok/fail, I don't think we
7:34 AM <•nrc> need this. Does anyone think that the proposed change will improve things?
7:35 AM <brson> i'm not paying enough attention to understand what the proposal is, but i'm not inclined to change things here lightly
7:35 AM <killercup> i don't think this is worth doing right now
7:36 AM <matklad> nrc: agree. What would be useful is custom tests reporters (to run all tests in parallel and collect their stdout/stderr for IDE integration), but that's a lot of work.
7:36 AM — killercup is hoping to see some test framework rfcs in the future, with tap/json output for example
7:36 AM <misdreavus> i would ask two questions for this: 1) what does the OP think now that the success/fail gets color coded? 2) do CI logs keep the color codes?
7:37 AM <•nrc> misdreavus: this is an old issue, I'm not sure the OP is still around - they haven't commented recently
7:37 AM <•nrc> 2) they do not
7:37 AM <killercup> misdreavus: the colored output works with travis
7:37 AM <•nrc> I'm not sure how important that is though
7:37 AM <•nrc> oh, it does
7:37 AM <•nrc> (not with Homu or whatever, unless that changed recently)
7:37 AM <•nrc> I propose closing this - the other issue  (progress) is 30047
7:37 AM <•nrc> any objections?
7:37 AM <oli_obk> no
7:38 AM <killercup> sgtm
7:38 AM <imperio> no
7:38 AM <misdreavus> no objections
7:38 AM <•nrc> ok, great
7:38 AM <•nrc> PRs: https://github.com/rust-lang/rust/pulls?q=is%3Aopen+is%3Apr+label%3AT-dev-tools
7:39 AM <•nrc> 42145 is just waiting on bors
7:39 AM <•nrc> 41700
7:39 AM <•nrc> stabilise --extend-css
7:39 AM <•nrc> imperio: how long has the option been around?
7:39 AM <imperio> ah finally! :D
7:39 AM <imperio> euh
7:39 AM <imperio> a year now I think?
7:39 AM <imperio> maybe more
7:39 AM <•nrc> steveklabnik: did you have anything for this?
7:39 AM <steveklabnik> extend-css?
7:40 AM <steveklabnik> i have no strong opinion
7:40 AM <imperio> https://blog.guillaume-gomez.fr/articles/2016-09-16+Generating+doc+with+rustdoc+and+a+custom+theme
7:40 AM <imperio> so I wrote this article about it almost a year ago
7:40 AM <imperio> but the option was already there for a moment
7:40 AM <misdreavus> the commit that added it is from a year ago https://github.com/GuillaumeGomez/rust/commit/ab835a12da26c2146b2dde1f3fe99df687730725
7:40 AM <•nrc> killercup: your comment about "e" what is the context for "usually"
7:40 AM <imperio> ah thanks misdreavus
7:41 AM <•nrc> so, it definitely has been around long enough
7:41 AM <killercup> nrc: "using cli tools" ;)
7:41 AM <•nrc> have there been any counter-proposals? Or complaints about it?
7:41 AM <imperio> nope
7:41 AM <misdreavus> it feels like one of those little-known features
7:42 AM <•nrc> killercup: like just CLI tools in general?
7:43 AM <killercup> my intuition when seeing a `-e` flag is not "extend"
7:43 AM <misdreavus> killercup: counterpoint: echo -e allows backslash escapes in the string
7:43 AM <•nrc> so long term, it seems like it would be nice to have other ways to modify the Rustdoc theme, if we add such things, does this option sit well with that?
7:43 AM <killercup> also, not "extend css", given that there may be other things to extend, e.g. html in the footer
7:43 AM <killercup> misdreavus: oh good point, with ssh as well
7:44 AM <acrichto> one question to consider for stabilizing something like this: do we provide any stability guarantees around the html structure?
7:44 AM <misdreavus> sed uses -e for the script
7:44 AM <•nrc> to me, that convention does not seem strong - I have no expectation around -e
7:44 AM <acrichto> in that, what guarantees do we provide about one css file from one release to another?
7:44 AM <imperio> nrc: don't mix this with themes
7:44 AM <imperio> I'll add themes later on but that's something besides
7:44 AM <•nrc> imperio: could you expand please?
7:45 AM <misdreavus> i'm not sure we have any stability guarantees around the output of rustdoc itself
7:45 AM <imperio> this can allow you to not just change colors but even the page shape
7:45 AM <steveklabnik> lol nope
7:45 AM <imperio> misdreavus: I added an issue for it
7:45 AM <misdreavus> the last discussion i remember having said as much
7:45 AM <imperio> my html-diff crate is functional now
7:46 AM <misdreavus> iirc the reasoning was that depending on *what* we stabilized, we wouldn't be able to improve the layout at all
7:46 AM <•nrc> ok, so I think the concern is that a user uses extend-css with a css file, then the html structure changes, and their css file no longer does what it used to do
7:46 AM <•nrc> which 'breaks' the css
7:46 AM <killercup> imperio: is this meant to "redefine" (overwrite) css or extend it with styles for custom markup used in docs?
7:46 AM <misdreavus> yeah
7:47 AM <imperio> killercup: both
7:47 AM <imperio> you can either extend or overwrite
7:47 AM <•nrc> imperio, misdreavus: do you think that is important? What are the expectations users should have when using this?
7:47 AM <imperio> mostly for personal themes
7:47 AM <imperio> we intend to do it for gtk-rs docs for examples
7:47 AM <imperio> we're waiting for this option to be stable
7:47 AM <imperio> so we can add a different background image
7:47 AM — killercup has a nitpick about the description then :)
7:48 AM ⇐ llogiq quit (llogiq@moz-581.j04.146.5.IP) Ping timeout: 121 seconds
7:48 AM <imperio> killercup: haha
7:48 AM <misdreavus> imperio: but would we give the impression that a css given to this option would work for any rust 1.x release?
7:48 AM <•nrc> so, perhaps gtk-rs is a good example - how annoying would it be for the css extension to stop working when Rust updates?
7:48 AM <imperio> misdreavus: it's not the point :-/
7:48 AM <imperio> it's just for users' use
7:48 AM <misdreavus> imperio: that's the question being asked here
7:49 AM <imperio> that's their problem after this
7:49 AM <misdreavus> i see it as a means for sites like docs.rs
7:49 AM <imperio> the html output of rustdoc changes very rarely
7:49 AM <killercup> i guess the current workaround for gtk-rs is using `--html-in-header` which is already stable?
7:49 AM <imperio> so it's quite safe to make css changes
7:49 AM <misdreavus> do we want to codify that?
7:50 AM <•nrc> so, it seems like the stability here is that: the argument name and effect are stable, the effect of any particular css file is unstable, but in practice will not break much
7:50 AM <killercup> and, if we do want to codify that, maybe only after the next meeting about rustdoc?
7:50 AM <imperio> killercup: ?
7:50 AM <misdreavus> nrc: yes
7:50 AM <•nrc> how could that be "codify"ed? Just documented?
7:50 AM <imperio> nrc: that's up to users
7:51 AM <•nrc> imperio: what is "that"
7:51 AM <misdreavus> nrc: that's what i mean, give it a stability guarantee like the standard lib and the rust language itself - that's what i assumed the discussion is about
7:51 AM <•nrc> ?
7:51 AM <imperio> nrc: the fact that their css is supposed to work
7:52 AM <misdreavus> i'm getting the feeling that two separate conversations are being had here
7:52 AM <killercup> imperio: hm? next monday we talk about rustdoc things anyway, so it'd be a good chance to specify how to document things like that
7:52 AM <imperio> yep
7:52 AM <Manishearth> we can't stablize whether or not css is expected to always work
7:52 AM <imperio> killercup: ah ok, well, why not
7:52 AM <misdreavus> Manishearth++
7:52 AM <imperio> Manishearth: exactly
7:52 AM <Manishearth> that's as daunting a task as stabilizing a language
7:52 AM <Manishearth> not worth it for a very niche feature
7:52 AM <•nrc> yeah, my feeling is that we don't acutally have any option here - we're not going to freeze the HTML
7:53 AM <•nrc> so we can't promise CSS will not break
7:53 AM <imperio> we don't need to?
7:53 AM <misdreavus> we can at least say that the mechanism of --extend-css is the same tho
7:53 AM <misdreavus> "this adds this css file to the css of each page"
7:53 AM <misdreavus> boom
7:53 AM <•nrc> so either we stabilise the option and document that your CSS may break as rustdoc evolves
7:53 AM <•nrc> or do not
7:53 AM <•nrc> so, it seems ok to me to stabilise the option (with docs)
7:54 AM <•nrc> acrichto: do you see any other options?
7:54 AM <imperio> it's strange that we'd have to say such thing but yes
7:54 AM <misdreavus> i am also okay with that option
7:54 AM <acrichto> I do not see other options, no
7:54 AM <•nrc> does anyone think we should never stabilise this because user CSS may break?
7:54 AM <acrichto> just needs to be consciously decided that this isnt guaranteed to be a stable feature
7:54 AM <acrichto> and this may generate pushback if we every try to redesign rustdoc
7:54 AM <acrichto> er, this will*
7:54 AM <misdreavus> imperio: if someone uses a css file across releases, what is the reaction when the additional css causes the site to look far worse than in a previous version of rustdoc?
7:55 AM <imperio> acrichto: this is a discussion about stabilizing it (or did I miss your point?)
7:55 AM <killercup> i would try to formulate this carefully as "adding css styles", not "redefine" or anything
7:55 AM <imperio> misdreavus: I don't see how it could
7:55 AM <imperio> only if they change the shape of the rustdoc output
7:55 AM <misdreavus> imperio: i'm talking hypotheticals here
7:55 AM <imperio> but then, they'll just provide their own css and not extend it
7:56 AM <misdreavus> if we reorganize the rustdoc output at some point
7:56 AM <•nrc> imperio: I think the worry is that people get attached to their CSS, then we make a big change and break it, then they complain
7:56 AM <misdreavus> or if their css overhauls the layout somehow
7:56 AM <imperio> nrc: I see the concern
7:56 AM <imperio> but like I said, you're not supposed to rewrite everything, just add what you need/prefer
7:57 AM <imperio> however it's a good point
7:57 AM <misdreavus> it's the "Deref for inheritance" argument
7:57 AM <•nrc> is there somewhere appropriate to document this in more detail?
7:57 AM <misdreavus> "this is what this is intended for"
7:57 AM <•nrc> that is are there docs for command line flags, other than --help ?
7:57 AM <misdreavus> nrc: it may be worth adding a line to the argument description, at least in the immediate term
7:58 AM <imperio> nrc: do we have a man page for rustdoc?
7:58 AM <steveklabnik> 1-1 done
7:58 AM — steveklabnik is now paying attention
7:58 AM <misdreavus> nrc: i don't think there's any help page beyond the --help text
7:58 AM <•nrc> imperio: or any other kind of docs?
7:58 AM <•nrc> ok
7:58 AM <misdreavus> there are a couple man pages but i don't remember what has them
7:58 AM — misdreavus looks
7:58 AM <•nrc> then I propose we r+ with the condition of adding a line to the --help text about stability
7:58 AM <steveklabnik> i have wanted to spin up some rustdoc docs
7:58 AM <steveklabnik> but have not
7:58 AM <misdreavus> https://github.com/rust-lang/rust/blob/master/man/rustdoc.1
7:58 AM <•nrc> is that OK with everyone?
7:59 AM <misdreavus> nrc: +1
7:59 AM <imperio> misdreavus: august 2016
7:59 AM <imperio> wow, quite recent
7:59 AM <misdreavus> oh, this still talks about json output >_>
7:59 AM <killercup> i'm okay with stabilizing with a better description
7:59 AM <killercup> i'd also like to point out that people can already add custom css with `--html-in-header="<style>body { color: red }</style>"` ;)
8:00 AM — misdreavus makes a note for docs team
8:00 AM <•nrc> I will write up a note after the meeting
8:00 AM <•nrc> or not :-)
8:00 AM <imperio> killercup: well, not very beautiful :p
8:00 AM <•nrc> ok, RFCs: https://github.com/rust-lang/rust/pulls?q=is%3Aopen+is%3Apr+label%3AT-dev-tools
8:00 AM <misdreavus> nrc: my note was about correcting the man pages
8:01 AM <killercup> imperio: didn't you write a css minifier? :P
8:01 AM <•nrc> sorry, https://github.com/rust-lang/rfcs/pulls?q=is%3Aopen+is%3Apr+label%3AT-dev-tools
8:01 AM <•nrc> misdreavus: ah, ok, cool, I'll write this discussion up
8:01 AM <misdreavus> (^^)b
8:02 AM <•nrc> I don't think we need to discuss the Cargo ones
8:02 AM <imperio> killercup: not yet
8:02 AM <imperio> incoming
8:02 AM <•nrc> #1990 is quite new, I think it is a good idea
8:02 AM <•nrc> imperio, misdreavus, steveklabnik: do you roughly think #1990 is a good idea?
8:03 AM <•nrc> (I don't think we need to discuss in more depth here, for now)
8:03 AM <misdreavus> have not read the rfc itself, but +1 for the idea
8:03 AM <imperio> I approved the idea as well
8:03 AM <killercup> the rfc was recently changed to use the `#[doc(include = "docs/example.md")]` syntax fyi
8:03 AM <imperio> (I thought I commented on the PR too...)
8:03 AM <•nrc> cool
8:04 AM <steveklabnik> i am like + 0.5
8:04 AM <steveklabnik> i need to read it in more detail
8:04 AM <imperio> steveklabnik: that's positive !
8:04 AM <steveklabnik> and expect i will end up :+1:
8:04 AM <•nrc> steveklabnik: have you commented on the thread?
8:04 AM <•nrc> oh, ok
8:04 AM <•nrc> imperio: can I assign this to you? I'm not sure assignment is really meaningful anymore, but it is good to have a contact person
8:04 AM <steveklabnik> this week is gonna be "steve focuses on rustdoc stuff" week, so it's been on a backburner for me
8:05 AM <imperio> nrc: sure
8:05 AM <•nrc> #1946 is a bit older
8:06 AM <•nrc> we are running out of time a little bit, so I'd like to push this to next week - it seems that mostly people approve?
8:06 AM <•nrc> possibly some details to work out, but that might be better to look at offline
8:06 AM <misdreavus> this one came up in docs team meetings but iirc we were in favor, generally
8:06 AM <•nrc> cool
8:06 AM <misdreavus> with nits that got discussed in-thread
8:07 AM <•nrc> OK, we're a bit short on time, but quick tools check-in
8:07 AM <oli_obk> clippy here
8:07 AM <•nrc> RLS - nothing much to report - working on some of the intergration with the Rust repo, no blockers
8:07 AM <•nrc> tools peers - what's up? Anything we need to be aware of?
8:08 AM <Manishearth> clippy: development chugging along. waiting on https://internals.rust-lang.org/t/rust-ci-and-submodule-crates/5232
8:08 AM <Manishearth> might want to try and 1.0, probably shortly after that happens
8:08 AM <matklad> IntelliJ Rust -- working on our own type inference, as usual. Oh, and @alygin added support for clippy :)
8:08 AM <•nrc> Rustfmt - we're struggling with Syntex dying - I have a branch that uses libsyntax and I think that is the future, but means moving to Rustup distribution and submodule into the main repo
8:08 AM <oli_obk> clippy 1.0 discussion https://github.com/Manishearth/rust-clippy/issues/1358
8:09 AM <•nrc> Manishearth: we will discuss that at the core team meeting this week
8:09 AM <•nrc> matklad: how does the Clippy integration work? You just shell out to Clippy?
8:10 AM <•nrc> emilio, jwilm: anything to report?
8:10 AM <matklad> That's basically it, and we automatically use nightly for it.
8:10 AM <•nrc> cool
8:10 AM <•nrc> it's been on my mind for the RLS too
8:10 AM <emilio> nrc: not today, I'd like to flag some bindgen issues that need decision for the next triage
8:11 AM <matklad> re libsyntax, I still believe that "parsing rust source code" is a work for a small clean library :)
8:11 AM <•nrc> emilio: cool
8:11 AM <•nrc> matklad: yeah, we played a bit with moving libsyntax to crates.io, but it looks like a lot of work
8:11 AM <•nrc> neither small nor clean :-(
8:11 AM <oli_obk> well, there is https://crates.io/crates/syn
8:12 AM <matklad> I project that this work must be done eventually to do cool stuff for the IDE.
8:12 AM <matklad> but it's definitelly a huge amount of work!
8:12 AM <•nrc> Manishearth, oli_obk: any objections to moving Clippy to rust-lang-nursery? I think that is something we'll want to do as part of the rustup integration, etc.
8:13 AM <oli_obk> no objections
8:13 AM <•nrc> ok, we're at the end of the meeting, time-wise. Thanks everyone! Next triage meeting is in two weeks, and I'll send out info about next week's Rustdoc meeting soon
8:14 AM <steveklabnik> so
8:14 AM <Manishearth> nrc: no objections
8:14 AM <steveklabnik> next week _is_ a us holiday
8:14 AM <steveklabnik> on monday
8:14 AM <Manishearth> nrc: might want to ask everyone on clippy first
8:14 AM <Manishearth> nrc: This has been my goal eventually, what I wanted to do was clippy 1.0 + integration and then move to the nursery
8:14 AM <Manishearth> y'know, give it to the nursery in a good condition :)
8:15 AM <•nrc> Manishearth: I think that 1.0 status and Rustup-ness are kind of orthogonal, but nursery *may* be a prereq for Rustup
8:15 AM <Manishearth> oli_obk: thoughts on 1.0?
8:15 AM <Manishearth> nrc: oh ok
8:15 AM <•nrc> fwiw  Rustfmt is far from 1.0 and is in the nursery
8:15 AM <Manishearth> nrc: no, I know those two are orthogonal, I wanted it to be 1.0 and rustup before it went into the nursery
8:15 AM <Manishearth> wfm
8:16 AM <•nrc> OK, so next weeks meeting
8:16 AM <•nrc> given the holiday shall we move to Tuesday?
8:16 AM <•nrc> steveklabnik, misdreavus , imperio ^
8:16 AM <misdreavus> that depends
8:16 AM <Manishearth> wfm
8:16 AM <steveklabnik> tuesday would be better for me
8:16 AM <•nrc> brson, mw, killercup, jntrnr, japaric ^
8:17 AM <imperio> hum, not sure for me :-/
8:17 AM <misdreavus> i have no idea what vidyo is, but my presence in meetings depends on being able to attend while at dayjob
8:17 AM <japaric> nrc: Tuesday wfm
8:17 AM <killercup> wfm, but i think acrichto mentioned it conflicted with another meeting? libs?
8:17 AM <oli_obk> Manishearth: we need to build a list what stability means for lints, and figure out how we can ensure that we don't break those rules
8:17 AM <jntrnr> nrc: should work
8:17 AM <•nrc> acrichto: won't be around for the rustdoc meeting
8:17 AM <acrichto> (that's ok)
8:18 AM <•nrc> misdreavus: Vidyo is kinda like Skype, I'll send details, but there are clients for Android, Windows, Mac OS, Linux (unreliable) and I think IOS
8:18 AM <Manishearth> oli_obk: hm
8:18 AM <steveklabnik> yeah iOS has one
8:18 AM <misdreavus> but if we did keep it on monday (us time) then it wouldn't matter because i'd be at home and could just use my laptop or whatever
8:18 AM <Manishearth> oli_obk: fwiw we already have this -- "stability gives no guarantees on deny(clippy) or deny(specific_lint)
8:18 AM <Manishearth> this is what rustc does too
8:19 AM <misdreavus> i.e. monday is probably better for this specific meeting for me >_>
8:19 AM <Manishearth> you can expect lints to have fewer false positives and also maybe catch more things as time passes
8:19 AM <oli_obk> Manishearth: well, also we can't remove lints, should not change suggestion order? ...
8:19 AM <Manishearth> oli_obk: yeah, no removing lints
8:20 AM <Manishearth> change suggestion order? meh
8:20 AM <Manishearth> why not?
8:20 AM <•nrc> steveklabnik: if we do it on Monday - would a different time help? (Seems Monday is better for imperio and misdreavus)
8:20 AM <steveklabnik> i am not sure if i have holiday plans at all
8:20 AM <steveklabnik> and what they are
8:20 AM <oli_obk> Manishearth: once IDEs start using suggestions, I don't know if there'll be implications for us
8:20 AM <steveklabnik> so. idk
8:20 AM <Manishearth> oli_obk: hmmmm
8:21 AM <imperio> so much people, kinda hard to schedule anything haha
8:21 AM <Manishearth> oli_obk: i think that's a question to be decided when rustfix is more mature cc killercup 
8:21 AM <•nrc> ok, I'll put off scheduling till the rust-doc meeting
8:21 AM <oli_obk> Manishearth: sgtm
8:21 AM <killercup> nrc: maybe you can send a quick doodle.com thing to also reach the folks not here right now?
8:21 AM <misdreavus> i'd be driving for morning/early afternoon (central us time)
8:21 AM <•nrc> killercup: I think everyone is here
8:21 AM <misdreavus> i.e. a couple hours earlier is good, but no more
8:21 AM <•nrc> oh, except mw, but I don't think he is that interested in Rustdoc
8:22 AM <misdreavus> er, make that like one hour earlier is good, but no more
8:22 AM — misdreavus is remembering travel times
8:22 AM <Manishearth> killercup: how far off do you think rustfix is?
8:23 AM <Manishearth> we do need to improve clippy suggestions as well, but ignoring that?
8:23 AM <Manishearth> if rustfix could fix my unused import warnings id be so happy
8:23 AM — •nrc has to go, will send a doodle and/or jump in the rustdoc meeting to fix a time
```