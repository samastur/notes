# PyCon 2016

## Monday - 30.5.

### Keynote - Lorena Barba
* NumFOCUS is a non-profit that supports open source projects that can't afford to be a non-profit on their own (fiscal sponsorship)
* before #CS4all there was CP4E (computer programming for everybody) where Guido worked on bringing software programming to everyone and was a start of PSF
* people act through language (from book: understanding computers and cognition by Fernando Flores & ??)
* language/action perspective helps to explain open-source celebration
* check Project CyberSyn 1971! (book: Cybernetic revolutionaries by Elen Medina)
* we don't communicate just to share information, but also to change the world (instigate actions)
* making commitments to each other is what makes it possible for us to create value (Flores)
* structure of a conversation for action
	* A makes a request or offer
	* B makes a promise or accepts
	* B declares to deliver
	* A declares to be satisfied
* the point is that we can bring rigger to how we design an organisation
* full analysis has more outcomes (A can modify conditions...)
* commitment-based culture of collaboration
	* I'm reviewing this PR
	* project contribution policy: "log an issue for any question or problem."
* building community => building commitment (what we really mean by building community)
* if you work on system for learning, study Winograd and Flores book
* we are creating a ritual (by participating at pycon) of creating a shared culture of Python community through language acts


### Machete-mode debugging: Hacking your way out of a tight spot - Ned Batchelder
* http://bit.ly/mdebug (slides)
* Python can be chaotic
	* dynamic typing
	* no private, protected, final
	* all objects in heap (no stack allocations so things can live longer than expected)
	* nothing is off-limits
* can cause problems when building large systems
* use chaos to our advantage!
* bulk: real projects, real problems
* NOT for production! (things that will be shown) Use it just to get information and get rid off it immediately afterwards
* Double importing (problem: modules imported more than once?)
	* Django will complain about this when it detects it
	* modules executed when imported and only executed once!
	* how did we break this and how do we find it?
	* used inspect module and its function stack to get tuples of who called who
	* overlapping sys.path: import things.apps.first.models and import first.models
	* reason was that they used sys.path.append which is generally a bad idea
	* Lessons:
		* import really runs code; doesn't have special mode for definitions, just runs code
		* don't append to sys.path
* Finding temp file creators (some tests didn't clean up after them)
	* too many to eyeball
	* want a flare in the file itself: put in file name
	* monkeypatching by replacing tempfile.mkdtemp with own function (any name can re-assigned)
	* read the source! (handful of different functions; only want to tweak the file names)
	* _get_candidate_names is where all the names come from
	* you have to monkeypatch early before code is run
	* site-packages/*.pth: if line in .pth starts with "import ", then it executes that line
	* so site-packages/000_first.pth with import statement will run before everything else
	* use inspect.stack to get meaningful bits to create readable and useful name
	* Lessons:
		* std lib is readable
		* it is also patchable
		* use whatever you can touch and change
		* *do* use addCleanup if you are using unittest
* Who is changing sys.path (something was modifying incorrectly)
	* wasn't them; must be in third party?
	* wanted: data breakpoints (pub doesn't have them); when a piece of data changes
	* write a trace function (you can write a function and register with interpreter and will get run for every line of code of your program); slow, but needed for a short while
	* sys.settrace() for registering trace function
	* culprit: nose test runner
	* Lessons:
		* it's not just your code (sometimes other tools have bugs)
		* dynamic analysis is very powerful (even if it takes 8 hours it can be faster than other ways)
* Why is random() different? (randomised, but repeatable problems)
	* used seed particular to student
	* something wrong with import as first time randint is run gives different result than with later calls?
	* 1/0: easy to drop in, unlikely exception: ZeroDivisionError so easy to spot
	* import random; random.random = lambda: 1/0 (booby-trapped random)
	* default args evaluated once; bonus: the value was never used!
	* Lessons:
		* exceptions are a good way to get information (you can put information in message)
		* don't be afraid to blow things up
		* sometimes you get lucky
		* don't share global state!
		* do use your own Random object
		* do suspect third-party code
* Big Lessons:
	* break conventions to get what you need
	* but only for debugging!
	* dynamic analysis - use it
	* understand Python!


### Finding closure with closures - Thomas Ballinger
* yes, python has closures (since 2.2)
* global variables aren't (limited to module)
* python functions look a lot like closures
* identifying the scope of a variable
	* `bold.__code__` (bold is a function) to introspect functions
* pure function: function using only local variables
* weak support (read-only) for closures; we can't specify variable value in 2.x to variable in closure (not-local)
	* we can use nonlocal to make it assignable (Py3.6)
* how did we survive without nonlocal for so long? why isn't nonlocal used much?
	* make updatable in 2.x by wrapping it in a mutable object like a list (changing [0])
	* not used as much because Python has a great object model
	* also cultural: Python developers are comfortable with private data being externally accessible
* use nonlocal when appropriate


### Let's read code: the requests library - Susan Tan
* we spend a lot more time reading code than writing it (over 10:1 ratio)
* Step 0: prepare your editor to:
	* jump into any method or class
	* search files by keywords
	* get call hierarchy for any given class or method
* Step 1: git clone and open the repo
* Step 2: set up local dev environment to get into mindset of a contributor
* reading codebase is like pac-man. sometimes dots lead you in logical progression and sometimes they don't
* Step 3: look at unit tests (over 1600 lines of code in test_requests.py)
* let's look at one unit test
	* separate test in logical units
	* check docs for unknown functions before diving into code or using debugger to introspect objects
	