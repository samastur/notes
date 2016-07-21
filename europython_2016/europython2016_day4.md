## EuroPython 2016 - Thursday, 21.7.

### pytest, what's new in 3.0 - Raphael Pierzina
* pytest is mature testing framework for Python
* running with py.test confuses people
	* pytest 3.0 will support pytest as well (py.test kept for backwards
	  compatibility)
* @pytest.mark.parametrize can be used to have same test where only parameters
  change (to avoid duplicating test code)
	* can do same with @pytest.fixture using input parameters
* pytest is extensible through plugins
* gh:pytest-dev/cookiecutter-pytest-plugin template for creating pytest plugins
* http://hackerbrot.github.io/pytest-tricks (what it says on tin)
* new features
	* approx() - asserts based on precision that values match (float
	  comparison)
	* yield fixture (so you can run setup code, yield, and after run tear
	  down code)
	* doctst_namespace (when using --doctest-modules) is a fixture for
	  renaming references? (check)
	* named fixtures - didn't get; look at release notes
	* --fixtures-per-test - if run --fixture it shows which fixtures are
	  defined in test files, but can get confusing; per-test shows same
	  information (including docstrings) for each test (you can still limit
	  tests with -k)
	* reinterpretation mode has now been remove. Only plain and rewrite
	  mode are available => --sert=reintrep option is no longer available
		* also removed some others
	* pytest warnings summary is shown by default so you learn when things
	  get deprecated (can disable with --disable-pytest-warnings)
	* rename getfunargvalue is renamed to getfixturevalue (old name is
	  deprecated)
	* plugins and conftest.py now benefit from assertion rewriting
	* #WriteTheDocs - focus on writing documentation for beginner user,
	  separate sections for advanced user and plugin authors
* talk to your manager about funding open source work
* blog.pytest.org - updates from core team
* speakerdeck.com/hackerbrot - SLIDES (have examples)
* plan to release 3.0 in next weeks


### async/await and why it is awesome - Yury Selivanov
* many ways to do coroutines in Python: callbacks & twisted, greenlets, yield,
  yield from and now async/await
* shows problem with using old methods (e.g. code could break if you just removed
  yield from statement)
* why is the anser:
	* dedicated syntax that is concise and readable
	* new builtin type for coroutines
	* constructs: async for and async with (maybe unique to Python)
	* generic, framework agnostic design
	* fast: only ~2x slower than a function call. **10-100x faster than yield
	  coroutines**
* async/await in real life: gh:magicstack/asyncpg (Postgres driver described in
  previous notes)
* asyncio developed and supported by Guido and is more of a toolbox for
  frameworks and protocols (already part of Tornado)
* what's inside:
	* standdardized pluggable event loop
	* interfaces for protocols and transports
	* factories for server and connections; streams
	* futures and tasks: callbacks + coroutines, timeouts, cancellation, etc
	* subprocess, queues, synchronisation primitives (blocks, events,
	  semaphores)
* asyncio documentation needs and will get improvement, but it is simple. You
  only need few functions
* loop = asyncio.get_event_loop() - create an event loop
* loop.create_task() - wraps coroutines in "coroutine runner" with a timer when
  to execute: a mechanism for
  the event loop to work with async/await
* asyncio.gather() allows you to wait on several coroutines and return when all
  end
* loop.run_in_executor() - runs slow CPU-intensive or blocking IO code in a
  thread or in a process pool and wait on it (poool = concurrent.futures....)
* loop.close() - called after run_until_complete ends to close loop
* asyncio debugging: PYTHONASYNCIODEBUG=1 python program.py or loop.set_debug()
	* configure python logging to see errors
	* configure your test runner to print out warnings
* **uvloop** is alternative implementation of asyncio event loop written in Cython
  and uses libuv under the hood
  * fast tasks and futures = faster async/await
  * super fast IO
* started 2 months before feature freeze
	* 2-5 days for the first draft of the PEP
	* 40 hours to prototype in CPython
	* 5 iterations of PEP
	* ~500 emails
	* ...got in


### Writing Redis in Python with asyncio - James Saryerwinnie
* goals:
    * how to structure a larger network server application
    * Request/Response structure
    * Publish/Subscribe
    * blocking queues
* slides will be online (and contain LOTS of code)
* great talk, worth rewatching, but difficult to take notes (fast and heavy
  code based)
* real redis does about 82k requests on author's laptop, this one with uvloop does about 38k :)


### Planning for the worst - guys from numberly
* overview of common problems and discussion


### Share nothing architecture - ??? (fill later)
* Considerations:
	* don't oversimplify
	* know when to start
	* infrastructure requirements
	* you may still require a source of truth
* Where to use shared nothing
	* web applications at scale (like Google or Bynder)
	* data warehousing
* Building blocks
	* ingest traffic (nginx)
	* front-end application (Bynder uses pyramid)
	* message queue (Celery)
	* back-end processing
* Future: scaling even further
	* shared nothing remains extremely effective
	* serverless architecture next big thing

