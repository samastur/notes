# Monday, 21.7.

## One year of Snowden, what's next? (Constanze Kurz)
* overview of what we learned about global surveillance since Snowden's leaks
* we have to conclude that democratic oversight failed
* study of Privacy International: Surveillance Industry Index ([source](https://www.privacyinternational.org/sii/))
	* at least 388 companies worldwide that sell 97 different surveillance technologies
	* passive and offensive hacking tools
* these companies usually don't take human rights as concern or potential for technology misuse
* in rare case of export restrictions they export from a different country to work around those
* surveillance priorities do not have much to do with terrorism:
	* EU institutions, eighty embassies worldwide
	* heads of foreign governments
	* G20 meetings, OPEC, World Bank
	* companies (Petrobas)
* ideology: "if you're looking for a needle in the haystack, you have to get the haystack first" (Deputy Attorney General James Cole, July 2nd 2013)
* US National Intelligence budget: 52.6 billion (probably more because some of it is part of other military spending)
* TAO (tailored access operations): exploits against Windows, MacOS, Linux, iOS...
	* grey market for vulnerabilities and malware (it would not be so big if the surveillance community wasn't spending so much on buying)
* success? modest (White House panel 2014 report)
* targeting Tor:
	* publications from July expose data collection explicitly targeting Tor users
	* parts of the search pattern code used in XKeyscore
	* data traffic to and from Tor servers
* they are challenging GCHQ snooping on article 8 of human rights convention in the European court of human rights (as GCHQ itself has helpfully suggested)
* The NSA: the only part of the government that actually listens :-P


## What can Python learn from Haskell? (Bob Ippolito)
* Python user since 2001 (simplejson, Mac stuff), use Haskell a lot now
* Python is not all bad, but has only enough time to talk about bad stuff
* works wonderfully for many and there are workarounds for many issues that do exist
* Haskell is not all good either
	* smaller community
	* non-strict evaluation
	* sometimes documentation is an academic paper
	* different terminology (Monoid, Functor, Monad,...)
* leave if you don't want Python to change a bit (ignorance is bliss); he was happier with Python until he started learning about other languages (makes a better programmer)
* nasty little paradox: the better somethings works, the less likely it will be improved
* correctness:
	* you will make mistakes in any language and with Python they are deferred to runtime
	* static analysis tools for Python are primitive
	* writing code to test for common mistakes is a lot of work
* Python compiles even code that obviously won't work (1 + "1"")
	* pylint and pyflakes don't catch it either
	* he couldn't find better tools and that's bad
	* most tools are concerned with the general case of Python, which makes it very difficult to do anything useful
	* Py3 already has syntax for function annotations and we've had decent AST tools for years
	* will talk about one promising option in a bit
* Erlang does not compile similar code even though it is also a dynamic language
	* has better tools (e.g. dialyzer which explains more about why something is wrong)
* Haskell also gives an error
* why is refactoring hard?
	* very hard to refactor without good tests
* gives an example of using Py3 annotations and [mypy](http://www.mypy-lang.org/) tool that can do better analysing
	* the author needs some help!
	* does not catch many mistakes
* why not X (PyPy, Nuitka...)? because they are concerned about speed, not correctness
* modest mypy proposal:
	* start using typing module even in vanilla Python3
	* support typing annotations in documentation tools
	* stop using PEP 3107 function annotations for any other purpose
	* start using mypy to type check code when running tests/continuous integration
	* start contributing to mypy! :)
* code quality tools for Python are really far behind other languages
* mypy is a huge step forward and can be used with existing Python 3 syntax
* another problem: mutability everywhere
	* mutability is the wrong default
	* very common source of bugs
	* prevents many optimisations (sharing, lock-free, etc.)
	* should be opt-in
* why is mutability wrong?
	* hard to understand code when underlying eta might change
	* defensive programming is difficult
	* sharing is prevented by copying
	* concurrent access requires synchronisation
* can we even fix this?
	* requires large changes to the language and libraries :(
	* see Rust or Swift for good examples
	* Haskell goes all the way (perhaps too far)
* another thing: expensive abstractions
	* Nothing is free. Function calls, classes, etc. do not typically get optimised away
	* Classes aren't easy to analyse for correctness, as they are always open. Subclasses ruin everything
* Python has grown many hacks such as Enum, named tuple, etc. that solve small parts of this problem
* Haskell and other ML family languages have an elegant solution to this
	* you can close what AST can be
* wrapping can be free
	* Haskell can define a wrapper for an existing data type at no runtime space cost (newtype)
	* strict fields can be inlined without boxing (no object overhead)
	* functions can be inlined (even cross-module)
	* no reason for 1 + True == 2
* performance?
	* everything I've talked about provide more ahead-of-time information
	* this information could be used by PyPy, Numba, Cython etc.
	* these features save developer time by making it easier to write correct code
	* writing less code in C/C++ will allow the language to improve much more quickly (thank you PyPy!)
* concurrency?
	* the python C API and current semantics make removing the GIL a non-starter
	* fixing other deficiencies in the language will make the necessary refactoring easier
* Summary:
	* incorporate all of the good ideas from mypy into Python
	* add some more of the conveniences from modern languages
	* enjoy a safer, faster, more capable Python


## Lightning talks

### xlwings and ExcelPython
* programming Excel from Python
* packages are complimentary and you can do with them anything you'd like to do with Excel
* still fairly young
* can work with higher abstractions (table in sheet)
* can work with Pandas
* supports Charting (will on Mac)
* seems to be Win/Mac only

### Packaging is AWESOME
* slightly better than it was and continually improving
* a lot going on (easy install, egg, virtualenv, setup.py, pip, cheese, twine, distils, setuptools...)
* packaging.python.org (single point of documentation)
	* kept up to date
	* not complete (mostly edge cases)
* Open Space: Thursday 11:00

### django-reportmail
* manage command as nightly batch
* [django-reportmail](https://github.com/hirokiky/django-reportmail)
* library for sending report mail of django management command easily
* some will recommend Sentry, but it requires a lot of resources

### PSF
* everyone can become member of Python Software Foundation now

### Flying Elephant
* investor seeking investments (50-200k euros of seed capital)
* you need to come to Berlin for first few months

### Architect - Object Relational Mapper's best friend
* automatic table partitioning
* works with py 2.6 - 3.4
* no external dependencies
* 92% test coverage
* MySQL and Postgres
* supports Django, Pewee, SQLAlchemy...
* [Architect](https://pypi.python.org/pypi/architect/0.2.0)
* [https://github.com/maxtepkeev/architect](https://github.com/maxtepkeev/architect)

### mate light
* display made out of Club Mate bottles and crates
* installation is usually at [c-base](http://c-base.org)
* each bottle has one LED and there's ~600 of them (?)

### Redhat project for automating stuff
* for automating everything related to programming except programming
* DevAssistant