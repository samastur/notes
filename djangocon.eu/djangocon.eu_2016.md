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



## Day 3

### Lightning talks

#### Django Under the Hood
* 3-6.11. in Amsterdam
* 1.5 days of technical talks and 2 days of sprints

#### Easy way to annotate your types, Dmitry Trofimov
* def greeting(name: str) -> str: (similar to Typescript)
* recent addition to PEP484 added very similar addition to Py2.7 with # type comment
* feature added to PyCharm for automated adding of type information to existing code
* missed switch required, but to work you need to run code in debugger for it to collect necessary information
* typed code then gives us code completion for typed variables

#### 6 steps to make beginning Tutorials more beginner friendly, Nicholle James
* don't ever use words like "easy", "fast" and "simple"!
* there is NO question that is stupid or boring!
* many beginners don't think to Google first, don't expect it of them
* don't assume a shared vocabulary
* break it down more than you think you need to (30 pages with 3 instructions better than 3 pages with 30; less intimidating)
* always have a backup plan (if you run tutorials, have thumb drive with software necessary)

#### My 2 (or 3) favourite things about Django, Daniel Rios
* 'self' for referencing foreign key to itself (and is documented)
* it's organised (and documented)
* it's got your back! (and documented)
	* password allocation warns you about bad passwords

#### Responsible DDoS, Akos Hochrein
* the swiss cheese model: each service is a layer of cheese (has bugs that let hazards through)
* they have a proxy relay that retries 4 times failed requests so one bad call killed 4 Django instances
* first issue: slow serializer using a lot of memory
* second issue: docker container had too little memory assigned
* third issue: broken monitoring

#### PyCon Namibia 2016, Daniele Procida
* happened from January 25th to 29th
* 118 attendees, 50% of whom were women
* 63 Namibian students including 7 high school students, 32 Django Girls
* most students attended for free because they couldn't afford it otherwise


### From Intern to Professional Developer: Advice on a Mid-Career Pivot, Rebecca Conley
* she made career transition recently (just turned 40)
* she had a passion for dance, but knew it wouldn't be a way to make a living
* became a teacher
* worked in Cape Town designing a small business accelerator; also on Haiti
* from Durham, North Carolina; town with economy that was based on tobacco which died out in early 80s
* got inspiration and was persuaded that she can become software developer
* so how?
	* option 1: teach yourself (good, but not efficient)
	* option 2: code school (job prospects after them are quite good)
* what I expected: screenshot from Silicon Valley show
	* a lot of barriers that she would encounter
* met Caktus Group people at conference who showed their work on app for registering for vote in Libyan elections who encouraged her to apply for internship
* what Cactus did:
	* give her mentor (Django Core developer)
	* worked on real client project, but they financed her time (invested a ton in her)
* learned how to ask questions and make the most out of opportunity
* today is a year since she was hired full time as professional developer
* she now tries to help and create opportunities for others
* why do we need Django developers who came from other careers?
	* we build software for people; people who come from different background come with lots of useful experiences
	* we benefit from diversity
* how can we support people making a career transition?
	* provide information
	* encouragement
	* training (running workshops, code school or funding training)
	* mentorship (code school is effectively paying for a mentor)
	* opportunity (companies need to be willing to hire people like her)
* code newbies on Twitter; they provide a lot of resources for newcomers
* http://whoisnicoleharris.com/connect/


### Beyond Web 2.0 - Django and Python in the modern web ecosystem, Russell Keith-Magee
* BeeWare: the IDEs of Python
* Quo vadiums? Does Django have future in this new world or will we all become Javascript developers?
* 2005: most websites were very simple affairs with light requirements reflected in Django's architecture (models, views, urls, forms and templates)
	* the most exotic thing may have been memcache
	* Google Maps was the exception, not the rule
* 2016: more exotic databases; expectations of UI have changed (code duplication); exposing APIs
	* increasingly we need to deliver content to native apps
	* user want live updates so we need channels...
* all sorts of potential for duplication
* this has led to development of rich client frameworks
* adding RCF doesn't fix any of the underlying problems, if anything it makes it worse
* this is the driving force behind Isomorphic Javascript Development so you use the same code on both sides
* this doesn't help you if your backend is Python
* why is Isomorphic Javascript Development being proposed? is it for isomorphic or Javascript?
* JS the only significant advantage is that is available in browser which is a huge advantage
* but extra tooling is already on the table because of native mobile apps
* everything old is new again
	* we tried to do isomorphism with Java in 90s
	* we learned then that MVC is a good way to handle that requirements
* API-first development
	* easier to test
	* improves API if this is what EVERYONE has to use
* high latency connections or no connection at all
	* you need to replicate part of the logic on client side
	* need Python on client
* Brython, Skuplt and PyPy.js are three ways of getting some subset of Python on client
* does it work? yes, BUT...
	* you need to deliver 500K interpreter first
	* also inefficient as it needs to interpret
* but do we need the full Python interpreter?
	* we just need to be able to run it; so why not ship unable version of that code?
* .pyc files and bytecode => Batavia is a JS implementation of Python VM (10K size for 100 py op codes)
	* can even access DOM
	* but is not fast, but we don't want to implement everything; mostly validation and maybe a bit of logic
* now we got a nicer separation
* on IOS you can access its API through Python because it's really just C underneath
* Android is more tricky and doesn't work well
* Russel has been working on VOC that converts Python to Java (missed bit of explanation :()
* but you still have separate codebase for IOS and Android, unless...
	* you use Kivy which doesn't use native buttons (app never looks like native)
	* or Toga where you get native experience, but not as mature (experimental)
* Toga for web?
	* he wrote a backend that targets web
	* freakboy3742.pythonanyware.com
* all this is possible right now, but isn't enough
	* unless it is easy and obvious solution, then these approaches won't get used
	* challenge is to build a mobile chat app in 15 minutes
* Django hasn't changed much in 10 years, but we are at risk of being boiled slowly like a frog if we don't evolve


### Let's Talk Geo: Adding the "Where" to Your Django Project, Corryn Smith
* works with GIS, analysing data on maps
* different presentations: lang/lot, UTM, State plans...
* where is important because it shows us things like accessibility or potential problems
* is map reading dead? geography is not taught anymore? (not really taught in USA anymore)
* she uses ArcMap - ESRI suite which also installs Python
	* can automate tasks with ArcPy
	* Python Scripting for ArcGIS book
* GEODJANGO = web framework to create geographic web applications
	* GeoDjango tutorial is great
	* can be overwhelmed by terms: Shapefile, SID...
* Checklist:
	* Spatial DB: need to be able to hold geometry (Postgresql with CREATE EXTENSION postgis;)
	* Geo data: Shapefile - geospatial vector file that are either point/lines or polygons
		* .shx (shape index format)
		* .shp (shape format/geometry)
		* .dbf (attribute data)
		* .prj (projection)
		* .sbn ... (find slides for links and missing bits)
	* QGIS alternative to expensive ArcMap
	* Geographic Model
		* your model should duplicate the fields in your shape file
	* importing geo data
	* Geographic Friendly Admin: from django.contrib.gis import admin
		* add data through admin only in emergency
	* Geo-friendly API
	* Mapbox (mapbox.com): adds the map to your project
	* Leaflet for customising map's look


### Best practices for scaling Django, Anton Pirker
* biggest project he worked on had 1M unique mostly users
* too many queries
	* should use select_related and prefetch_related
* separate db and web server
* next improvement: memcache (can check if it works with timestamp in generated page)
	* cache being busted because cookies are changing and Django assumes that change happened
	* can't just delete all cookies because it breaks sessions (delete non-Django ones)
* problem: bad gateway occasionally => hitting postgres connection limit
	* PgBouncer to the rescue (connection pooling)
* Celery + Redis for async tasks
	* Celery Flower for monitoring
	* have multiple queues for tasks of different importance
* next steps: putting elements on separate servers


### Don't be afraid of writing migrations, Markus Holtermann
* 3 recipes for writing migrations (when Django can't automatically)
* he'll publish repo with recipe examples that will grow over time
* class Migration(migrations.Migration) is required
* dependencies attribute lists which migrations need to be executed before this one can run
* Recipe 1: optimising makemigrations output
	* creating Book and Library models creates book, library and then foreign key from book to library
	* optimisation is to create Library first and then Book together with FKey
	* Django doesn't do it so because it sorts creation alphabetically and determinism
* Recipe 2: adding a non-null field (in multiple migrations)
	* create nullable field
	* populate field with data (probably from external source, but needs to cover **all** objects)
	* make field nullable and select option 2 (ignore for now)
* Recipe 3: rename an app without dependencies
	* you can't have incoming foreign keys to this app
	* rename table in migration
	* set db_table in model's Meta class to current value and create migration for it
	* apply with zero --fake
	* now we can rename files everywhere except db_table attribute
	* apply with plain --fake
* when renaming tables it (probably) doesn't rename constraints (or indexes?)


### Safe-Ish by Default: The Django Security Model and How to Make it Better, Philip James
* things Django does out of the box to keep us safe and what we could do to really lock things down
* XSS: yes, with template escaping (can get around with mark_safe(), |n, |safe)
* CSRF: yes, CSRF middleware (token)
	* CSRF cookie not set HTTP only! (so that AJAX can make correct request)
* SQLi: "Don't ever confuse code and data, it's the key to happiness" -- Alex Gaynor
	* yes, SQL statement and parameters are kept separate till the end
	* can get around with .extra() and RawSQL() so be careful about their use
* Clickjacking: protected with XFrameOptionsMiddleware
* Host Header Validation: ALLOWED_HOSTS necessary so that generated urls don't point to the wrong site
* Sessions: if you don't control subdomains, then your cookies may leak!
* Passwords: automatic algorithm upgrades (rehashes passwords of users on login if their stored is for old hash)
* how to improve
	* Constant Vigilance! (always check if exemptions are used)
	* HTTPS
	* CSP Reporting (Content Security Policy) - especially if you have content from 3rd parties
	* django_encrypted_fields (defrex/django_encrypted_fields) is great for storing data that needs to be encrypted
	* django-secure (slowly being subsumed by Django)
	* Pony Checkup (https://www.ponycheckup.com)
	* making Django ridiculously secure talk from djangocon 2015


### Using Django with service workers, Adrian Holovaty
* ServiceWorker is a proxy in browser you control (so you can massage requests and responses, store them...)
	* has fetch() API
	* can cache (separately from browser)
	* arbitrary JS
* once ServiceWorker loads, browser will send all following requests through it (but there can be those that didn't go before it was loaded)
* possibilities:
	* cache
	* preload assets
	* alter data depending on user/browser
	* loading JS framework and using it
* how to use with Django?
	* create offline page that is nicer
	* "cumulative cache" offline mode (as user visits pages you cache them so you can show them later)
	* "UI cache" offline mode (can request something for later usage)
* Problem 1: we're caching too much
	* probably caching empty base.html is a good approximation of what to cache (App Shell architecture)
	* on page load, make Ajax request for current URL with "from_appshell=1" so you return just the content, not the entire template
	* if you don't have quotes in template extend, then you are basically calling a variable and so you can replace base_template in render function call
* Problem 2: Cache invalidation
	* Cache-invalidation strategies:
		* static assets: use hashed URLs (CSS, JS, images)
		* App shell: remember last-updated time and periodically refresh
		* App shell: refresh it on every page load
		* Data: give users a way to clear the cache



### Lightning talks

#### GraphQL, Idan Gazid
* strongly-typed declarative query language
* you ask for data in the way you want it (you get in a shape you asked for)
* reduce number of network requests
* GET for queries and POST for everything else
* graphene-python.org for Python

#### PostgrSQL full text search
* no additional dependency
* strong integration with relational data
* in Django nearly landed (__search)
* performance not great, add functional indexes

#### async/await in 5 minutes, Milan Stanojevic
* coroutines - functions whose execution you can pause
* event loop added in Python 3.4
* async/await is not synonymous with asyncio


http://djangocon.eu/badge (recording of what is on cassette badge)