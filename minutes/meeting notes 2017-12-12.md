# December 12, 2017

Triage week, irc meeting in #rust-dev-tools

Mostly discussing [RFC #2234](https://github.com/rust-lang/rfcs/pull/2234)

## IRC log

```
[21:59:17]  <~nrc>	Hi all! Meeting time!
[21:59:41]  <~nrc>	We might be a little short-handed today since there is a Mozilla all-hands this week, so most Moz folk will be travelling today
[21:59:46]  <matklad>	o/
[21:59:51]  <killercup>	o/
[21:59:57]  <~nrc>	https://public.etherpad-mozilla.org/p/rust-dev-tools
[22:00:18]  <~nrc>	There are no nominated issues and one PR with an accompanying RFC - https://github.com/rust-lang/rust/pull/46450
[22:00:32]  <~nrc>	https://github.com/rust-lang/rfcs/pull/2234
[22:02:26]  <killercup>	i think we should decide as soon as possible if we want to wait for custom test runners before doing anything like this
[22:02:52]  <oli_obk_>	how will custom test runners change anything related to the output?
[22:02:55]  <killercup>	or if we're fine with focussing on improving/specifying the current test runner
[22:03:03]  <oli_obk_>	the possibility for hierarchies?
[22:03:05]  <~nrc>	so
[22:03:18]  <killercup>	oli_obk_: it's orthogonal in every aspect but dev time
[22:03:25]  <~nrc>	I think we definitely want to be looking at custom test runners next year
[22:03:36]  <~nrc>	so we need to work out how this fits into that story
[22:03:43]  <~nrc>	I think it is not orthogonal:
[22:03:55]  <~nrc>	different test runners might faciliate emitting data in different ways
[22:04:00]  <matklad>	In theory, custom test runners can be used to tweak output format
[22:04:01]  <~nrc>	e.g., JSON, XML, etc.
[22:04:12]  <~nrc>	I don't want to have to build every format into the compiler/libtest
[22:04:12]  <matklad>	this is how intellij for java works
[22:04:23]  <~nrc>	also different human-readable forms too
[22:04:29]  <matklad>	we feed JUnit and Scala analogue our own classes which produce intellij-specific format
[22:04:32]  <oli_obk_>	wait... why would the test runner decide the format?
[22:04:36]  <killercup>	i don't mean the implementation per-se but the specification of json output schema
[22:04:43]  <~nrc>	oli_obk_: why would it not?
[22:05:12]  <oli_obk_>	nrc: the test runner decides what data to output, the `cargo test` caller decides in what format the data is supposed to be produced
[22:05:14]  <~nrc>	IMO, the test runner should be able to change as much as possible about testing
[22:05:48]  <~nrc>	there has been talk about separating the test framework from the test harness
[22:06:07]  <~nrc>	I think the harness would basically replace any infrastructure in cargo or the compiler
[22:06:24]  <~nrc>	so `cargo test` just delegates to the harness, which can output test data however it likes
[22:06:58]  <killercup>	yeah, that's why specifying a machine readable format is quite important IMHO. so people can use different test runners but still use the same tools to consume their results
[22:07:37]  <killercup>	(or at least the lowest common denominator)
[22:07:40]  <matklad>	For machine readable output, I think it is important to have a *single* machine readable output format which all test runners support. 
[22:07:56]  <oli_obk_>	matklad++
[22:08:00]  <killercup>	^ excatly
[22:08:03]  <oli_obk_>	that's how I thought it would happen
[22:08:15]  <matklad>	otherwise, you'll have to change the runner to make it work in IDE, which is not great I think. 
[22:08:30]  <~nrc>	I think there should be some standard at some point in the pipeline, either API or data format
[22:08:47]  <oli_obk_>	--output-format for cargo and rustc also only support json. What's the advantage of allowing any format?
[22:08:50]  <matklad>	Also, it seems to me that we don't need to support *different* machine readable formats out of the box, the single one would suffice.
[22:08:59]  <~nrc>	it could be that the output is controlled by the IDE, e.g., to produce IntelliJ-specific format or something
[22:09:20]  <matklad>	It's the responsibility of the client to massage given single format into something the client can undersand. 
[22:09:27]  <oli_obk_>	creating a json -> toml converter with serde is like 10 lines of code, including boilerplate
[22:09:31]  <~nrc>	oli_obk_: there are various 'standard' test output formats floating around, it seems desirable to support them
[22:09:47]  <oli_obk_>	nrc: ah, ok, that is the motivation, didn't know about that
[22:09:59]  <~nrc>	anyway, I think we don't need to go too far down this rabbit-hole :-)
[22:10:13]  <~nrc>	we clearly have a long term thing and a possible short-term thing
[22:10:28]  <~nrc>	I think we probably want to iterate on the JSON format in any case
[22:10:36]  <killercup>	i actually think the format proposed by the rfc is quite nice as a first draft and something to build upon. the a-json-document-per-event thing should be quite easily extensible
[22:10:42]  <~nrc>	so having something like this PR to start with seems like a good idea
[22:10:43]  <killercup>	nrc: yep
[22:10:48]  <~nrc>	and then we can specify it later
[22:10:58]  <oli_obk_>	my point stands: create converters from json to any of the standard test output formats, instead of supporting multiple formats
[22:11:11]  <matklad>	nrc: One big question is if there is an existing format which we can use?
[22:11:12]  <killercup>	i agree, oli_obk_
[22:11:46]  <matklad>	I've reserched this question, and it seems that the answer is no, and we have to roll our own (what the RFC/PR does).
[22:12:04]  <~nrc>	oli_obk_: perhaps, there are downsides there too, but I think this is a question for later
[22:12:17]  <matklad>	oli_obk_: +1, one machine readable format should suffice. 
[22:12:18]  <~nrc>	matklad: good to know, thanks for looking into it!
[22:12:32]  <oli_obk_>	yea, having an unstable interface now allows us to try to check whether there are downsides other than a little overhead
[22:13:08]  <matklad>	There's TAP, which seems like it is more-or less popular, but I think it's not a good fit: not really extendable, text-based, oriented towards single-threaded test execution
[22:13:46]  <killercup>	matklad: i did some research as well for the last rfc for this, there was a tap yaml thing IIRC and a json version of it, but it seemed dead in the water
[22:13:48]  <~nrc>	so, proposal: we ensure that the PR only adds unstable stuff, make clear it is the start of the process towards a long-term design, postpone the RFC until we have some experience with the implementation
[22:13:57]  <~nrc>	thoughts?
[22:14:03]  <killercup>	matklad: can you comment on the rfc to add some "prior art" links?
[22:14:06]  <oli_obk_>	:+1:
[22:14:11]  <~nrc>	oh, and land the PR, of course :-)
[22:14:17]  <matklad>	killercup: will do!
[22:14:24]  <~nrc>	thanks matklad !
[22:14:32]  <matklad>	nrc: great plan, but I want to discuss one open question
[22:14:53]  <matklad>	The RFC does not specify what happens with stdout/sterr of the tests
[22:14:54]  <killercup>	nrc: sounds good! alternatively, re-label it ERFC?
[22:15:33]  <matklad>	and it is actually a hard problem: in general (parallel test execution), it is impossible to map text output to each test.
[22:15:43]  <~nrc>	killercup: I think an eventual specification would be very different because of pluggable test runners, so I don't think it is worth keeping it open
[22:16:02]  <~nrc>	matklad: what exactly do you mean by "what happens with..." ?
[22:16:28]  <matklad>	Like, let's suppose that test has a `println!("Hello")` line in it
[22:16:38]  <matklad>	What happens to `Hello`?
[22:16:42]  <matklad>	Is it output as is?
[22:16:55]  <~nrc>	I thought all stdio output was captured by the test runner?
[22:17:02]  <matklad>	Or is it wrapped into a message `{type: output_line, text: "Hello"}`
[22:17:14]  <matklad>	Or is it captured, and output after the fact?
[22:17:26]  <killercup>	matklad: each test is executed in a thread with its own std{out,err}, so you could capture it and output it as "stdout" field on the ok/failed event?
[22:17:48]  <matklad>	What happens if test prints `hello` in a loop, waiting 10 ms between iterations?
[22:17:51]  <killercup>	i'd capture everything by default, but that's just a gut feeling
[22:17:51]  <~nrc>	I would expect the former, but I've not put a lot of thought into it
[22:18:05]  <matklad>	Do we see output as it is appearing, or do we see it only after the end of the test?
[22:18:20]  <~nrc>	currently, only at the end of the test and only if the test fails
[22:18:42]  <~nrc>	(for stdout, maybe stderr is reported as the test goes along, not sure0
[22:18:42]  <matklad>	>only if the test fails
[22:18:43]  <matklad>	Not neccessry, there's `--nocapture`
[22:18:44]  <killercup>	if this is the machine readable output, who see it (and untderstands it) when it appears?
[22:19:00]  <matklad>	And for machine readable output, we want to always give output I think.
[22:19:10]  <~nrc>	yeah, my feeling is that if the user specifices `--nocapture` all bets are off and we do nothing with stdio
[22:19:13]  <matklad>	killercup: the user of the IDE
[22:19:50]  <matklad>	Like, IDE needs machine output to show which test is currently running, which tests already failed, et.c
[22:19:58]  <matklad>	but it also wants to show text output.
[22:20:13]  <matklad>	capture + output after the fact really sucks for printf debugging.
[22:20:15]  <~nrc>	I would think that if you want JSON output you don't want `--nocapture`
[22:20:32]  <~nrc>	and we shouldn't worry about the combination
[22:20:33]  <killercup>	matklad: okay, they see it how? they don't see the json. so the ide display every non-json line? that's weird. i'd either add a new event type and wrap the output in a json doc as well, or only output it at the end of the test's run
[22:21:14]  <matklad>	killercup: yep, IDE processes the output and shows only stdout/stderr.
[22:21:59]  <matklad>	So, IDEs need both JSON + live output streaming.
[22:22:08]  <matklad>	Does these constraints make sense?
[22:22:09]  <killercup>	sounds like --nocapture should generate {type: "test", event: "stdout", content: "…"}
[22:22:41]  <matklad>	killercup: yep, that sounds ideal!
[22:22:54]  <killercup>	which to me means it should totally also add an "id" field
[22:23:03]  <killercup>	so you know which test it came from
[22:23:13]  <matklad>	Yep!
[22:23:15]  <~nrc>	matklad: it doesn't make sense to me - why do IDEs need a live stream of output?
[22:23:27]  <~nrc>	rather than using stdio embedded in the JSON?
[22:23:58]  <matklad>	Well, it can be embedded into JSON, but it must be available live, and not after the test passes
[22:25:30]  <~nrc>	why? We don't do that in the cargo test runner
[22:25:45]  <killercup>	matklad: if we managed to add unique test identifiers, anything regarding --nocapture can be implemented in a second pr, though, right?
[22:27:56]  <~nrc>	I would be in favour of leaving any fancy --nocapture stuff till later
[22:28:15]  <matklad-hates-riot>	sorry, I've dropped out for a bit
[22:29:08]  <matklad-hates-riot>	I am afraid we won't be able to take advantge of JSON in intellij before the probelm with output is sorted out
[22:29:28]  <~nrc>	why is it so important?
[22:29:45]  <killercup>	matklad-hates-riot: even if you assume the default case is "nothing was output"?
[22:29:54]  <matklad-hates-riot>	We would have implemented parsing of current text output, if not for the text output problem
[22:30:30]  <~nrc>	matklad: why is it important to run with `--nocapture`?
[22:30:31]  <matklad-hates-riot>	nrc: So, there are two bits here 1) machine readable output 2) live test output
[22:31:00]  <matklad-hates-riot>	we all uderstand why machine readable outupt is important, right? So I'll need to explain live test output
[22:31:18]  <matklad-hates-riot>	The main reason is printf debugging
[22:31:18]  <matklad-hates-riot>	suppose you have a test that runs for like one minute
[22:31:21]  <matklad-hates-riot>	and you are debugging it.
[22:31:23]  <killercup>	ohh! did you see the rfc actually talks about a "test_output" event?
[22:31:49]  <matklad-hates-riot>	killercup: hm, no, I thought it didn't talked about it :)
[22:32:19]  <killercup>	i've been writing some comments and just came across this: https://github.com/rust-lang/rfcs/pull/2234/files#diff-75e4778087c684a56bee7436b76fe463R41
[22:32:28]  <killercup>	sorry, didn't want to interrupt you
[22:32:32]  <matklad-hates-riot>	If you see you printfs only after the test passes (i.e., after one minute, and not at the moment they appear), you life is miserable :)
[22:33:00]  <matklad-hates-riot>	killercup: yep, this is the "output after the fact thing".
[22:33:44]  <killercup>	matklad-hates-riot: i don't think so, but it's underspecified, i'll comment on it
[22:34:17]  <matklad-hates-riot>	It says "output for failed tests", and to know the test have failed, you'll need to finish the test
[22:35:11]  <killercup>	matklad-hates-riot: yeah but it's an event, and not part of the test->failed document… anyway, i'll write a comment
[22:36:58]  <~nrc>	hmm
[22:37:24]  <~nrc>	seems like a hard thing to get right
[22:37:50]  <matklad-hates-riot>	I'm trying to record a video of how testing works in java to show what I want :)
[22:39:12]  <matklad-hates-riot>	https://youtu.be/08LbZKXsSXU
[22:39:29]  <matklad-hates-riot>	Ok, here's how the ideal user experience looks likes
[22:39:49]  <killercup>	matklad-hates-riot: thanks!
[22:40:03]  <matklad-hates-riot>	Does it make sense now?
[22:40:08]  <matklad-hates-riot>	>seems like a hard thing to get right
[22:40:09]  <killercup>	it does to me :)
[22:40:14]  <matklad-hates-riot>	for sure!
[22:40:31]  <matklad-hates-riot>	That's why the current behavior is as it is
[22:40:31]  <killercup>	i'm still writing that comment, i'll cc you in it
[22:40:56]  <matklad-hates-riot>	more specifically, `--nocapture` + parallel execution causes "after the fact" printing
[22:41:10]  <matklad-hates-riot>	`--nocapture` + single thread gives "live streaming"
[22:41:15]  <~nrc>	I think we should focus on the non --nocapture case for now, and make sure this works as part of the testing overhaul
[22:41:36]  <matklad-hates-riot>	So, actually, we need all three JSON + output streaming + parallel test execution
[22:41:51]  <matklad-hates-riot>	But the last two points actually contradict each other
[22:42:15]  <matklad-hates-riot>	b/c it's impossible to assign output to tests
[22:42:28]  <matklad-hates-riot>	I don't really know what could be done here =/
[22:43:02]  <oli_obk_>	matklad-hates-riot: why not? you could uuid all the tests and then just repeat with the additional output
[22:43:37]  <matklad-hates-riot>	If you have 10 tests running, and some thread prints hello, how'd you know which test is resposible for it?
[22:43:47]  <killercup>	matklad-hates-riot: it's impossible to assign output to tests <Pascal Hertleif it's not!
[22:43:57]  <killercup>	lol textexpander
[22:44:13]  <killercup>	matklad-hates-riot: each test is run in its own thread
[22:44:27]  <~nrc>	So, I'd like to stop here - are we happy with the proposal to merge the PR and close the RFC?
[22:44:33]  <oli_obk_>	jup
[22:44:46]  <~nrc>	I'd also like to propose that we continue next week and have a dedicated meeting about testing
[22:44:55]  <matklad-hates-riot>	Yep, but we need to specify at least a bit what happens with stdout =)
[22:44:57]  <killercup>	nrc: i have comments on the rfc it'd like to post, but we can postpone
[22:45:03]  <matklad-hates-riot>	like, in "unresolved questions" probably
[22:45:13]  <~nrc>	matklad-hates-riot: well, I would just close the RFC
[22:45:31]  <matklad-hates-riot>	killercup: what if the test itself spawns threads?
[22:45:36]  <~nrc>	and merge the PR assuming it doesn't work very well if you use `--nocapture` for now
[22:46:08]  <matklad-hates-riot>	nrc:  :thumbs_up:
[22:46:11]  <killercup>	matklad-hates-riot: then it would only work *right now* if it printed to the inherited stdout!
[22:46:50]  <matklad-hates-riot>	killercup: not sure I understand, but currently (with human output) this does not work.
[22:48:23]  <~nrc>	OK, then I declare this meeting over!
[22:48:35]  <~nrc>	Thanks all, and see you next week for more testing talk!
```
