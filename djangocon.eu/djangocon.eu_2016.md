# djangocon.eu 2016

## Day 1

### The Front-End Revolution, Claudina Sarahe
* revolutionary is what replaces old order with new one
* rapidly changing web landscape
* revolutions happen when we adopt new behaviours, not new technology
* behaviour is a result of motivation, ability (can you do it: do you have enough time...) and trigger
* trigger can take many forms: alert, notifications
* successful trigger have 3 characteristics: we notice it, we associate it with target behaviour, has to happen when we are motivated and able to perform behaviour
* when behaviour doesn't occur, at least one element is missing (motivation, ability or trigger)
* Django succeeded because it was accessible (good documentation), easy to install, needed just a small will to try out
* one of important ingredients for change is community: encouraging & rewarding
* contributions are essential => modularity/plugins
* the rise of CSS preprocessor was inevitable because we needed a way to manage rising complexity of CSS
* Less became a successful competitor to Sass because it was easier to adopt, but Sass developers learned their lesson
* it succeeded because it prioritised the community and got it involved
* compass also made it easier to run and be useful
* sache.in: discover Sass & Compass extensions
* libSass was written in C++ by a person who was motivated enough to learn C++
* Extensible web movement	(#extendwebforward)
	* developers submit proposals openly in Github
	* prototype is built in Javascript
	* prototype is released and used and integrated upon
	* if there is enough interest, a proposal is made to w3c
	* developer and spec writer work from live prototype
	* developers build polyfills


### Learning Django, Learning French, Nicole Harris
* celebrate late adopters
* linguistic researchers have found that under controlled conditions adults are better at language learning (because they can use other knowledge to improve)
* seeing other adult people who learn language inspires you/makes you believe you can do it well
* need to celebrate people who start using Django later in life and/or have no CS degrees (come from other fields)
* motivation barriers:
	* largest learning jumps happened when she felt she got something back from learning (with french when she could have some basic conversation)
* got stuck first with timezones and deploying apps; needed someone to walk her through it
* facilitating mentorship is therefore important
* husband mentor helped her with (learning french):
	* answer difficult questions
	* correct mistakes
	* suggest improvements
* In Django would be great to have someone who would:
	* answer difficult questions
	* review code
	* develop learning plans
* working on application that would help connect mentors and mentees
* immerse (new learners) => let's get comfortable with the idea of implementing first and understanding later
* problem is that we write documentation with a view that understanding comes first; Nicole would like to see more code examples
* boost confidence: for years she was really shy and embarrassed to talk in french; she made a resolution to stop carrying which led to her biggest improvement
* we have problem in tech community that people are afraid to ask questions because they don't want to appear stupid
	* we should celebrate failure; encourage learners to make mistakes
	* How to Make Mistakes in Python by Mike Pirnat (great book)
	* discard old myths about good and bad programmers
	* inspire participations (PyBee and PyBeeWare; check how they do it)


### Django and ReactJS: The good, the bad and the ugly, Tomáš Ehrlich
* a way to think of it as a template engine: renders data and handles events
* handling changes can be difficult, but re-rendering everything would be slow
* ReactJS does away with this by introducing virtual DOM on which it calculates the minimum set of changes it needs to make and then does only those
* diff is fast; DOM speed is always a bottleneck (me: not sure this is actually still true)
* in Django you write template top to bottom; in ReactJS components are written from bottom up (first create components and then use them together); existing stuff (e.g. Bootstrap components) can be wrapped as ReactJS components
* advantages: separation of concerns, composable and testable
* how to use it:
	* Facade: take a Django app and add React frontend (easy, but not DRY as you get to write templates on backend and front-end)
	* Full-stack React: replace Views and Templates of Django with React; use Django only for API
		* disadvantages: needs to replace existing code, more complex, loses Django admin...
* Django by default is pretty safe; this is less so in JS world (ExpressJS requires more setup)
* Django in React
	* Django best practices (and good design decisions)
	* opinionated framework (good for reusable apps)
	* nothing like that in ReactJS (just many of starter kits); best practices varies and evolve very quickly
	* missing Django-like React framework


### Rub-a-Dub Rubber Duck: Don’t Be Afraid to Debug!, Anna Ossowski
* there is always a way to make things work again (in software!)
* use (read) error messages
* logical errors more difficult to understand because there's a problem with logic (wrong variable name, indenting at the wrong level...)
* Rubber Duck debugging (explain problem to the rubber duck)
* easier to debug smaller chunks; test code as you write it and keep parts small enough (short functions)
* list of tools to help you debug (pub, pyflakes, pylint, pychecker...)
* you are **not** the code you write!


### To mock, or not to mock, that is the question, Ana Balica
* Mocks simulate the look and behaviour of real objects
* Mocks != stubs; stubs are objects that give us canned responses for the task
* unittest.mock (py3); mock (py2)
* MagickMock has default implementation for most of __methods__
* Mock the object where it is used, not where it came from
* Streams: mock stdin/stdout by replacing sys.stdout with StringIO object
* Good mocks: system calls, unpredictable results, streams, clocks, timezones...
* Bad mocks: as mocks are greedy, they will respond even to calls they shouldn't
* use call_count instead of assert_called_once_with as it is less error prone
* TDD helps as tests should fail first
* provide functional tests to check that all layers still work (each of them can pass, but together they may still fail to cooperate)
* Question from Russel: can need for a mock be a code smell?


### Going with the flow with Django Admin, Marysia Lowas-Rzechonek
* the admin has many hooks for customisation already
* example on how to use admin infrastructure (existing templates, admin models...) to implement review flow app


### How to Upgrade to the Newest and Shiniest Django, Susan Tan
* why upgrade? features, bug fixes, security fixes AND easier to upgrade in future
* right time to upgrade?
	* old version not supported anymore
	* a patch release has been created
	* have a comprehensive test suite!
* why is upgrading a Django project a difficult thing to do?
	* hard to know how much time or effort it takes?
	* hard to know which tests will break or even run?
	* hard to know if UI will continue working
* Do not skip versions when you do upgrade:
	* miss on deprecation warnings (supported for 2 versions)
	* have invalid code (call APIs or use modules that don't exist in newer Django version)
* What is step 1: run tests
	* may not even run, tests may fail or tests pass (suspicious?)
	* fix tests one at a time
	* once all pass, check UI; then edit deploy script, requirements.txt and documentation
* read release notes
* checklists help as does taking notes


### Lightning talks

#### Heroku deployments without Git, Markus Zapke-Gruendemann
* usual git push heroku
* what if you want to deploy python package?
* build own heroku slug? sounds reasonable, but a lot of work
* he built heroku-slugify (users heroku sources and builds API endpoints)
* example: deploy Jupyter notebook

#### Transaction Isolation Level in  Wonderland, Shai Berger
* transactions are one of these things we just assume
* standard SQL isolation levels (defined by standard) in rising isolation order:
	* READ UNCOMMITTED
	* RED COMMITTED (default for Postgres, Oracle)
	* REPEATABLE READ (default for MySQL)
	* SERIALIZABLE
* defined by anomalies: dirty reads can happen only in UNCOMMITTED, non-repeatable reads only in COMMITTED
* none should happen in serialisable (but do)
* Postgresql SERIALISABLE is the only one that actually is
* Django will start using READ COMMITTED on MySQL by default

#### Kinto - a backend for web apps RectJS, Remy Hubseher
* what if we could deploy an instance and use an existing client to configure and consume the API
* KINTO - a lightweight JSON storage service
* sync, permissions, push notifications, ops friendly, encryption, ReactJS admin
* next: plug this protocol with Django, use Django users to authenticate, Django forms to validate data
* Mozilla's work

#### 4 truths and 1 lie about Aussie Animals, Markus Holtermann
* Drop bears don't exist
* Wombats poop cubes
* Koala pouches are upside down, so are wombat pouches but that makes sense
* Pyromania - falcon that starts bush fires as part of its hunting process
* Australian sheep have learned to commando-roll over road rails (cows not yet)
* Emus - army once declared war on 20000 aggressive emus; emus won
* Which one is a lie?
* DjangoCon AU in August!

#### Act as Auth Backend for better Customer Support, Peter Zsepi?
* pip install djactasauth
* same idea as my Impostor :)

#### Hat Rack, Katie McLaughlin
* we don't have any good metrics system for work that we do that is not code
* http://bit.do/LABHR (let's all build a rat hack)
* send to people a list of cool things they have done (better if you also publish it on Twitter)
* you can make people really happy by thanking people for doing awesome things
* github.com/LABHR/octohatrack (helps tracking these contributions)
* http://labhr.github.com/hatrack