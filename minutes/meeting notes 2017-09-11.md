# September 11th 2017

Proactive week: RLS clients, irc meeting in #rust-dev-tools

## Notes

* RLS is basically 'done' for now, feature-wise
  - current effort mostly around more complex project configurations, e.g., Cargo workspaces
  - and stability/bug fixing
  - aiming for 1.0 around the new year
  - big internals improvements from compiler after that
* VSCode extension is looking good!
* Good time to focus on wider editor support
  - Some support already for Atom and NeoVim - kinda active
  - beginnings of things for emacs
  - xi: work hasn't seriously started yet (i think there may be an initial prototype) but would like to later this fall
* Vim is most popular editor in Rust world, followed by VSCode, Sublime, Atom, Emacs (these three roughly equal shares)
* In wider world Visual Studio is most popular
  - https://insights.stackoverflow.com/survey/2017#technology-most-popular-developer-environments-by-occupation
  - Potential for recruiting users
  - Necessary for being a serious Windows platform
  - However, no LSP client implementation, so harder work (but still do-able)
* Some discussion of useful things in the RLS, see action items below.


## Action items

* Improve documentation (nrc)
  - config options
  - LSP coverage (i.e., which parts we use)

* Website (nrc)
  - finish
  - where to host?

* For impl period
  - issues for editor implementations (nrc)
  - write-up of what needs doing for Visual Studio (jntrnr)


## Follow-up

Atom announced atom-ide - first-class LSP support in Atom: https://blog.atom.io/2017/09/12/announcing-atom-ide.html

rovar missed the meeting, but has done some work using RLS with Vim8: https://github.com/rrichardson/rustlangserver.github.io/blob/vim8/files/install_vim_rls.sh


## IRC log

```
8:02 AM <•nrc> hello!
8:03 AM <•nrc> today we are talking RLS clients (in irc)
8:03 AM <•nrc> the agenda is here: https://public.etherpad-mozilla.org/p/rust-dev-tools
8:03 AM <•nrc> I'll just give people another couple of minutes before starting...
8:03 AM <jntrnr> k
8:05 AM <vlad20012_> o/
8:05 AM <matklad> btw, vlad20012_ now also works on IntelliJ Rust :)
8:05 AM <jntrnr> cool :)
8:06 AM <•nrc> hi vlad20012_ !
8:06 AM <•nrc> OK, so
8:07 AM <•nrc> today I want to go over the current status of the RLS quickly, talk about prioritisation of work for new RLS clients, and get a handle on what work on clients is underway and what is blocked on what
8:08 AM <•nrc> If anyone working on clients wants to bring something up, pop it on the agenda - we should have time for extra stuff, if not I can stick around to discuss after the meeting
8:09 AM <•nrc> to start with, very quickly, the RLS is (I think) in a pretty good place - it's use of the LSP is pretty stable, most core features are implemented, and it is fairly stable (i.e., doesn't crash too much)
8:09 AM <jntrnr> did it make it onto the train?
8:09 AM <•nrc> information is till imperfect  - a lot of that is bugs in the save-analysis layer of the compiler
8:10 AM <•nrc> jntrnr: yes, although we are not quite installable on beta yet
8:10 AM <•nrc> ^ this means we should be able to use the RLS with stable in 5 weeks time
8:10 AM <•nrc> the biggest remaining feature is workspace support
8:11 AM <•nrc> that brings with it some more stability for projects which are a bit more complicated than a single bin or library (which is where we currently have the most problems)
8:11 AM → llogiq joined (Andre@moz-581.j04.146.5.IP)
8:11 AM <•nrc> the reference client is also working pretty well - still some bugs, but pretty well
8:11 AM <jntrnr> this is the vscode plugin?
8:11 AM <•nrc> so, I think it is a good time to be thinking about other clients
8:11 AM <•nrc> yes
8:12 AM → vlad20012_ joined (vlad20012_@moz-4ts.mal.185.93.IP)
8:13 AM <•nrc> hopefully we'll have a 1.0 release of both the RLS and reference client (VSCode) early in the new year
8:13 AM <•nrc> anyone have questions/comments about the RLS status?
8:14 AM <raph_rc> comment: yay!
8:14 AM <jntrnr> yeah, one question - do we have a sense from the compiler team when the frontend work will land that will let us drive more straight from the compiler?
8:15 AM <•nrc> around the end of the year
8:15 AM <•nrc> so, to expand on that
8:15 AM <•nrc> there are some big changes to the compiler that will improve RLS latency and hopefully make it useful with large projects
8:15 AM <•nrc> incremental compilation to type checking and demand-driven compilation
8:16 AM <•nrc> they should also allow us to do compiler-driven code completion, which should be more complete
8:16 AM <•nrc> however, there are some big changes to take advantage of this
8:16 AM <•nrc> so it is going to be a post-1.0 thing
8:17 AM <•nrc> ok, other editors
8:17 AM <•nrc> there is a tracking issue here: https://github.com/rust-lang-nursery/rls/issues/87
8:18 AM <•nrc> Gnome Builder seems to be the most actively developed extension atm
8:18 AM <•nrc> see this comment https://internals.rust-lang.org/t/interested-in-editor-support-for-the-rls/5873/4?u=nrc
8:19 AM <raph_rc> i'd like to see xi added to the client list, even though work has barely started
8:19 AM <jntrnr> Christian is also very responsive.  If we have questions and want to reach out to him for comment about working with RLS, etc
8:20 AM <raph_rc> i found the proof-of-concept impl here: https://github.com/google/xi-editor/issues/160#issuecomment-294995411
8:20 AM <•nrc> added :-)
8:21 AM <•nrc> Otherwise, Atom and NeoVim have plugins and are somewhat active
8:22 AM <•nrc> Anyone know of other work going on?
8:22 AM <•nrc> oh emacs too
8:22 AM <jntrnr> is emacs active?
8:22 AM <•nrc> it does not seem active
8:22 AM <•nrc> https://github.com/emacs-lsp/lsp-rust
8:23 AM <jntrnr> unfortunate
8:23 AM <bkchr> I use rls with emacs
8:23 AM <•nrc> although it has had a couple of PRs recently
8:23 AM <•nrc> ah from bkchr :-)
8:23 AM <•nrc> bkchr: how is support?
8:24 AM <bkchr> it is "okay". Most things work
8:25 AM <bkchr> Currently there are some bugs with parsing the message headers
8:26 AM <bkchr> I use spacemacs with the following layer: https://github.com/bkchr/rustrls/tree/my_branch
8:27 AM <•nrc> you use that and the RLS/LSP support? Do they conflict?
8:28 AM <bkchr> No, that layers uses the lsp-mode ^^
8:28 AM <bkchr> It is just an integration of the lsp-mode
8:28 AM <bkchr> With shortcuts etc
8:28 AM <•nrc> ah, ok
8:28 AM <•nrc> is the lsp-mode just in your branch, or upstream too?
8:29 AM <bkchr> just my branch. 
8:30 AM <bkchr> for upstream it is not stable enough, in my own opinion^^
8:30 AM <bkchr> Currently I'm using it daily
8:30 AM <•nrc> because of the bugs parsing message headers?
8:31 AM <bkchr> yeah and the overall quality of rls :D
8:31 AM <bkchr> https://github.com/rust-lang-nursery/rls/issues/470 this one for example
8:34 AM <•nrc> so, that leads pretty well into my big open question - what we can do on the RLS side to help client devs?
8:35 AM <•nrc> that kind of issue (470, etc - stability with different build setups) is high priority for me, once we riding the trains properly
8:36 AM <bkchr> Hmm good question, maybe good documentation for the options for example?
8:36 AM <llogiq> I think making it as easy as possible to set up (and keep up to date) with various editors/IDEs is the way to bring in more testers.
8:36 AM <jntrnr> nrc: I want to talk about this, but I think we also skipped the bullet between them
8:37 AM <•nrc> jntrnr: the stackoverflow link?
8:37 AM <jntrnr> basically that that Visual Studio, vim, and Sublime need some level of support
8:37 AM <jntrnr> but it seems like currently we don't have that
8:37 AM <jntrnr> (I didn't really phrase the bullet well)
8:38 AM <•nrc> llogiq: do you have suggestions for how to do that? Beyond better docs?
8:38 AM <•nrc> jntrnr: ok, yeah, I didn't get that :-) lets come back to that in a sec - it meshes with prioritisation
8:38 AM <jntrnr> kk
8:39 AM <•nrc> bkchr: by options to do you mean the specific configuration options?
8:39 AM <bkchr> nrc: yeah
8:39 AM <bkchr> nrc: I know where to look in the code, but for others it is maybe more complicated
8:41 AM <llogiq> nrc: better docs, perhaps install scripts per editor/distribution/OS combination, advertising the possible options via blogs, etc.
8:43 AM <•nrc> good ideas!
8:44 AM <jntrnr> nrc: have we talked about the rls website?
8:44 AM <•nrc> raph_rc: anything you think that would help you later with RLS support?
8:44 AM <•nrc> ah, yes, booyaa[ has been working on a website for the RLS
8:45 AM <•nrc> it covers a few of the clients
8:45 AM <•nrc> https://github.com/rust-lang-nursery/rls/issues/261
8:46 AM <•nrc> it is pretty great! We need to figure out where to put it online and then advertise it a bit
8:46 AM <jntrnr> you can play with it here: http://rls.booyaa.wtf/
8:46 AM <jntrnr> not sure if 100% up to date but you get the idea
8:46 AM <raph_rc> nrc: i'm not aware of anything right now, but am happy to provide feedback as it comes up
8:46 AM <•nrc> raph_rc: thanks!
8:46 AM <raph_rc> we're in the fortunate place where we can make design decisions in xi to support rls
8:49 AM <•nrc> OK, so in terms of moving forward
8:49 AM <•nrc> we have the impl period coming up
8:49 AM <•nrc> which is meant to be a few months where the Rust community is very implementation-focussed
8:49 AM <•nrc> and a good time for getting new contributors involved and people working on new things
8:50 AM <•nrc> which seems like a good time to try and move the needle a bit on clients
8:50 AM <•nrc> I wonder how many clients we should focus on?
8:51 AM <•nrc> and how to pick the clients
8:51 AM <jntrnr> I think finishing what we have is important
8:51 AM <•nrc> as jntrnr pointed out above, the stackoverflow survey showed VS, Vim and Sublime to be basically the most  popular
8:52 AM <jntrnr> and also three we don't have good support for
8:52 AM <•nrc> but none of those have extensions started
8:52 AM <jntrnr> right
8:52 AM <jntrnr> just one data point: VS had 66% of the stackoverflow survey, but only 1.5% of the Rust survey
8:52 AM <•nrc> so, I'm not sure how much weight to give new clients, vs pushing on existing ones
8:52 AM <jntrnr> that's a *ton* of potential new developers getting onboard
8:53 AM <mw> does VS integrate with the LSP?
8:53 AM <jntrnr> no
8:53 AM <jntrnr> I tried to ask around to see if they'll ever do that but didn't get any straight answers
8:53 AM <•nrc> ah, so that would be a lot of work then?
8:54 AM <jntrnr> no more than a normal plugin
8:55 AM <•nrc> I guess you'd only have to implement the parts of the LSP you need, which is I think is like 50%
8:56 AM <raph_rc> nrc: so _that_ would be helpful, if there is only a fragment of the LSP that's need, documenting what that is
8:56 AM <raph_rc> obviously not as important for clients (like vs code) that already implement ~all of it
8:56 AM <•nrc> OK, I can def add that
8:57 AM <raph_rc> i imagine it might be a moving targets, more coverage as more features get added
9:03 AM <•nrc> sorry, internet cut out
9:03 AM <•nrc> it is moving, but not that quickly - we're more using existing parts and adding custom extensions, rather than using more of the existing parts
9:04 AM <•nrc> tools team - is anyone interested in getting more involved with RLS clients? Either implementation or coordination (testing, tracking status, etc.)
9:06 AM <•nrc> jntrnr: would you be able to write up some info about VS? If someone is interested, where to find info on VS extensions and the RLS, what language they'd be using, etc.?
9:06 AM <jntrnr> jntrnr: I wish I had the information.  I never worked on that side myself
9:06 AM <jntrnr> but I could try to dig around and find it
9:08 AM <•nrc> that would be great, thanks! Doesn't need to be super-details, just so someone can see whether they'd like to get involved
9:09 AM <•nrc> OK, we're already a bit overtime, thanks everyone!
```
