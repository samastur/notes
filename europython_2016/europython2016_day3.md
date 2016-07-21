## EuroPython 2016 - Wednesday, 20.7.

### Gilectomy - Larry Hastings
* GIL added in 1992 and is/was fabulous design that solved many problems
    * simple
        * easy to get right
        * no deadlocks!
    * low overhead
    * single-threaded: fast!
    * i/o bound: ok
    * cpu bound: not ok
* Python has a rudimentary multi-threaded support and it is time we improve that
* there was a failed attempt ("free-threading") in 1999 with Python 1.4: 4x-7x slower than normal version
* Larry removed GIL in April, but it's terribly slow
* four technical considerations need addressing:
    * reference counting
    * globals & statics (per thread, shared singletons) - not as many as feared
    * C extension parallelism and reentrancy (a lot of code that depends on single threading)
    * atomicity
		* a lot of Python code expect atomicity (append to list...)
* three political considerations
	* don't hurt single-threaded performance
	* don't break C extensions
	* don't make it too complicated
* what won't work
    * tracing garbage collection (to get rid of reference counting)
        * it would break every C extension out there
        * too complicated (mor than existing and that one still trip people)
    * software transactional memory
        * falls on both counts at tracing
        * research quality now and Python can't wait
* my proposal
    * keep reference counting (don't break C extensions)
    * he switched to procesor's atomic incr/decr (costs 30%)
    * per-thread (PyThreadState)
    * shared singletons remain shared
    * C ext parallelism & reentrancy: they will need to adapt
    * atomicity: a bunch of locks
        * new lock api: Py_LOCK and Py_UNLOCK
* what needs locks?
    * all mutable objects (C mutable, not Python only)
        * str (has hash; problem with utf8 and wstr - unicode)
* userspace locks
    * need to be as light as possible
    * linux futex, windows has critical_section and os x has pthread_mutex (therefore we have them for major platforms)
* political considerations
    * won't be slower and won't break ext! => how can be true
    * by having two builds :)
* two entry points
    * so C extension will not run with wrong version because there won't be the wrong entry point
    * you might build one .so that supports both, but no idea if that is interesting
* this is really a new C api
    * then maybe a time to enforce best practices
    * PEP 489
* fails at: don't make it too complicated
* still work at progress
* how to remove the gil
    * atomic incr & decr
    * pick lock
    * lock dictobject
    * lock listobject
    * lock 10 freelists
    * disable gc, track & untrack
    * murder gil
    * use tls thread state
    * fix tests
* at language summit benchmarks:
    * approx. 3.5x slower wall time
    * approx 25x slower cpu time
* gilectomy's official benchmark: recursive fibonacci
* it's currently incredible slower; why?
    * don't know for certain, but looks like:
    	1. synchronization & cache misses
    	2. lock contention
* what makes cpus fast: caches (L1 1x, L2 2x, L3 10x, RAM 15x)
    * in gilectomy cache never warms up
    * buffered reference counting is a potential solution, but can lead to ordering problem
* where do we go from here
    * immortal objects
    * thread-private locking (idea: most objects never escape the thread in which they are created)
    * garbage collection
        * stop-the-world is current approach
        * buffered track/untrack
        * auto-lock around c extensions


### Core Developers Panel
* Larry: Python is becoming more difficult for beginners because of unicode knowledge is now required (but is good to know), but in general language is not changing much from version to version
    * Yury: unicode was always something rest of the world had to handle
* CPython core development is not funded although few people are paid to work on it full-time
    * PSF actually has about $2million, but spends it on other things


### Lightning talks
* Python Blogory (blogory.org/python) - curated directory of Python resources (packages, videos, books)
* PEP 487 - upgrade for descriptors in Python3.6; PEP 520 for defining order in which attributes were created
* www.tuxcademy.org - open source Linux training materials (in German and English)
* you can control OpenOffice from Python (pyoo library simplifies that) because it listens locally on port 22861
* Fabio Pilger showd Jupyter Labs: next generation of Jupyter
* qutebrowser - Qt keyboard driven minimal browser (vim-like browser); calls for more people to try to crowdfunding for financing projects with passionate users
* Jupyter tricks:
    * jupyter-dashboards: productionizing notebooks
    * microservices from a notebook: kernel_gateway
    * jupyter magics: %lsmagic
