# PyConWeb 2019 (Muenchen)

## Saturday, 25.5.

### Building robust APIs, Nejc Zupan

* start design of the API with documentation
	* easier to see how api looks and feels like
	* shows if naming is actually good
	* used documentation snippets as tests (making changing API by mistake harder!)
	* write narrative docs before you write code
* swagger 
	* de facto tool for describing and documenting RESTful APIs
	* has integrated test client
	* swagger ==> OpenAPI (now community driven)
	* does not include automatic tests?
* 2 ways of doing API: generation and validation
* Generation:
	* define views and models in Python
	* on the fly generate openapi.yaml by introspecting code
*  Validation:
	* write openapi.yaml
	* define views and models in Python
	* on-the-fly validate against openapi.yaml
* API-first incentivises you to write the specification so thal al of your developers can understand what your API does, even before you write a single line of code
* provides a clear separation between the intent and the implementation
* both approaches are imperfect; much easier escape hatch with Validation approach
* several services running the same API is easy with API-first approach
* his package pyramid_opanapi3 does not return responses that do not match definition of response
	* prevents people learning the wrong API returns


### Tornado Web Services on AsyncIO, Stefan Behnel

* Tornado similar to Node.js, Twisted, aiohttp…
* uses asyncio bellow (not its own loop anymore)
* async makes sense for web servers because most of the time they are I/O bound
* introduction to Tornado
* from tornado.gen import multi (for waiting on multiple futures)
* it is better to process data as it comes in and not to wait until the end
	* asyncio has as_completed (and tornado has a similar feature)
	* reorders requests in order they resolve so they can be processed quickly (for future in as_completed(futures)…)
	* from tornado.gen import WaitIterator
* caching
	* async code is single-threaded so you can cache with a dict!
	* easy to avoid duplicate requests as long as you are able to recognise them
	* you can await the existing unresolved future on later requests


### Hello to the World in 8 Web Frameworks, Aaron Bassett

* @aaronbassett (will tweet links to slides)
* Task - serving simple JSON response (simple API)
* Flask
	* started as Denied and as an April 1st joke
* cherrypy
	* nice for including in other scripts/applications
	* cherrypy.org
* falcon
	* fast
	* returns JSON by default as it was created for making APIs
	* falconframework.org
* hug
	* built on top of falcon with focus on simplicity (removing boilerplate)
	* nice for versioning API (@hug.get decorator has versions range parameter)
	* creates nice documentation
	* www.hug.rest
* microframeworks great when freedom is important (great also for demos)
	* easy to get in NIH syndrom
	* worked for company where this devolved to 5 places to define a model
* django
	* good documentation, batteries included
	* use ccbv.co.uk for class based views documentation
* pyramid
	* start small (can be used as other microframework), finish big (like Django, scaffolding and lot)
* tornado
	* async and can do websockets
	* www.tornadoweb.org
* Sanic
	* gotta go fast
	* not stable?
* not huge differences in code so better to care about:
	* documentation
	* bugs/triaging
	* alive community


### The dos and don'ts of task queues, Petr Stehlik

* task queue -- parallel execution of discreet tasks without blocking
* decided to use Redis (with rq framework) even though Kiwi otherwise uses Celery (new is always better ;))
* new service worked worse because they needed to do a lot of scaffolding, adjusting it to their needs etc. because they didn’t read documentation carefully enough
* final setup uses Flask, Postgres, Connexion, Celery, Redis on AWS
	* multiple deploy targets
	* Logz.io & Datadog
	* Sentry
	* PagerDuty
* check Monolith
* Connexion (OpenAPI 3, token-based authentication & authorisation)
* Lessons learned
	* use Redis or AMQP brokers (never a database)
	* pass simple objects to the tasks (id of object that gets queried for latests data)
	* do not wait for tasks inside tasks
	* use `retry_limit`
	* use `autoretry_for` (for retrying on specific exception)
	* use `retry_backoff=True` and `retry_jitter=True` (it prolongs retries with every try and avoid repeating at the same time, useful specially for 3rd party requests)
	* set hard and soft time limits
	* use `bind` for a bit of extra oomph in tasks (logging, handling, etc.)
	* separate queues for demanding tasks (and set priorities) to avoid starving long running tasks
	* prefer idempotency and atomicity
	* be careful of scalability of 3rd parties


### Matrix: Building a decentralized messaging platform using Python and Twisted, Richard van der Hoff

* Matrix is an open standard for interoperable, decentralised, real-time communication over the Internet
	* conversations are shared over all participants (conversations are first class citizens)
	* no single party owns your conversations
* Synapse - original Python implementation of server
* Dendrite written in Golang and has a bit of a problem following Synapse (supposed to be next version)
* decentralisation problematic (time is an illusion ;))
	* you can’t trust other servers to timestamp messages correctly (incompetence, malicious…)
	* cannot sort by time; matrix uses DAG (Directed Acyclic Graphs) — messages link to previous messages
* Synapse
	* originally proposed as ‘the reference Matrix home server”, a proof of concept
	* clear and simple and high performance not a goal
	* today: ~100k lines of Python/Twisted and 60K DAU on matrix.org (not simple anymore)
* Twisted/Python 2 (because Py3 was not supported in Twisted at that time as was distro support)
* as December 2018 they support Python 3


### Dependency Injection in Python: Lessons Learned from Enterprise Web Development, Preslav Rachev

* Java 101 ;)
* number of parts that need to be changed is a big part of modern complexity
* DI is a 25 EUR term for a 5 cent idea
* is a form of *inversion of control*
* a central component (“injector”) arranges all other components in one place, and provides them with their dependencies
* the Hollywood principle: “Don’t call us. We’ll call you.”
* in the majority of cases, DI boils down to passing dependencies to an object at a time of inicialisation
* Injector - Python package for DI (mimicking DAG automatic generation of Spring or Guice from Java)
	* github: alecthomas/injector


### Testing network interactions in Python, Dmitry Dygalo

* talk is only about mocking solutions, not live servers
* thumbs up for factoryboy!
* https://gitpitch.com/Stranger6667/talks#/
* https://github.com/Stranger6667/talks/tree/master/articles
