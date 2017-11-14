# Novemeber 13th, 2017

Triage week, irc meeting in #rust-dev-tools

## Notes

### Automate management of broken tools

* https://github.com/rust-lang/rust/issues/45861
* Some discussion of the pros and cons c.f., toolstate.toml
* Some discussion of implementation details
* Key assumption is that mostly fixing the tools is the responsibility of the tools authors and we should make the process low-friction for Rust contributors

### Current Rustdoc

* Is it worth continuing to work on current Rustdoc, given the existence of new Rustdoc?
  - Yes!
  - Going to be a while before we switchover
  - Frontend code might get re-used
  - Value in iterating on UX issues, tests, experience, etc. (i.e., beyond the code itself)
  - Probably not worth starting big refactorings or other major, non-user-facing work
* Lots been happening in the last few weeks
  - the sidebar has now its own scroll (it's independent from the main doc scrolling)
  - the methods/fields/variants/deref methods/required methods are now in the scrollbar
  - the search screen has now 3 tabs to help users find what they want more easily
  - you can search into generic types
  - more keybindings
  - few optimizations
  - improved docs rendering (titles mainly and some deref methods "badly" displayed)
  - added the crate version flag and made the std docs use it



## IRC log

```
10:04 AM <•nrc> triage
10:04 AM <•nrc> https://public.etherpad-mozilla.org/p/rust-dev-tools
10:04 AM <•nrc> Kenny nominated https://github.com/rust-lang/rust/issues/45861
10:05 AM <•nrc> *kennytm
10:05 AM <•nrc> I am not 100% sure of the CI details as proposed
10:06 AM <•nrc> oli_obk: do you have thoughts on this? It's an alternative/improvement to toolstate.toml
10:07 AM <oli_obk> gh45861 is just email notifications, right?
10:07 AM <•nrc> no, it would mean changing toolstate.toml was unnecessary
10:07 AM <•nrc> for a Rust dev to break tools
10:08 AM <oli_obk> ah, well... isn't the goal that we'll get to the point where motivated parties won't change toolstate.toml, but just update to a new submodule commit?
10:09 AM ↔ jafo nipped out  
10:09 AM <oli_obk> hmm... that could still happen.
10:09 AM <oli_obk> Idea: CI can detect this and push a commit to the PR breaking the tool
10:09 AM <•nrc> oli_obk: given the dance that has to happen with branches, etc., it seems pretty unlikely that anyone will ever do that, unless it is a tool owner making the breaking change
10:10 AM <•nrc> (it is what we wanted originally, but I think using toolstate.toml is the realistic default)
10:11 AM <oli_obk> nrc: yea, but we can automate it by having CI edit the toolstate.toml and commit it to the active PR
10:11 AM <oli_obk> it can then also add `ping @foobar` automatically to the commit message
10:12 AM <•nrc> we thought of that, but it seemed like the effect was the same as just handling things in the CI, but it was more complex to implement because the bot would need to create the commit, etc
10:12 AM → kennytm[mobile] joined (kennytmmobi@moz-fcr.be7.90.113.IP)
10:12 AM <•nrc> kennytm[mobile]: hi, was there anything specific we should discuss about 45861, or just the idea of using CI vs a bot?
10:12 AM <oli_obk> nrc: but we'd have to remember the state somewhere
10:13 AM <oli_obk> otherwise we'd get an email with every further CI-run
10:14 AM <•nrc> yeah, I think the idea was that there would be some kind of dashboard, or we could just use the Travis badge. I guess the bot would need to remember state somewhere, but it seems easier to do that outside the Rust repo
10:15 AM ⇐ kennytm[mobile] quit  
10:16 AM <oli_obk> wouldn't this additionally lead to noone fixing the tools if they break them? Because noone notices that they broke a tool, since only merged PRs ping tool teams?
10:16 AM → kennytm joined (kennytm@moz-fcr.be7.90.113.IP)
10:18 AM <•nrc> I think the idea is that some bot would email the tool contact as notification (I trust a bot to do that more reliably than Rust contributors, tbh)
10:19 AM <oli_obk> nrc: yea, but I meant the PR author won't know about it
10:20 AM <oli_obk> it's a general question. Do we (tool authors) fix tools, or do the PR authors fix the tools they broke?
10:21 AM <kennytm> nrc: sorry the irc client was crashing. i think a dashboard is still desired, as the badge itself doesnt provide information which tool is failing. but either the bot will need to parse the CI log, or the CI job needs to be able to upload the test result to the dashboard
10:21 AM <•nrc> oh, yeah, that's kind of the point :-) It seems that changing toolstate.toml is just ritual, devs don't take it into account in any way and it is a (small) barrier of entry for new contributors
10:21 AM <•nrc> oli_obk: I think we should assume that mostly, we will fix the tools
10:21 AM <oli_obk> nrc: ok, then I'm fine with just getting emails
10:22 AM <•nrc> some Rust devs (think seasoned contributors who think of the big picture) will fix the tools because they're awesome, but I expect that to be the exception, not the rule
10:22 AM <•nrc> kennytm: how hard would it be for the CI job to push the test result somewhere? That seems much easier than polling and parsing
10:23 AM <•nrc> kennytm: "Alternatively, still run the job but mark it as allow_failure. That's rather pointless though" - don't we have to do this to know if the tests fail and thus if we should notify?
10:25 AM <kennytm> nrc: pushing the result shouldnt be hard. the more involved part is the dashboard having a secure endpoint  to receive the result
10:25 AM <•nrc> I'm a fan of having a repo for this kind of thing and letting GH deal with security
10:26 AM <kennytm> nrc: iirc if the allow_failure job failed travis wont send the failure email
10:27 AM <•nrc> kennytm: hmm, are you expecting to just rely on Travis's normal failure mechanism? I was expecting we'd have some dedicated email for this
10:31 AM <kennytm> the dedicated email is sent for tests *after* the PR is merged i.e. on the master branch right?
10:33 AM <•nrc> kennytm: could be, exactly when it is sent doesn't matter too much
10:36 AM <•nrc> OK, it sounds like these are basically implementation issues, lets continue on the issue and move on
10:36 AM <•nrc> only PR is https://github.com/rust-lang/rust/pull/44781
10:36 AM <•nrc> misdreavus: ^
10:36 AM — misdreavus pokes back in from dayjob stuff
10:36 AM <•nrc> misdreavus: is there anything here to discuss or are you iterating on review?
10:37 AM <misdreavus> iterating on review
10:37 AM <kennytm> nrc: if we use GitHub repo to store the dashboard data, would @-mentions in the commit message be good?
10:37 AM <•nrc> ok, cool
10:37 AM <imperio> should be close to an end no?
10:37 AM <misdreavus> i'm just waiting for someone to review it now, tbh
10:37 AM <•nrc> kennytm: yeah, that would probably work
10:38 AM <•nrc> imperio: are you ok to review or does it need more compiler input?
10:38 AM <imperio> I can
10:38 AM <misdreavus> we finally got "load the files during early expansion" so at this point anything farther may just force my hand to say "iterate behind the feature gate"
10:38 AM <imperio> though some others can take a look
10:38 AM <imperio> misdreavus will just require to ping me
10:38 AM <misdreavus> :3
10:38 AM <imperio> because I'm overbooked as always :)
10:38 AM <•nrc> ok, sounds good, thanks!
10:39 AM <misdreavus> he's currently on another pr of mine so i'll let this sit for a moment :)
10:39 AM <•nrc> only relevant PR is https://github.com/rust-lang/rfcs/pull/1615
10:40 AM <•nrc> matklad: ^ any thoughts since our last email?
10:41 AM <matklad> Not really: I still hoping that slightly poking someone would give us a rewritten RFC :) 
10:41 AM <•nrc> ok
10:42 AM <•nrc> Anything else we should discuss from the various tools?
10:42 AM <imperio> hum yes
10:42 AM <imperio> do we have any news concerning the new rustdoc?
10:42 AM <imperio> I'm still a bit reluctant going through huge changes in the current rustdoc
10:42 AM <imperio> I ended the first step today (or yesterday)
10:42 AM <imperio> with the huge improvements in the search and the sidebar
10:43 AM <misdreavus> steveklabnik is currently abroad (and offline?) so i dunno
10:44 AM <imperio> well then I'll ask him
10:44 AM <imperio> first I have misdreavus thing to go through hehe
10:44 AM <misdreavus> but yeah, i haven't heard anything major, but i also haven't really been working on new-rustdoc
10:44 AM <•nrc> aiui there's still progress
10:45 AM <•nrc> but we're not going to be replacing current rustdoc any time soon
10:45 AM <misdreavus> i've just been working on old-rustdoc because i figured it would be super-long-term before new-rustdoc would need to get replacement plans
10:45 AM <misdreavus> make docs beautiful today, etc etc
10:45 AM <•nrc> imperio: I guess it depends on the scale of the change, whether it is worth doing it now for current rustdoc
10:46 AM <•nrc> I think it is still worth doing medium-size things, especially if they are user-facing, but I would avoid any large refactorings/architecture changes
10:46 AM <imperio> nrc: well, if the new rustdoc isn't going to replace the current one in the current year, there's no point asking and I'll just continue
10:46 AM <imperio> no, it's mostly front-end things
10:47 AM <•nrc> current year as in 2017 or the next 12 months
10:47 AM <imperio> the next 12 months ;)
10:47 AM <•nrc> because there is no chance in 2017 :-)
10:47 AM <misdreavus> heh
10:47 AM <imperio> haha
10:47 AM <•nrc> I'd have to check with steve, but my guess is a switch-over would be 6-12 months aways
10:47 AM <misdreavus> that's my assumption too
10:48 AM <imperio> then it's totally worth that I continue my work there
10:49 AM <imperio> I just wonder if all the JS I've been working on will be moved over in some way
10:49 AM <imperio> this area isn't clear to me
10:49 AM <misdreavus> depends on whether they make a new ui or not
10:49 AM <misdreavus> but again, that's fairly far future
10:50 AM <imperio> but still pretty important :)
10:50 AM <imperio> if I make changes and they get remove a few months later, are they worth the effort? ;)
10:50 AM <misdreavus> will your changes greatly improve how people interact with docs in those few months?
10:51 AM <imperio> I think they did until now
10:51 AM <imperio> and I think they will
10:51 AM <misdreavus> then yes
10:51 AM <•nrc> 👍
10:51 AM <imperio> even though they'd get removed?
10:51 AM <imperio> (that's what scares me)
10:52 AM <misdreavus> so, here's what i want to happen when the rustdoc switchover happens:
10:52 AM <misdreavus> librustdoc gets moved out of tree to some other repo
10:52 AM <•nrc> I think there is value on iterating in user-facing features (improving UX, tests, etc.) far beyond the actual code. I think that the code might get re-used, but the rest is still valuable even if it doesn't
10:52 AM <misdreavus> i/someone make(s) a cargo vintage-doc wrapper that calls "old rustdoc"
10:52 AM <•nrc> *in iterating on
10:53 AM <misdreavus> i keep working on old rustdoc because i've grown attached, dammit
10:53 AM <imperio> misdreavus: but if it gets removed from rust repository, it'll just get lost
10:53 AM <•nrc> and I would expect that an 'old rustdoc skin' for new rustdoc would use quite a lot of the existing frontend code (but that depends a bit on how things turn out_
10:53 AM <misdreavus> but yeah, improving the ui also provides an example for the new one to base from
10:53 AM <imperio> all this makes me very uncomfortable :-s
10:54 AM <•nrc> imperio: I don't think that is true - we're moving tools out of the Rust repo - new rustdoc will never live in the Rust repo
10:54 AM <imperio> I don't mind switching to a new UI if it's better
10:54 AM <imperio> but working for nothing isn't really pleasing
10:54 AM <misdreavus> even if i have to host vintage-rustdoc myself, i'm determined to save it
10:54 AM <imperio> misdreavus: I'm all for it!
10:54 AM <imperio> but it'll only be the two of us I think haha
10:55 AM <misdreavus> it already is only the two of us, lol
10:55 AM <imperio> well, working on it yes
10:55 AM <imperio> but everyone uses our code indirectly
10:55 AM <misdreavus> yeah
10:55 AM <imperio> well, anyway
10:55 AM <imperio> I'll make the changes I have in mind
10:56 AM <imperio> the second step will make a lot of people happy I hope
10:56 AM <•nrc> imperio: what do you have planned?
10:56 AM <misdreavus> i think i mentioned this somewhere a while back - "major" additions are fine, refactorings are probably not
10:56 AM <imperio> nrc: did you follow last changes I made into rust docs?
10:57 AM <imperio> misdreavus: I don't refactor (at least not the rust code)
10:57 AM <misdreavus> imperio: then you have nothign to worry about
10:57 AM <imperio> well, I refactored JS
10:57 AM <imperio> quite a lot
10:57 AM <imperio> and I'll continue to
10:57 AM <•nrc> imperio: I missed quite a bit while I was on leave
10:57 AM <imperio> nrc: ok so let me write a list
10:57 AM <misdreavus> heh
10:58 AM <imperio> * the sidebar has now its own scroll (it's independent from the main doc scrolling)
10:58 AM <misdreavus> iirc the most recent thing was scrolling the sidebar independently and adding loads of things to it
10:58 AM <imperio> * the methods/fields/variants/deref methods/required methods are now in the scrollbar
10:59 AM <imperio> the search screen has now 3 tabs to help users find what they want more easily
10:59 AM <misdreavus> oh yeah, that too
10:59 AM <imperio> * you can search into generic types
10:59 AM <imperio> * more keybindings (just two for the monent, I want to finish all changes before adding more)
10:59 AM <imperio> * few optimizations
11:00 AM <imperio> * improved docs rendering (titles mainly and some deref methods "badly" displayed)
11:00 AM <imperio> and I think that's it
11:00 AM <imperio> certainly forgot a few things
11:01 AM <•nrc> nice! I noticed a few of them when I was using rustdoc'
11:01 AM <imperio> if you try the current nightly docs, two things: it's slow and don't have generics
11:01 AM <imperio> both issues are fixed now (next nightly docs generation will make it real for users)
11:02 AM <misdreavus> i added the crate version flag and made the std docs use it, i forget how recent that was
11:02 AM <imperio> misdreavus: oh right, it was a nice add
11:02 AM <imperio> quite useful for me haha
11:03 AM <misdreavus> document impls when the type appears in a trait's generics
11:03 AM <•nrc> and what do you have in mind coming up?
11:03 AM <misdreavus> my major ones are still in flight, heh
11:04 AM <imperio> nrc: a bit hard to explain
11:04 AM <imperio> how to sums it up...
11:04 AM <imperio> huuuum
11:04 AM <misdreavus> for me, doc(include) which was linked earlier, and figuring out a way to make people not complain about how https://github.com/rust-lang/rust/pull/45039 looks
11:04 AM <imperio> for now, the docs look too much like rust code
11:04 AM <misdreavus> imperio: for me that's a plus, tho
11:05 AM <imperio> I'll make it looks like docs with possibility to make it look more like rust
11:05 AM <imperio> misdreavus: it is nice, but slow down the reading
11:05 AM <misdreavus> in what way?
11:05 AM <imperio> for impl with a lot of types for example
11:06 AM <imperio> example: "impl<'a, T> From<&'a [T]> for Vec<T>" (a simple one)
11:06 AM <•nrc> misdreavus: re 45039, could you put the important traits in a tool tip when you hover the return type?
11:07 AM <imperio> if you're not used to read rust code (well even if you are), you need to analyze this to understand what this is about
11:07 AM <•nrc> imperio: interesting
11:07 AM <•nrc> sounds good
11:07 AM <misdreavus> nrc: imperio is working on something like that, but i didn't know the best way to write that myself
11:07 AM <imperio> nrc: about 45039, we intend to put it into a popup instead (popup window like the helper)
11:07 AM <misdreavus> for as much as i work on rustdoc's frontend, i'm not well experienced in web design >_>
11:07 AM <imperio> haha
11:08 AM <imperio> nrc: so my next steps will be about how to make docs faster and easier to read and to allow to have all the information we have now at the same time
11:08 AM <imperio> I have a few things in mind to make it happen
11:08 AM <•nrc> misdreavus: just stick the text into the `title` attribute
11:08 AM <imperio> I'm already experimenting a few things
11:09 AM <•nrc> good stuff, I'm looking forward to what you come up with!
11:09 AM <imperio> nrc: or in the dom JS directly
11:09 AM <misdreavus> the title is already in use for the full type path
11:09 AM <•nrc> misdreavus: why not both?
11:09 AM <imperio> nrc: all this comes from mozilla firefox developers
11:10 AM <imperio> they invited me for a lunch and they suggested this
11:10 AM <•nrc> what is "this"?
11:10 AM <imperio> that the doc wasn't easy to read enough
11:10 AM <misdreavus> i guess if you had that in a little glyph next to the type that would be nice
11:10 AM <imperio> misdreavus: isn't it what you wanted?
11:11 AM <misdreavus> imperio: i thought it would be fancier than title text
11:11 AM <imperio> I agree on it
11:11 AM <•nrc> ah, right, cool
11:11 AM <imperio> now you know
11:11 AM <•nrc> :_)
11:11 AM <•nrc> :-)
11:11 AM <imperio> I'm happy with the new search functionalities
11:12 AM <imperio> the doc is more accessible than ever
11:12 AM <imperio> but still so much things to do
11:12 AM <imperio> it'll be interesting
11:12 AM <misdreavus> after using the new sidebar, i'm really excited about what you've done recently
11:12 AM <imperio> it's just the beginning
11:12 AM <misdreavus> <3
11:12 AM <imperio> I wanted at first to make only one PR for the sidebar and all the items inside it
11:12 AM <imperio> then I thought that you might have issues with so much at once haha
11:13 AM <misdreavus> lol
11:13 AM <imperio> anyway
11:13 AM <imperio> for now, coming back to misdreavus modal
11:13 AM <imperio> nrc: anything you'd like me to do on docs (suggestions or anything you'd like to have?)?
11:14 AM <misdreavus> i'm so used to what we already have, lol
11:14 AM <misdreavus> after i scratched my itch back a year ago with reformatting the where clauses and function signatures, i was happy, lol
11:14 AM <imperio> haha
11:14 AM <misdreavus> now look at me
11:14 AM <imperio> I was as well because we're used to it
11:15 AM <imperio> but with my experiments, I just realised how much it could be improved
11:15 AM <•nrc> hmm, well, one thing is that I nearly never want the expanded view of all functions to start with, I wonder whether we might change the default to compressed or add a contents/summary view or something?
11:15 AM <misdreavus> yeah
11:15 AM <imperio> the important information isn't as easy as spot as what we might think unfortunately
11:15 AM <imperio> nrc: the expanded view?
11:15 AM <misdreavus> docblocks unfolded
11:16 AM <imperio> ooooh
11:16 AM <•nrc> yeah
11:16 AM <imperio> well, it makes sense to have it unfolded now
11:16 AM <imperio> since you can access very quickly through the sidebar
11:16 AM <misdreavus> i have a back-of-my-head idea to make the "toggle all" thing a global setting that goes into local storage
11:16 AM <imperio> and you can fold/unfold everything with "+" or "-"
11:16 AM <imperio> misdreavus: didn't we talk about it?
11:16 AM <misdreavus> probably
11:16 AM <•nrc> but the sidebar doesn't have types, and usually I want to scan the types as well as names
11:16 AM <misdreavus> it's an open issue
11:17 AM <imperio> but yes, I think it's a good and a bad idea at the same idea
11:17 AM <imperio> don't know yet]
11:17 AM <imperio> nrc: then maybe misdreavus's idea will please you ;)
11:18 AM <•nrc> I think I would be in favour of remembering the default, for sure
11:18 AM <imperio> I just wonder if it'd need a menu or just remembering the last user's action
11:19 AM <misdreavus> if it were just that, i think we could get away with just remembering the last action
11:19 AM <•nrc> my other niggle is that when you go to a search view, it loads the front page first, which is a bit irritating
11:19 AM <•nrc> yeah, keep it simple, just remember the setting
11:19 AM <misdreavus> (brb)
11:20 AM <imperio> nrc: the front page?
11:22 AM <misdreavus> (back)
11:22 AM <•nrc> e.g., if I follow https://doc.rust-lang.org/nightly/std/index.html?search=Vec I see the docs for std for a second or two before I see the search results
11:22 AM <misdreavus> yeah, that's a thing
11:23 AM <imperio> ooooh
11:23 AM <imperio> I see
11:23 AM <imperio> nrc: it's an issue so please open one and put me on it ;)
11:26 AM <•nrc> shall do
11:26 AM <imperio> thanks
11:26 AM <imperio> misdreavus: btw: https://github.com/rust-lang/rust/pull/45970 (haha)
11:27 AM <misdreavus> lol
11:27 AM <imperio> nrc: as well, if you have anything coming to your mind (bug, suggestions of new feature, improvements), don't hesitate to share it with me and misdreavus 
11:27 AM <misdreavus> yes, please!
11:27 AM <imperio> any idea is good to take
11:28 AM <misdreavus> depending on my rustdoc mood i may brush it off initially but it'll still be good
11:28 AM <•nrc> imperio: shall do
11:28 AM <imperio> misdreavus: you're faster and faster to review XD
11:28 AM <misdreavus> imperio: the pr was tiny
11:28 AM <imperio> yep but just opening a PR takes me between 5 min and 5 months you know
11:28 AM <misdreavus> lol
11:28 AM <imperio> I write them way more quickly
11:29 AM <•nrc> oh one more thing, I think the quality of search could be improved (I have no idea about how to do search, so I'm not sure if this is easy or hard). For example, when searching for "Vec" we probably shouldn't shopw results for "std::f32::consts::E" or "*eq"
11:29 AM <imperio> might be worth retrying when the generics kick in
11:30 AM <imperio> search can still be improved a bit
... [some discussion of details of search]
```
