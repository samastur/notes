## EuroPython 2016 - Monday, 18.7.

### Keynote - Rachel Willmer
* beta.luzme.com (e-book price comparison site)
* what could you do?
* Product?
    * product: ebook, online training, wordpress plugin
    * one-off revenue
    * no support
    * good for side-project or first project
    * easy to start and good to experience the whole journey from idea to selling it
* Service? (bigger long term money)
    * SAAS
    * recurring revenue
    * support costs
    * better when full-time
* Publish
    * blog
    * youtube vlog
    * podcast
* how to monetise
    * sale of book, affiliate commission, advertising, sponsorship, data analytics
* Examples
    * Discover Meteor book by Sacha Grief & Tom Coleman ($300k in 18 months); self-published via Gumroad
        * packaged for different levels (price and content)
        * can get good ideas from it
    * Balsamic (annual profits >$2m)
* and finally
    * OK to start small
    * you don't LEARN until you start to fail
    * don't be afraid to THINK BIG
    * choose B2B and charge more (if you have a choice)
    * choose the customers you want
    * charge more (yes really!)
    * and above all... START A MAILING LIST
* work on your presentation skills!


## What Python can learn from Haskell packaging - Domen Kožar
* Cabal in Haskell
* build-type selects the type of set up (Simple, Make, Custom)
* in Python: PEP 518 which will eventually allow you to not even touch setup machinery
* Cabal features:
    * you can create flags that can be toggled (in a way very simple configuration language)
    * downside is that once it is compiled you can't tell which flags were used
    * in Python PEP 508 for environment markers (already supported in pip, but not many people use it yet)
* Hackage (Haskel's PyPI)
    * destructive editing: when you edit cabal file over Hackage interface, following line is added: x-revision: 2
* Cabal hell (figuring out and settings which version of dependencies are supported)
    * Elm handles this by forcing semantic versioning
* Stackage (LTS): stable source of package guaranteed to build consistently and pass tests before generating nightly and Long Term Support (LTS) releases
    * also released Stack which is a tool around Cabal
* in Python we have setup.py which is a script and we would need to create setup.json from all of them by running those scripts and we would need to create pypi requirements.txt (have some of it now, but not all) -- if we wanted to have it working similar to Haskel
* Takeaway: be more declarative, be more declarative, be more declarative...
    * so many files to touch: setup.py, setup.cfg, requirements.txt, MANIFEST.in, pyproject.toml, tox.ini


## Kung Fu at Dawn with itertools - Victor Terron
* not working with indexes -> fewer places where we can make mistakes
* we can use loop for anything that's iterable
    * objects that implements `__iter__`
    * implement it with returning self (chaining)
	* `iterator.__next__()` for returning next element
	* `StopIteration` should be raised when there's no next element
* generators are a special type of iterator
* example: how to use itertools.count (to generate sequential numbers) and (itertools.)filter to find primes in generator
* itertools.islice() (lazy evaluation, works wither iterators and has same interface as slice)
* simplify further using 'yield from'
* itertools.takewhile for pattern where we loop until we hit the limit
    * opposite is itertools.dropwhile
* example: how to use itertools.groupby to implement uniq and compress tools
* itertools.cycle for cycling among values
* itertools.chain for chaining iterators


## Dynamic class generation in Python - Kyle Knapp
* what it is: generate classes at runtime
    * data driven
* a simple example with type(): type('MyClass', (BaseClass,), {'hello_world': hello_world}) # name, parent classes, properties
* why:
    * can improve your workflow
    * can improve reliability
    * reduce physical size
    * is production-level pattern (used in boto3)
* downsides:
    * tracebacks (execution paths more difficult to follow)
    * IDE support is lacking (not there)
* boto3: AWS SDK for Python; dynamic and data driven
    * he supports API changes just by updating a JSON file in boto package and doesn't need to write any new Python code
* when should I consider dynamically generating classes
    * exists a canonical data source
        * examples: web API's
        * databases (i.e. sandman)
* example: build a light-weight boto3 that is model driven and validates inputs
* JSON RPC: overview
    * relies on HTTP POST's
    * single URI
    * JSON in body of HTTP request/response
* `method.__name__` and `method.__doc__` can be used to set correct documentation (method name and its description)
* making the client extensible:
    * example: operation cache that prevents unnecessary calls to server
    * done using base classes in type call
* dynamic class generation:
    * features/bug fixes have more widespread impact
    * produces robust code (because execution paths get more used)
    * enables an efficient workflow
* github:kyleknap/dynamic-web-api-clients
