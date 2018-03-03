# .concat() 2018 (Salzburg, 3.3.2018)

## When Fast is Faster than Fastest, Daniel Clifford

* between 2007 and 2012 we used mostly microbenchmarks for testing (useful at
  time, but not anymore)
* in 2012 benchmark Octane was released trying to measure a wider spectre of
  usage
* when measuring real web page runtime breakdowns they discovered that
  relatively little time is spent running Javascript while Octane was measuring
  almost only that
* now try to test more with real web pages
* important lesson: realizing that optimization will lead to optimizing for
  specific goal which might lead to worse behavior for others
	* goal to have consistent baseline performance
* how V8 works
	* V8's ignition interpreter and TurboFan optimizing compiler
	* Javascript is dynamic which makes it difficult to optimize for
* now can optimize the whole JS (not before; try-catch used to be a problem)
* getting feedback for specific optimizations
	* use mechanism of inline caches are used in V8 to make operations faster
	  and record type feedback
	* once bytecode is generated they also create a feedback vector for
	  collecting information when code runs
* if you use same method for structures of different types (e.g. array of
  numbers and array of strings), it can lead to polymorphic handling under the
  hood and making running it slower
* how about using builtin for summing (using reduce)?
	* same benchmark on Chrome up to 58; worse for integers (4.7x slower)
	* builtin reduce is written as Javascript that is more complex than
	  straight sum function (self-hosted)
	* hence need to optimize for them
* building a better builtin: The CodeStubAssembler
* Why not C++? 
	* ABI differences
	* deferred code
		* make checks to select between different paths (fast, slow); hard to
		  do in C++
	* GC handling of pointer types
	* tail calls
* How about assembly?
	* for a while they did this, but turned in a mess after a while
* CodeStubAssembler generates using TurboFan optimized assembly for each
  platform
* new results: consistent, but numbers slower than at the beginning (strings
  much faster)
	* reason: overhead of switching between functions (more expensive than work
	  inside)
* fixed by: inlines functions passed to reduce if they meet certain criteria
* Chrome 66: much faster across the board, but not completely consistent
* Take aways:
	* Design minimally and specifically
	* write idiomatic Javascript
	* use new features which allow you to express intent using JS builtins
	* measure and tune carefully (and chose the right benchmarks)


## Your algorithm isn't neutral: exclusionary UX and how to avoid it, Ivana McConnell

* inclusive is often not well defined (inclusive UX)
* inclusion needs to happen before user can try to complete the task
* inclusive definitions are always opositional ones: not to exclude X
* software creates communities: it's just common ownership and the best ones
  foster togetherness
* I am X user instead of I use X means we identify partly with it
	* hence code and design are critical becomes they enable these interactions
* algorithms are rulesets design by people (hence NOT neutral)
* exclusion: what do these experiences look like
	* Apple's Healthkit: cannot track reproductive health
	* Pure Gym UK: did not allow a woman Dr. to women change rooms
	* voice interfaces are useless for those who can't speak, have accent...
* exclusion is a decision: someone didn't see a problem; didn't stop or did  go
  ahead
* who *should* your product empower?
	* who does it empower? does it match up?
* Inclusive UX: how do we approach it?
	* not easy and don't expect it to be
* Bias: we all have it and need to address it
	* with inclusion is generally is not defined for each project
* Empathy has limits
* You need to clearly understand the problem and if you are the right person to
  be working on it (or making decisions)?
* Appoint the right decision-makers
	* need to involve other people; the ones that are affected
* ...and involve them from the start
	* if you get problem definition wrong, then what are you building?
* ...and trust them. Listen.
	* and respond in a meaningful way (even if it does not feel right to you)
* Build intentionally (NOT move fast and break things)
	* moving away from defaults
* Accept mistakes; it's how you react that matters!
* Inclusive != universal != neutral (it just has to include those for whom the
  solution is helpful)
* Inclusive UX: what success looks like?
	* Nextdoor: slow down when problematic posts are being written
	* Mobility Map
	* Wayfindr
* small choices matter; make them whenever you can (e.g. Add to Slack icon with
  darkskinned hand)
* Don't ask for what you don't need (gender, race, title...)


## When Old Meets New: Codebases, Ursula Sarracini

* how to write a modern web app in legacy codebase
* works on new tab page written with new web technologies they want to put into
  Firefox codebase
* Firefox is old and web moves fast; need to update technologies to stay
  relevant
* FF runs in a sandbox: 2 context
	* main process (parent process) can do privileged actions
	* content process which renders content but cannot do privileged actions
	* lots of code separation
* Shared code first challenge encountered
	* FF has many ways to import modules
	* but no way to import the same module in two different contexts
	* copy code? write a module loader?
	* used Babel to convert JSM to commonJS + webpack so we can import them in
	  both contexts
	* keep in mind: be ready for a mix of architectural patterns
* Multi-process Javascript
	* architect app using async messages passing, a common store between
	  process
	* FF has a builtin IOC
	* our own action dispatcher to attach Redux with the FF message manager
	* wrap existing code in new code to make it work
* Testing frameworks:
	* wnated to use Karma + Mocha + Chai
	* does not exist in FF - use old framework called Mochitests
	* Writing tests internally isn't enough - fix the existing tests in FF
	* spent a lot more time than expected to fix legacy tests
' Mocking
	* unit tests are obviously very important for your app
	* external dependencies in legacy codebase, affect the unit tests
	* mock out the external APIs that wouldn't be available int he context of
	  our unit tests
* Source control & issue tracking
	* not wanted to use hg for lots of bad reasons
	* their flow:
		* write git patch in our repo -> periodically node script copies over
		  files to FF repo -> use hg to write a patch -> push to FF
	* mirror bugs from Bugzilla to Github
		* important not to forget the way people are the most comfortable
		  filing issues
* Human & cultural
	* people didn't want bundles in the Firefox tree - but we needed React +
	  Redux to exist in the repo
	* had to commit the files to the tree (not ideal, but a compromise)
	* we had to write more JSMs to fit FF code style
	* out app wanted to be modulear - lots of small independent modules but...
	* FF gives lots of overhead to loading modules so we had to compromise
* Release schedule was a big issue (FF takes 8 weeks to ship an update) and
  they have 2 week milestones
  * lots of A/B tests, style updates, bug fixes, feature updates
  * can't afford to wait 8 weeks until people see updates
  * compromise: ship as an add-on to the browser and get a different release
	cycle
* Communication
	* lots of developers are set in their ways about the FF code
	* change our strategy - be less isolated - write code that integrates with
	  the existing code
	* join meetings, talk to engineers so they know what's coming, know who to
	  talk to about the legacy code
	* lots of conversations about data collection
* Know when to make the right compromises
* Be open about the changes the product is making to the legacy code
* Don't be afraid to introduce new tech & modernize the old code


## Let's save the internet: How to make browsers compatible with the web, Ola Gasidlo
* https://webcompat.com
* project for bug reporting for the web
* project then takes over of notifying websites, browsers


## Build bridges, not walls – Design for users across cultures, Jenny Shen

* what is culture? Dimensions:
	* power distance
	* individualism
	* masculinity
	* uncertainty avoidance
	* long term orientation
	* indulgence
* Users were not filling in passport data (Netherlands)
	* added warning to form for names to match, but warning wasn't effective
	* more effective to charge for mistakes and notify them
* research local UI patterns (hamburger menu not used in China much; they use
  compass/discover menu item)
* start designs with longest translations first (or most complicated)
* localize copy means also localizing idioms (for example 1 is free)


## Thinking PRPL, Houssein Djirdeh

## Fast. Simple. Accessible., Estelle Weyl

## The Strategy Guide to CSS Custom Properties, Mike Riethmuller

## Remote device sign-in – Authenticating without a keyboard, Tiffany Conroy

## Styled Components, Max Stoiber

## Lighting talks

## Unlocking the power of SVG — SVGs beyond icons and illustrations, Sara Souiedan
