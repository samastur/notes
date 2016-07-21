## EuroPython 2016 - Tuesday, 19.7.

### Inside the Hat: Python @ Walt Disney Animation Studios - Paul Hildebrandt
* on Zootopia they did a stress test if they can render everything by rendering 7 million trees (much more then needed)
* for different markets they don't replace just signs in movies, they might even replace/remove characters


### High Performance Networking in Python - Yury Selivanov
* why async/await is the answer
	* dedicated syntax; concise and readable
	* new builtin type for coroutines
	* new concepts: async for and async with
	* generic, framework agnostic design
	* fast: only ~2x slower than a function call
* couroutines are subtype of generators; shares **a lot** of code
	* await -> yield from -> YIELD_FROM opcode (same opcode)
* asyncio - developed and actively maintained by the BDFL
	* a toolbox for frameworks and protocols
	* part of standard library (slower releases but guaranteed support)
	* inspired by ("copied from") Twisted
* asyncio: what's inside:
	* standardized and pluggable event loop
	* interfaces for protocols and transports
	* factories for servers and connections; streams
	* futures and tasks: callbacks + coroutines, timeouts, cancellation, etc.
	* subprocess, queues, synchronisation primitives
* event loop is the foundation of asyncio
	* factories for tasks and futures
	* IO multiplexor
	* low level API for times events, schedulling callbacks, subprocesses and signals
	* and **you can replace it**
* gh:magicstack/uvloop
	* 99.(9)% compatible asyncio event loop written in Cython
	* uses libuv under the hood
	* fast tasks and futures = faster async/await
	* super fast IO (2-4x faster than asyncio)
* uvloop faster than even nodejs and almost as fast as golang
	* just use uvloop! :)
* what to use: low level sock_* or higher level constructs
* downsides:
	* low level loop.sock_* cannot buffer data for you and no flow control
	* streams: easy to use, but too generic
	* protocols & transports: say hello to callbacks
* which API to use:
	* stick to streams for implementing protocols
	* use loop.sock_* for easy porting sync code to asyncio. better consider streams
	* use protocols and transports for performance (just for drivers; high level code should not think about them)
* Protocols
	* loop pushes data to Protocols
	* Protocols send data back using Transports
	* Protocols can implement specialized read and write buffers
	* Protocols can do flow-control
	* use:
		* strategy 1: develop custom streaming abstraction tailored for the concrete protocol and use async/await (example aiohttp)
		* strategy 2: implement protocol parsin and logic using callbacks; hide that under an **easy to use** async/await facade (can be faster as you can use C)
* gh:magicstack/asyncpg - the fastest PostgreSQL driver for asyncio which uses binary data format
	* more efficient encoding of data, less traffic and faster parsing
	* **not all**  PG types can be safely encoded as text
* asyncpg
	* ditches DB API
	* idea: let's build an efficient driver for Postgres. Use its features to the max
	* uses prepared statements (as PG loves them) and caches them to improve speed
	* crazy fast compared to others (including nodejs and golang)
* Use Cython; it's amazing (easier than C and fast)
* use loop.create_future() instead of async.Future (added in 3.5.2 and 30% faster)
* always profile and benchmark
	* Cython code can be profiled with valgrind and visualized with KCachegrind
	* use `cython -a` option to generate HTML output for your Cython code
	* implement custom buffer classes
	* working with Python bytes is expensive; prefer C types
	* have efficient buffer to build requests and one buffer with transport.write. Or use multiple buffers...
* use TCP_NODELAY to send data as soon as you call transport.write
* use TCP_CORK option if you have no control over how transport.write is called (buffer writes and send as few TCP packets as posible)
* implement timeouts as part of your APIS

### The Joy of Simulation: for Fun and Profit - Vincent Warmerdam
* inverse turning test: if you can generate random numbers, then you are not human
* human entropy is terrible which is why we prefer to use computer to help us think about probability (math works, but is harder)
* inference via sampling
	* being able to sample allows us to not have to resort to maths
	* sometimes we know the characteristics of a system but we'd like to know the likelihood of a certain events happening
* PyMC3 or emcee libraries for sampling (tutorial on sampling algos on his blog: koaning.io)
* Monopoly
	* spike for jail (likelihood of ending there), but that makes next areas are because of this more likely for you to land
	* chart: rent income vs probability of landing (size of point is expected value)
	* we can understand game better by using sampling
* let's consider something profitable: lego minifigures!
	* they make 16 figures sets for 2 months and then never again
	* there is a market for them
	* a minifigure costs about 3 euro a piece; we can seel for 100 euros later
	* how likely it is we'll get a full set
	* sampled to get likelihood of geting full set depending on number of packets bought
	* the more legos he buys, higher likelihood to get more sets (because of reuse of spares)
* general usecases: optimisation!
	* silly example: finding the largest triangle in a 1x1 square
		* make random triangles in square: not the best way
		* improvement: keep bigger triangles and look at distribution of points to see which areas for points perform well
		* how about repeating this (sampling sample): until some form of convergence (genetic algos work in similar way)
* generative methods (...to outsource creativity)
	* recruiters cannot distinguish a pokemon name vs. a name of a technology
	* creating a library generating names that sound like pokemon (grabble)
	* problem: composing believable set of token (using markov chains)
	* most of results are not good, but some are interesting
* another solution: add judges (add models that judge results of a model)
* sampling can be fun and profitable; easy to start with and surprising how often can help out


### Effective Code Review - Dougal Matthews
* the only reason not to do them is if you are the only developer
* average effectiveness of code inspections is 55-60% (25% for unit) if believed
* goals (expectation vs. outcome)
	* while finding defects remain main motivation, they provide also knowledge transfer, increased team awareness and creation of alternative solutions to problems
* often negative view of reviews because of view it is author vs. reviewers
* code review bad name; code discussion or code collaboration would be better
* authoring changes
	* important to talk about what you want to do first (motivation for code)
	* adhere to project guidelines (write test, documentation, follow style guide and test the relevant platforms)
	* provide context with change to review (good commit messages, good PR descriptions)
	* small & contained
		*  code review: 10 LOC - 9 issues, 500 LOC - looks fine (@mikhailgarber)
		*  larger patches lead to a higher likelihood of reviewers missing some bugs
	*  opening a review is a start of the conversation (don't ask for it to be merged, ask for it to be reviewed)
	*  relinquish ownership
* better: shared responsibility
* important that everyone reviews: junions, seniors
	* review to learn, verify and teach. Not necessarily in that order
* keep reviewers on the same page: if the are all reviewing to different rules, it will never make sense
	* can make clearer with review guidelines (be careful with checklist)
	* automation: code style, tests run (better for egos and saving time)
	* remove the bikeshed (ignore style comments not tested automatically)
* multiple reviewers (try if possible; in openstack minimum of 2)
* important to feel fresh for reviews => frequent short reviews (reviews peak at around 200LOC/h)
* constructive criticism and praise (it's easy to just point out the bad things, but when somebody teaches you something, let them know)
	* be polite and aware of tone; some things come across overly negative
		* why didn't you do...? 
		* replace with: "Could we do this...?"
		* ask questions instead of telling
		* never be harsh and never personal
* **careful** reviewing can be a good way of starting working on a new (open source) project
* tooling not so interesting (GitHub, Gerrit, Review Board, Phabricator)
	* review **before** the merge (otherwise updates don't get done)
* Github: loose workflow, labels are useful and simple UI
	* some things difficult (multiple reviewers)
* Gerrit: very defined, multiple reviewers (kind of opposite of Github)
* code review data


### Writing faster Python - Sebastian Witowski
* Rules of optimisations (3)
	* Don't
		* don't optimise if you don't have good reasons for it
	* Don't yet
		* finish your code first
		* have tests
		* now
	* Profile (don't guess)
		* cProfile
		* pstats
		* RunSnakeRun, SnakeViz
* Levels of optimisation
	* design
	* algorithms and data structures
	* source code
	* build level (build flags)
	* compile level
	* runtime level
* optimisation is about speed
	* ...and memory...and disk space...disk i/o...
	* you have to decide what is crucial
* writing faster python == source code optimisation
	* first check if there is a builtin function that does the job
	* check collections for data types if standard are not good enough
	* if you need list, then list comprehension is faster than filter iterator
* permission or forgiveness?
	* if hasattr is slower than try:except and difference becomes larger if need to check multiple attributes (as long as it is very likely that attribute is there; exception is expensive)
	* depends on case (measure!)
* membership testing
	* in statement is faster than loop (speed depends on where in list the element is)
	* sets are faster, but converting takes time (100 or so ms)
* remove duplicates: fastest by changing to set or by OrderDict if order important
* list sorting: .sort() much faster than sorted()
* avoid function calls (inline if you can)
* checking for True/False: fastest by using (not) variable (== or is is slower)
	* same for list
* def vs lambda: use def (same performance)
* list() or []: literal is faster (same for dict)
* variables lookup is faster for local variables (true also for saving references to object methods)
* source code optimisations add up and are cheap, but write idiomatic Python and don't reinvent the wheel
* PROFILE your code and be curious!


### Conda: Easier Installs and Simpler Builds - Mike Mueller
* is
	* an installer similar to pip
	* an environment manager similar to virtualenv
	* cross-platform and not limited to Python
* Miniconda: small bootstrap-like version that includes Python and `conda`
	* provides access to many libraries
* Anaconda: large distribution of Python packages with focus on scientific applications (200 or so)
	* needs about 2GB of disk space
* Channels:
	* locations of packages
	* default == Anaconda server
	* conda-forge
	* private channels
	* install -c my_channel package_name
* basic tasks:
	* install packages
	* create and administer environments
	* create packages
* search: conda search pandas (shows a list of matching packages with  versions and channels where found)
	* conda search --full-name pandas (displays only those named pandas)
	* conda search --platform win-32 --spec pandas=0.18.1
* install: conda install pandas (shows information and asks if to proceed)
* create an environment: conda create -n mypy35 python=3.5 (-n name)
	* activate it with: source activate mypy35
	* list envs: conda env list
	* conda list (list all installed packages in env)
* building a package:
	* from a package on PyPI
	* from scratch
* build from PyPI:
		* conda install conda-build
		* conda skeleton pypi mypackage
		* conda build mypackage (result is a tarball)
		* conda install --use-local mypackage (or specify a full path)
	* build can specify a python version
	* convert to other platforms: conda convert --platform all ..package.tar.bz2 -o outputdir/
	* upload to anaconda cloud: 
		* conda install anaconda-client
		* anaconda upload ...path_to_package
* build from scratch:
	* need:
		* meta.yaml (what you want to do)
		* build.sh - linux and Mac
		* setup.py (just as with pip)
	* ...
* conda is great and works together with pip
* benefit: you can package things from different languages (e.g. jar files with python)
* conda envs include python so upgrading system one shouldn't break them (but not upgrade them either when system does)


### Exploring our Python Interpreter - Stephane Wirtel
* with CPython 3.6.0a3+
* CPython has a developer guide (with how to start, where to get help...)
* Mentorship: http://www.pythonmentors.com (provides an open and welcoming place for those interested in Python development - a mailing list)
* the directories of Python (where can we find the information)
	* Doc -> manual
	* Grammar -> where the grammar is defined
	* Include -> C headers
	* Lib -> .py modules
	* Modules -> .c modules
	* Objects -> the builtin objects (bool, dict, set, tuple, list,...)
	* Parser -> grammar, lexer, parser, compiler
	* Programs -> the python executable
	* Python -> the virtual machine
* CHECK SLIDES for code


### Ingesting 35 million hotel images with Python in the cloud - Alex Vinyals
* challenge: unify all data
* every partner gives access to catalogs that have slightly different names, addresses and they need to match them
* skyscanner catalog is data release of cleaned data
* every partner gives them all images; usually similar but slightly shifted or cropped
	* they try to remove them
* they have more than 200 partners and need to process around 35M images
	* resize to 14 different configurations
* tale of an image processing pipeline
	* they are happy users of Postgres (Amazon RDS), SQS, EC2 - it's AWS all the way down :P
	* DjangoRestFramework but not Django ORM (SQLAlchemy instead)
	* Kombu for messaging/queues/amqp
	* Python 2.7
* Pipelines:
	* async: triggering->downloading->fingerprinting
	* sync: deduplicating->prioritising->generating
* Triggering goes through catalogs and looks for changes (which urls are new or changed)
* Downloader - downloads image and saves a copy in case partner removes it
* Fingerprinting - hash that can be used to match similar images
	* imagehash.dhash on tiles of an image to raise chances of matching
* what is a group of hotels? hotel listed by several partners
* Deduplicating fetches all images from hotel group: image groups added in one set
	* throw out positive matches for same_picture?
	* guarantees needed -> build a corpus optimised manually to get metrics with which you can compare runs
* Prioritising: pick best images and sort in "best" order (sometimes partners want a specific first one)
	* picking first image is not easy (example of a toilet)
	* detect features, prioritise based on that
	* tools to manually fix data
* Generating: make all needed sizes