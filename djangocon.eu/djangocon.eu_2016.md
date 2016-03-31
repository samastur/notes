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

#### Kinto - a backend for web apps ReactJS, Remy Hubseher
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
* Australian sheep have learned to commando-roll over road rails (cows not yet) - NOT TRUE
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



## Day 2

### Keynote: The Art of Programming, Erika Heidi
* excelled at programming courses, but failed maths
* is programming science or art?
* art: the expression of human creative skill and imagination, generating an output that can be possibly experienced by someone other than you (Erika's definition)
* art: not an adjective; doesn't need to be good, beautiful
* Margaret Hamilton began using software engineering to distinguish it from hardware; wasn't taken seriously for a long time
* programming language are building blocks
* programming is both art and science
* community drives adoption
* "if you want to go fast, go alone. if you want to go far, go in company"


### Introducing Django To The Foreign World, Bashar Al-Abdulhadi
* Kuwaiti open-source enthusiast
* we have people from 40+ countries speaking 30+ languages
* we have 3 different language directions among them: LTR, RTL and Top-Bottom (LTR on web)
* Arabish = English + Arabic (Arabic words using English alphabet): used at the beginning to chat on IRC
* they use numbers in place of some characters; also abbreviations like Q8 for Kuwait
* Kh represented with 5 because 5 in Arabic starts with Kh
* startups, Google, Microsoft etc. started to adopt Arabish as an input method when Arabic keyboard didn't exist yet
* challenges in transformation of English & Arabish to Arabic
	* fonts (joining letters wasn't supported in early days)
	* Arabic support in browsers (used images instead of text)
	* databases: no standards to follow for encodings
	* Python also had problems with connected letters (shows screenshot of a browser where content renders fine, but menus don't)
* Initiatives: ArabEyes.org (non-profit group for translating applications)
	* have technical dictionary to standardise terminology
* Standards (like unicode)
* django-parler presented as lightning talk in DjangoCon.eu 2014; got him more involved in project
* he started to translating and coordinating translation of more and more apps
* moving forward: collaborative translation
	* add your package to collaborative translation sites (you never know who will be interested)
	* support your native language on other packages
	* a little continuous contribution is better than discontinuous bulk one


### IoT with Django: From hackathon to production, Anna Schneider
* she'll discuss IoT where vendors already provide web APIs
* we'll look at patterns and anti-patterns of using Django for IoT projects
* Models
	* we want to be storing vendor ID number (vendor's unique ID; whatever data you need to send to their API)
	* on observation model you want to store timestamp (there will more of them than objects in usual apps)
* Tasks (outside of request/response cycle)
	* kinds: monitor and control; generalisable and custom
	* pass pas (pro: don't rely on database state; con: can require multiple calls)
	* encapsulate client! (easily mockable/testable)
* Celery!
	* Django celery (djcelery) creates more problems than it solves
* github: aschn/cookiecutter-django-iot.git (cookiecutter template)
* Lessons:
	* data model: Device, Attribute, Status
	* tasks not views: outside request/response
	* easy but rigid: Heroku Scheduler
	* flexible but complex: celery periodic tasks


### The Power ⚡️ and Responsibility 😓 of Unicode Adoption ✨, Katie McLaughlin
* 7 bits enough to encode English; 8 bits for western countries
* Unicode create to solve problem of other languages (including dead)
* allows for about 1 million of code points and we are using about 10% now
* UTF-32 can encode everything, but can waste a lot of space for certain languages; so we like UTF-8 more
* what about emoji? started in Japanese telcos without standardised codes
* Unicode7 in 2014 and Unicode7 in 2015 added even more emoji symbols
* Android's emoji's are shit; MS clapping hands are too
* you know what? all have problems
* 5 things that gets emoji included:
	* is there compatibility issue?
	* is it frequently used?
	* distinctiveness
	* completeness
	* frequency of requests
* whisky will get added! :)
* platforms use different way to mark them (:cake: on slack or (cake) on hip chat)
* never rely on user agent to be able to handle emojis; use alt-text for code point
* proposal to include special chars that will flip the direction of emoji display
* use Twitter or Emoji1 set (both open and Twitter is also widely used)


### A Brief History of Channels, Andrew Godwin
* goals:
	* hard to deadlock
	* built-in authorisation
	* widely deployable
	* scales down
	* optional
* Django as it stands is not built for this (async calls)
* channels: instead of requests we have events
	* events of the same type are grouped on a named channel
* instead of writing a code against request you now write for channel
* a channel is a **named, FIFO, at-most-once, non-broadcast, network-transparent** queue of messages
* Protocol Server and Worker Server are now separate
* you can send onto channels from anywhere (model.save, view...)
* responses are on single-reader channels (http.response!1A2B3C)
* how do you use it?
	* Views are "replaced" with Consumer that takes an event and send zero or more events
	* you can't wait on an event (=>avoids deadlocks)
	* every message on a channel runs the consumer function
	* http.request routes to a view system consumer
* channel is low level; not end-to-end solution
* routing.py: urls.py for Channels
* message.user: like request.user
* message.channel_session: per-socket sessions (because there's no built-in shared state)
* Groups: broadcast/pub-sub
* backend is pluggable and has a specification so you can replace it
* github: andrewgodwin/channels-examples (heavily annotated)
* pip install channels (Django 1.8 and 1.9); docs were written first so should be pretty complete


### Hermione Granger and the Wizard Information System, Lacey Williams Henschel
* talk about how things we do impact our lives
* HP protagonists are really research oriented
* lots of comparisons with HP and symbolism that is completely lost on me; no real new insights :( 


### Building A Non-Relational Backend For The ORM, Adam Alton
* github: potatolondon/djangae
* Potato builds stuff for Google and get lots of users from them
* Datastore build around idea that whatever you're building is probably gmail :)
	* every query must use an index
		* no table scans
		* max 1 inequality filter per query
		* no JOIN queries
		* OR queries are multiple simple queries in parallel
	* indexes built and replicated asynchronously
		* eventual consistency aka "where did my data go"
		* no unique constraints except PK
	* updating every row could take days
		* no schema (because adding column is slow)
		* transactions must query by PK
	* random auto IDs (not auto-increment; auto-random)
	* counting is slow
* For django that means no ManyToMAnyField => no permissions, unique or unique_togther
* no GenericForeignKey or concrete model inheritance
* transactions are out as well as is Paginator (because wants to know how many pages there are)
* Building a DB backend
	* DatabaseFeatures class tells what DB can and cannot do
	* ....
* 15095 lines of code (2422 commits from 45 contributors)
* normalising WHERE clauses
	* WHERE (a=1 or b=2) AND c=3 => WHERE (a=1 and c=3) OR (b=2 AND c=3)
* rebuilt ManyToManyField, Permissions, unique=True and most missing parts including admin (but not migrations)
* still need migrations even on schema-less db because of queries
	* they can take days


### HTTP/2 : why upgrading the web?, Quentin ADAM
* why new version? we need to be faster, faster, faster
* HTTP/2 is a binary protocol
* we should use it now! (better notes on benefits in my other notes)
* url cannot change => two ways to manage for http and https
	* how to negotiate a protocol upgrade
	* http/1 build in method: upgrade header (still slow; needs 2 connections)
	* we also need to encrypt the web (TLS)
	* with HTTP/2 we use NON on client and ALPN on server
* Python has a library called Hyper (1.0 release in Oct. 2015) - http://python-hyper.org
* WSGI incompatible with HTTP/2? (need to switch from request/response model to channels)
* use Wireshark!
* clever-cloud.com gift coupon code: DJANGOCON16


### Django Microservices Made Easy, Paul Hallett
* moved to micro services because of hosts of problems, also Django ones (making migrations separately on same app)
* 1st problem: shared database
	* solution: always isolate dependencies as early as you can
* 2nd problem: all new services were built differently (include things service discovery, error logging...) - inconsistent file structures
	* solution: project templates using cookiecutter
* 3rd problem: inconsistent HTTP interfaces (some very lovely REST, some less so), inconsistent integrations
	* solution: education & guidelines!
	* http://bit.ly/api-best-practices (guide Lyst wrote)
	* second part of solution: client templates
* final problem: consistent deployment (so any member of the team can deploy them)
	* solution: Empire (open source platform that works on Amazon EC2 with heroku like interface)
	* bit.ly/empire-pass
* consistency is the key
* Lyst got huge boost in productivity, tiny outside dependencies, increased team autonomy
* where next?
	* Platform
	* Auto-generation (swagger? not good fit)
	* we now have a lot of communication overhead (need to talk to developers of the service); trying to solve this with Protocol Buffers
* Top questions (that you are doing things correctly)
	* in depth: http://bit.ly/micro-10-q


### Lightning talks

#### Low-level love with NAND2Tetris, Nicolas Noe
* it is important and fun to look at inner workings of abstractions we use
* discovered NAND2TETRIS: learn how to build from scratch a complete computer (basic, but complete)
* 12 weeks you build something more complex on top of what you built previous week
* you use simulator for building hardware
* http://nand2tetris.org

#### Django REST framework - what's next?, Tom Christie
* REST framework 4: client libraries & realtime
* Client libs:
	* dynamic client libraries
	* either schema-driven or hyperdermia-driven
	* Python & Javascript libraries
	* command line client
	* schema generation & hypermedia support
* Realtime:
	* documentation for using REST framework with Django channels
	* client libraries support for WebSocket APIs
* Sponsorships
	* monthly paid plans & range of sponsor benefits
	* collaboratively funded development - far greater return on investment than internal development
* Beyond Django
	* working on a cross-framework, cross-language, API framework
	* work with Django when appropriate, switch to Flask, Falcon, Node when required
* http://coreapi.org

#### DjangoGirls Summit, Dalege Lucie
* patreon.com/djangogirls (seeking funds) and paypal donations

#### In-san-i-ty, Michal Lowas-Rzechonek
* HTML5 is X11
* WebSockets is TCP
* Grunt is Make
* NPM is APT
* SASS is M4

#### What's the time, Joachim Jablon
* all time rules have special cases which might have special cases of their own
* Australia has crazy timezones (6 moving separately)
* TZ Data and Pytz
* naive datetime does NOT exist
* use UTC everywhere except to human beings
