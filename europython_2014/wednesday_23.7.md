# Wednesday, 23.7

## Our decentralised future (Pieter Hintjens)
* Pieter: fighter agains software patents, ZeroMQ developer, book writer
* ZeroMQ first community he has been in where there is no fighting (few flame wars), which is unusual in large projects
* ZeroMQ does not do design, meet ups to discuss what will come, no wish lists, no irc...occasionally have beer
* it is not accidental, because they developed over time a process of working which was to move away from centralisation
* world we live in is about decentralisation, giving up control (food supply for a city has nobody in charge)
* so why do we accept centralisation in our software projects?
* ZeroMQ community mimics their tool (async, decentralised, no locks...)
* the future of computing is distributed if we like it or not; you won't have a job if you will not be able to build for this world
* how do we build decentralised systems? we can't, we grow them organically
* internet is a set of protocols (RFCs), a set of contracts
* most of the work was done without actual real fighting at all (??)
* alternatives had good developers producing good software, but failed
* what makes good software? what is quality in software? (as industry we don't understand this)
* common way of developing with main authors and maintainer who reject patches they don't like; what was even worse than feeling of rejection was that stuff coming out wasn't very good either
* a lot of code in old ZeroMQ wasn't used because of wrong opinions and expectations of first author(s)
* so the current process is you send a pull request to master and Pieter will merge it (there's a little bit of filtering for sanity, but not much); maybe their patch is not the right patch, but their presence is the right presence
* the quality of patch is measured not by its beauty, but how accurate and relevant it is; Wikipedia was the model for development approach
* only market can write the software that the market needs
* software we build will reflect structure of the organisation that built it
* how do you come back to quality with ZeroMQ approach? (question from the audience)
	* allow mistakes, but work on quickly fixing them (let people learn from them)
	* if you can make mistakes quickly, you can also fix things quickly
	* so all work is done on master (they make stable branches for customers, but don't use them)
	* they let users contribute test cases
	* contracts need to be testable
	* every patch triggers test run so they know quickly if it broke anything
* another question: benevolent dictators in early stages?
	* in software we don't have that much controversy (?!); if there are it's good if they are documented and in software controversies are "stored" in forks and market picks the winner
	* his role is mostly being lawyer; use it, write about it (RFC) that community can use; he ignores his vision because it's junk
* question: this outlook feels anarchic (in political sense); how did this outlook come about?
	* you have authority in anarchy, but anybody is free to chose the one they like best
	* there is and has to be authority because who will enforce contracts (people will cheat); there is always a small percentage of dedicated cheaters and they will do great damage to communities
	* he had bad experience with people like that in FFII where he wanted to run by consensus
* question: you can have power without realising or accepting that you do (white male); how does such process take into account diversity?
	* he can't talk about diversity, but can talk about work done to remove obstacles to contributing
	* he thinks the process is competitive and may have certain communication patterns which may lead to most contributors being male
	* he's an Adam Smith type believer in free market; differences are valuable
	* he accepts that he is biased, but he can't see it (he suspects that gender bias is not the biggest one; there's age bias, race...)
* question: do you think people will chose the best software for their own benefit?
	* what is most interesting in software is choosing the right problems and solving the right (biggest) problems first
	* what ZeroMQ community tries to do is to identify those problems
	* it is fundamentally up to users to decide what is right for them; he can't
* question: is this a different management style? what about the size of community?
	* he doesn't think it's about scale; you have to decide what kind of approach you want to use


## Writing multi-language documentation using Sphinx (Markus Zapke-Gründemann)
* Sphinx: Python documentation generator with reStructuredText markup language
* output formats: HTML, LaTeX (PDF). ePub, Texinfo, manual pages, plain text
* [sphinx-doc.org](http://sphinx-doc.org)
* i18n: translating into other languages without changing the code (GNU get text is used frequently)
* why use get text for docs?
	* untranslated strings are display in the source language
	* markup is not duplicated
	* professional translation tools can be used
	* contributions possible without using a VCS
* check http://sphinx-doc.org/intl.html
	* .rst use to generate .pot with sphinx-build get text from which you get .po with Pootle
	* sphinx-build -Dlanguage gets you translate build
* gettext_compact = True (merge catalogs in fewer files)
*  pip install sphinx-into
* workflow:
	* make get text
	* sphinx-into update -l de -p _build/locale
	* then translate documentation
	* sphinx-into build
	* make SPHINXOPTS="-Dlanguage=de" html
* [transifex](http://www.transifex.com) is a web service for localisation that is free for open source where people can collaborate on translating
* sphinx has support for transfix (tx tool and options for sphinx-intl):
	* tx push and pull for exchanging data
* using code inside the documentation: use English in examples to make translations easier
* how to handle URLs: use the inline syntax because otherwise it is lost
* you can use the ext links extension to define URLs in the configuration to maintain versioned URLs
* ifconfig construct can be used to handle special cases (e.g. ifconfig:: language == de)
* make SPHINXOPTS... linkcheck can be used for checking links
* still missing:
	* a translations settings to build all languages with a single command
	* a way to add a "landing page"
	* use gettext_compact to create a single catalog
	* collect all indices into a single .po file
	* a language switch template

	
## gevent: asynchronous I/O made easy (Daniel Pope)
* example very similar to BaseHttpServer that starts a server which seems to be very scalable
* can use urllib2 unchanged with monkey path
* check slides and/or talk; hard to take notes for
* drawbacks of threads:
	* terrible performance (GIL)
	* threads in Linux allocate stack memory (see limit -s)
* drawbacks of processes:
	* no shared memory space
	* high memory use
* pitfalls of callbacks (callbacks are the new Goto)
	* untidy code structure...
* better: method-based callbacks in Twisted (but doesn't really deal with the problem)
	* still splits processing into multiple chunks
	* difficult to combine with other classes
* same approach can be used in asyncio with same problems
* generator-based coroutine (Tornado and new asyncio in 3.4):
	* using "yield"" and "yield from" to break out of stack
* example with @asyncio.coroutine looks much more readable (similar in Tornado)
* generators vs coroutine
	* generators provide coroutine-line functionality
	* but can only yield to their called
	* full coroutines can yield to any other coroutine (stack)
	* ...
* greenlets/green threads: gevent simpler because we don't need to use yields anymore
* Gevent: green lets plus monkey-patching
* monkey-patching bad?
	* works with any sync pure python code
	* generally works with async code too (only select() is emulated)
	* is optional (can't rely on it if writing libraries; can enforce in frameworks)
* always patch before any other import
* think of it as distribution of Python
* can spawn and kill greenlets easily
* has support for Semaphores, Locks... (less important than in threads because everything runs on one processor)
* business logic can call async code without being aware of it being async (and also vice versa)
* doesn't work on Py3 yet and there exists the possibility of deadlock between greenlets.
* pitfalls:
	* actual blocks (C libs...) halts everything
	* keeping the CPU busy prevents other green lets getting service


## Design Your Tests (Julian Berman)
* tests are code
* good coding principles that translate directly:
	* do one thing
	* simple is better than complex
	* test one thing
	* transparent is better than terse
* three step process: set up, exercise and verify
* what's the benefit of having one assertion per test?
	* the most important things about tests are failure; larger assertions give more context
	* sometimes one assert per test is not possible or useful
* two types: assertEqual and assertStatels (difference between asserting on data and some state of object)
	* compare some strings vs compare some HTML
* he defines his own asserts for handling more complex assertions (e.g. checking status code and mime type of a response together with its content)
	* order assertions in complex cases in the order of importance (e.g. content before checking content length)
* higher order assertions (assertRenders, assertValidHTML, assertHasContent, assertEqual)
* next steps:
	* mixin hell: GDBMMMixin, LoggingMixin, MagneticLoggingMixin, ResponseContentMixin, StatsdMixin, SaveMeMixin (mixins from author's company)
	* testtools.matchers (presumably better solution; check out)
* share your test helpers!
* if you find cases where you want to test only parts of the data (so need more assertions), look at code to see if what you are testing shouldn't return multiple objects (me: obviously doesn't work in case of Django's context data)
* sometimes it's nicer to shove results of complex assertions into container to return results of all of them (to avoid running tests multiple times); preferably not


## Supercharge your development environment using Docker (Deni Bertovic)
* what is docker?
	* light weight containers to isolate process to run
	* available on most linux distress with recent kernel
	* one process per container (best practice); can run more
* used terms:
	* image - immutable snapshot of a container
	* container - a running instance of an image
	* docker file - DSL for building new images
	* Hub (registry) - a central hub for sharing images
* it's fast with minimal overhead/resource usage and easy to run your whole production stack locally
* everyone on the team runs the same database, cache, amqp, c libraries...
* how to run containers? (docker run -i -t debian /bin/bash for interactive or -d for daemon)	* docker run -d -t postgres
* docker ps (lists all running containers; returns containers ids that you can use for other commands like log, kill...)
* images not prefixed by username on hub are maintained by the core team
* docker files: small DSL for building image
* on mac: use boot2Docker (runs then in virtualbox)
* how does docker fit into your dev environment?
	* important to run the same software in development and production
* NOTE: fast talking speaker; check the talk online
* because containers can't change, you have to mount volume to where data can be saved
* running your web app in a container:
	* simplify runtime
	* make sure everyone runs the same version of reps
	* what about virtualenv? --> C libraries!
* automation: use makefiles or shell scripts
	* sometimes they are not enough; then there's docker-py (https://github.com/dotcloud/docker-py)
* if you already use ansible/chef/puppet: you can use both together
* better to run one process per container because it makes it easier to update (replace) it
* fig: sort of vagrant for docker (configuration described with yaml)
* summary: run your production stack in dev, uses the same env everywhere and upgrade separate components more easily
* didn't touch deployment because of time, but he believes it simplifies it
* editing code is done by mounting it as a volume that is accessible at know position to docker container
* EU regulation make saving images to central hub a problem! (data protection)


## Advanced Uses of py.test Fixtures (Floris Bruynooghe)
* way back and difficult to read slides
* fixtures are powerful:
	* dependency injection
	* isolated
	* composable
* caching fixtures: fixture decorator has optional scope argument (e.g. scope='session' or 'function' so you can limit how many times it is called; values: session, module, class, function)
	* great for stable and expensive fixtures
* fixtures can have fixtures too (so they can be composed for example to pass db connection)
* fixtures can trigger skipping/failing of all dependent tests with pytest.skip (and pytest.fail): both of which will stop execution of the test that uses this fixture
* marks: run tests based on markers (aka tags)
	* make them strict (so you have to declare markers upfront and can catch possible marker typos)
* autouse fixtures (setup/teardown without explicit request)
	* @pytest.fixture(autouse=True) so it is called every single time
	* if combine with scope than you can limit to when it is run
* parametrising fixtures (pass params=... which inside of fixture can be accessed with request.param)
	* individual fixtures can be parameterised
	* multiple parameterised fixtures combine
* skipping parameters: skipping can be done on a parameter level
* accessing fixture info (find out what other fixtures are requested):
	* use request.fixturesname in fixture to get names of all applied
* plugins and hooks
* using command line options (new options can be accessed from fixtures and tests)
	* can be used for example to make sure that skipping is not allowed on CI server