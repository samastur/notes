# PyConWeb 2019 (Muenchen)

## Sunday, 26.5.

### New web platform security features for modern webapps, Lukas Weichselbaum

* On Twitter: we1x
* Common web security flaws (Google)
	* XSS (35,6%) — injection
	* CSRF — example of insufficient isolation
	* Clickjacking
	* Other (7.8%)
* Insufficient isolation also includes clickjacking, spectre/meltdown…
* check HackerOne report for vulnerabilities by industry
* Web platform security features
	* Injection defenses
	* Isolation mechanisms
* Injection defenses
	* Content Security Policy Level 3 (useful for the first time ;))
		* is a defense-in-depth mechanism against XSS
		* developers can control which scripts get executed and plugins get loaded
		* two modes: enforcement and report only
		* whitelist approach is useless (by-passable)
		* better, faster, stronger: nonce-based CSP
		* nonce needs to be on every script tag so does not work well with third-party scripts/widgets and potentially large refactoring effort
		* `strict-dynamic` propagates trust from script to dynamically generated script tags
		* CSP implementation:
			1. Remove CSP blockers (inline javascript handlers, javascript URIs)
			2. Add &lt;script> nonces
			3. enforce CSP with CSP response header
			 ``
			 script-src ‘nonce-…’ ‘strict-dynamic’;  
			 object-src ‘none’; base-uri ‘none’
			 ``
		* You can use CSP hashes for static sites
		* detailed guide at [csp.withgoogle.com](https://csp.withgoogle.com)
		* [CSP evaluator](https://csp-evaluator.withgoogle.com)
	* Trusted Types
		* eliminate risky patterns from your Javascript by requiring typed objects in dangerous DOM APIs
		* over 60 dangerous APIs!
		* require typed objects for passing HTML, URL, script string or script URL string (e.g. URL string => TrustedURL)
		* it can now be enforced with CSP on client side
		* the **default** policy is called as a fallback when a string is assigned to a sink; good way to get started and to identify dangerous DOM assignments
		* simplifies security reviews
		* [bit.ly/trusted-types](https://bit.ly/trusted-types)
* Isolation mechanisms
	* usually two types of attacks: on resources and on windows
	* Fetch Metadata request headers
		* `Sec-Fetch-Site`, `Sec-Fetch-Mode` and `Sec-Fetch-User`
		* what is source of request, its mode and was it a consequence of a user action
		* tells server how requests were made and if by someone else
		* allow requests without this to allow other websites to link to yours (and support browsers that don’t implement this yet)
		* this will ship in Chrome M76
	* Cross-Origin Opener Policy
		* `Cross-Origin-Opener-Policy`: `same-origin`/`same-site` will remove reference from websites that opened yours
		* available in Firefox Nightly and hopefully in Chrome soon


### What do travel, food & health websites have in common? Auditing websites & apps for privacy leaks, Konark Modi

* people are careless when going online (posting credit cards, username/password in background of photos…)
* not difficult to find stuff like this online (credit card statements…)
* legit use cases for 3rd parties
	* web analytics, content delivery network, on- and offline user journey and conversion tools…
	* Google can track 81% of the web traffic…
* talk about how including jQuery from Google server leaks data
* links are crafted uniquely for you (session IDs, marketing tokens…) and usually include several redirects
* example how this leak can give access to flight booking to 11 other websites and flight booking page HTML then includes information like passport data
* Emirates did not consider this as a security threat
* response to his article told him a lot of developers are not aware of these kind of leaks, but are willing to fix them
* leaks happened also ti experiences organisations like Mozilla
* websites are clearly leaking sensitive PII to plethora of third parties; more often without users’ consent
* more dangerously without the websites realising it
* Fixes
	* use secure connection (https)
	* private pages should have `noindex` meta tags
	* limit the present of third-party services on private pages
	* Referrer-Policy on pages with sensitive data
	* implement CSP and SRI
* Use mitmproxy for testing and finding these kind of problems
* Babel: network analysis framework being developed by Konark
	* classify calls as first or third party
	* uses dataset from whotracks.me to identify to whom those domains belong
	* uses http observatory from Mozilla to grade calls
	* and also use geo-ip database to list where calls go to to know if you are breaking privacy policy regarding location of data
* Babel not open-sourced yet, but will be


### Testing Python security, José Manuel Ortega

* Jose is an author of books on Python security
* Looks at:
	* Python dangerous functions
	* … (too slow)
* Improper input/output validation
	* example taking an input parameter in CLI and passing it on without validation
* a walk through some of Python’s dangerous functions (os, pickle/cPickle…)
* command injection (similar to SQL injections)
* provide arguments as an array to `subprocess.Popen` instead of one string (it escapes each of them)
* module shlex
* examples on how to prevent XSS in web frameworks
* automated security testing
	* SQLMap for SQL injection
	* XSScrapy: sql injection and XSS
	* Source code Analysis tools: Bandit (a security linter from PyCQA)
		* can be customised with plugins
* pyup — service for checking safety of code


### Real world Graphene: lessons learned from building a GraphQL API on top of a large Django project, Marcin Gębala

* Saleor - GraphQL-first, headless e-commerce
* first, introduction to GraphQL
* Schema is a contract between backend and frontend — it defines data and operations available in API (defined in GraphQL specification)
* Graphene
	* bindings for various web frameworks
	* building schema using Python classes
	* most popular (over 4k stars on Github)
* Lessons learned & recipes
	* took one year to fully migrate to Graphene
	* project structure
		* API lives in a single directory (because they had an existing Django project)
		* API modules mirror the structure of Django apps in the project
		* `api.py` — imports all API modules and exposes the schema
	* `types.py` — definitions of Graphene types mapping models to types
	* `resolvers.py` — resolver functions fo queries
	* `mutations.py` — definitions of mutation classes
	* `schema.py` — gathers all that and exposes it
	* Auth & perms — not provided by default by Graphene
	* they are using JSON Web Tokens to authenticate user with `django-graphql-jwt`
	* restricting access to queries and mutations
		* restricted with decorators provided by the above package (permission_required…)
	* unified mutations are the biggest issue
		* standard implementation has some limitations:
			* no input data validation
			* no standard for returning errors
			* a lot of boilerplate if the number of mutations is large (in Saleor it is over 150)
		* they built abstractions (BaseMutation) that does this and returns the same error structure using Django’s standard ValidationError (and isn’t part of Graphene)
		* built ModelMutation that cleans input data, create model instance, validates and saves it
	* database query optimisation
		* N+1 problem (relations in DB which may result in duplicated database queries)
		* `graphene-django-optimizer` — a small library that dynamically join related tables with Django’s `select_related` or `prefetch_related`
	* missing features and open problems
		* a fully-fledged API requires multiple additional third-party libraries
		* no way to calculate query cost to prevent malicious queries
		* not possible serving multiple schemas e.g. private and public API (so private API is exposed and protected only by permissions)
		* maintenance issues
			*  a lot of open issues and pull requests, uncertain development plans (original creator not involved anymore)
			* lack of good resources, inconsistent docs (had to find good patterns themselves)
			* non-pythonic concepts in the source code (e.g. Promise)
* Summary
	* GraphQL is a great tool for data exchange between backend and frontend (especially if using Typescript on front-end)
	* Graphene allows the gradual addition of GraphQL to your apps
	* the future of Graphene is uncertain
	* new libraries are emerging
	* still, Graphene is the most developed framework for Python right now
	* they are pretty happy with the final outcome (so advise to use Graphene with Django)


### Is Django too complicated?, Daniel Hepper

* there will be lots of personal opinions and anecdotal evidence ;)
* Django has an amazingly complex dependency graph
* Django imports more than a third of its modules even with a tiny hello-world app (but memory usage was almost the same as Flask’s)
* Django follows mantra explicit > implicit (Daniel thinks settings could offer even more guidance)
* some choices make life harder for developers with good reason (e.g. CSRF)


### Augmented and virtual reality on the web, Doug Sillars

* what we can do today
	* [A-Frame](https://aframe.io) from Mozilla
	* github: dougsillars/ARtGallery
* resize images (to power of 2) so that phone doesn’t have to
* Draco Compression for optimising .bin files (for optimising GLTF files)
* AR with WebXR is a spec (difficult to test since it only works on a couple of obsolete Chrome versions at this time)
* AR does not have to be processor intensive, nor utilise huge amounts of data
* generally works like s**t for now (alpha quality)
