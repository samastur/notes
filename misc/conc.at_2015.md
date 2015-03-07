# .concat() 2015 (Salzburg, 7.3.2015)

## The Better Parts, Douglas Crockford
* we are not paid to use every feature of the language
* we are paid to write programs that work well and are free of error
* foot guns (Brendan Eich): feature of a language that is used to shoot yourself in the foot
* JavaScript requires more discipline than most languages (because of its flimsiness)
* scheduling
	* time it takes to write code
	* time it takes to make the code work right (should be 0, can be more than first)

### New Good Parts in ES6
* proper tail calls:
	* faster and reduces memory consumption
	* becoming real functional language
* ellipsis ... aka ...rest, aka ...spread
	* function curry(func, ...first) {
		return function (...second) {
			return func(...first, ...second)
		}
	}
* module
* *let* and *const* (let is the new var)
* restructuring: let {that, other}  = some_object; (let that = some_object.that, other = some_object.other)
* WeakMap: how objects should work (any value can be key); worst name ever
* megastring literals: multiline strings
	* http://jex.im/regulex (use if you write regular expressions!)
* fat arrow functions: shorter notation for functions

### Bad Parts in ES6
* class: relying on this will prevent developers ever understanding how JS works and how to use it effectively

### Good Parts Reconsidered
* I stopped using *new* years ago and have used `Object.create` (added to language)
* does not use that either because he stopped using *this* (ADsafe.org)
* if you stop using *this*, you are left with functional language
* also stopped using *null* (only uses *undefined* because it is the one JS uses)
	* typeof of null returns Object which is wrong
* stopped using falsiness; he explicitly looks for values
* does not use *for* and uses `array.forEach` and its many sisters instead; also not `for in` (replaced with `Object.keys(object).forEach`)

### The Next Language
* how will we recognise it?
* certain: when it arrives, we will reject it (programmers are as emotional and irrational as normal people)
	* it took a generation to agree that high level languages were a good idea
	* it took a generation to agree that `goto` was a bad idea
	* it took a generation to agree that objects where a good idea
	* it took two generations to agree that lambdas were a good idea
* we do not change minds; we need to wait for a generation to retire or die
* two sets: systems languages and application languages
	* biggest error with Java was that it couldn't design to which set it should belong
* more concerned with application languages, because most of us use them
* classification taxonomy: turns out to be really hard (you always get it wrong and get object graphs that don't quite fit); when it gets bad you refactor
* not inevitable part of programming, just if you do classes
* prototypal inheritance: memory conservation (may have made sense in 1995)
	* confusion: own vs. inherited properties
	* retroactive heredity (can change what object inherits after it was constructed; no good use for that)
	* performance inhibiting
* now an advocate of class free object programming:

	function constructor(spec) {
		const
			{member} = spec,
			{other} = other_constructor(spec),
			method = function () {
				// member, other, method, spec
			}
		// Freeze make object immutable
		return Object.freeze({
			method,
			other
		})
	}

* a bug story: int
	* int: the worst thing you could do is to throw away bits and don't tell anyone (overflow errors; which is what we do)
	* complement: saving gates in subtraction (from time when computers were made of vacuum tubes)
	* memory used to be really expensive and scarce
* Javascript: number (same as double)
	* having only one type is improvement
	* is the wrong type (0.1 + 0.2 == 0.3 is **false**)
	* binary floating point made a lot of sense in the 1950
* proposed solution: DEC64 (coefficient + exponent; number = coefficient * 10^exponent) for application languages
	* https://github.com/douglascrockford/DEC64/
	* dec64.com

## Ain't No Party Like a Third-Party JavaScript Party, Rebecca Murphey
* works at bazaarvoice.com (whitelabel reviews software for retailers)
* if someone can get a script tag on your page, **there's nothing they can't do**
* third-party javascript is consensual XSS
* the flip side is that customer pages can also mess with us (where us is 3rd party JS providers)
* not just supporting 6 browsers on 6 platforms anymore; now supporting users on millions of pages where each can change environment (e.g. by redefining index method of Array object)
* you can't check if you have clean execution environment because it would take way too much time
* some errors seen in wild:
	* more than one <body> element (you have to insert second with JS to be there; otherwise Chrome would remove it)
	* element with ID undefined ($("#" + {}.notexist) searches for #undefined)
	* user generated content is scary, can be malicious and needs to be escaped before used
		* problem when some other script parses page to process and re-add content (this will likely execute published, previously escaped JS)
* Third-Party Javascript book (http://www.amazon.com/Third-Party-JavaScript-Ben-Vinegar/dp/1617290548)

## How Teaching Kids Made Me a Better Developer, Ramón Huidobro
* US born Chilean living in Austria
* the challenge: at half year kids can leave the class and when they leave it breaks his heart
* learned: break ALL the things
	* appreciate that which I take for granted
	* learn by doing, understand by practising (need to do lots of katas)
	* simple != easy to understand
	* read loads of code
	* learn together
* having fun is important (easy to be too focused on solving the problem)
* why can't you:
	* defined method more than one to extend it?
	* define variable after you use it
	* why is coord system different on screen than it is in maths (0,0 top left instead of bottom left) => from how CRT screens worked

## Tonight We're Gonna Code Like It's 1999: Designing Responsive Emails, Jennifer Wong
* http://jennz0r.github.io/1999/ (slides for this and more expanded version of talk)
* have logo so people know who it is from
* a bit confusing slides to navigate, but really full of useful information
* there are tools that can inline CSS for you so you have clean development version and ugly one needed for mail clients
* max-width: 550-600px


## A Credit Card Walks into a Bar, Cristiano Betta
* what  **not** to do when building a Credit Card Form:
	* add a captcha (it will charge or not; no need to complete it)
	* ask users to register before purchasing
	* don't auto detect credit card type (check on wikipedia if writing a parser to process them; http://jquerycreditcardvalidator.com/)
	* odd way of collecting CVV (called different things on different cards; again different libraries that can make this nicer)
	* don't format dates as they are on my card
	* don't validate my card number inline (Luhn algorithm; the last digit on card is a check digit; card.io library for mobile apps that can read information from card photo)
	* AMEX has 15 digits, VISA 13 or 16 (other 16)
	* don't remember me
	* remember too much (store CC details) and then losing them; realistically you can't do it safely (solved with tokenisation which are revokable and tied to IP addresses)
	* you have a "reset" button
	* where was I? disappearing placeholders (moving them above when entering works better)
* @cbetta


## No More Tools, Karolina Szczur
* we assign empowerment to our choice of tools and tooling, which may not be true (good artists are good no matter which tools they use - huh?)
* simplicity: visual and/or operational
* we live in superabundance of information and the most crucial skill isn't multi-tasking but single-threading our attention
* the strive for simplicity is superficial; complexity is a fact of the world, simplicity is in the mind (Don Norman)
* Tesler's law of conservation of simplicity; every application has an inherent amount of irreducible complexity (the only question is who will have to deal with it)
* automation so we can focus on tasks that *can't be delegated*
	* postcss/autoprefixer
	* csslint/csslint
	* media optimisations (strip bytes)
		* jamiemason/imageoptim-cli
		* jakearchibald.github.io/svgomg
		* addyosmani/tmi
	* magnification
		* jakubpawlowicz/clean-css
		* jbleuzen/node-cssmin
	* code bloat (search and remove unused declarations)
		* giakki/uncss
		* geuis/helium-css
	* testing performance (find specific elements hindering performance)
		* zeman/perfmap (colours slow elements on page)
		* adyedinborough/stress-css
* debugging layout issues (find problems with the box model): pesticide.io (Chrome extension)
* GUIs for automation
	* Hammer for Mac
	* CodeKit
	* Livereload
* frameworks: Bootstrap, Foundation and Pure versus Basscss and Tachyons
	* better to start with smaller projects as foundations
* bit.ly/front-end-tools (great list of all sorts of tools)
* *npm* provides a native ecosystem for task automation
* Gulp and Grunt
* collaboration
	* bottom line: empower yourself, your team and community to work faster
	* put good of your team before personal preferences
* good tools, especially command line based, don't fail nor succeed silently
* it's easy to introduce unnecessary complexity by adding tools that manage other tools
* the right set of tools or lack thereof sets apart a craftsman from an operator (so what are the right ones? :))

## Useful Performance Metrics, Ben Schwarz
* 40% of people abandon your site when it takes more than 3 second to load (source: kissmetrics)
* etsy.com: add 160K to a page increases bounce rate by 12%
* 80%-90% of what people perceive as performance happens in browser
* lots of known stuff: minimisation, css animations, hardware accelerated properties...
* quicker also means less battery burn
* only load what you actually need
* Guardian loads article in HTML and the rest of website with AJAX calls
* **idea: set a budget (how much is too much?)**
* window.performance.timing (for building own metrics)
* user timing API: window.performance.getEntries() (slew of metrics for every asset on page)
* set a mark (to measure length of actions you are interested in): window.performance.mark('tweet')
* window.performance.getEntriesByType('measure')
* radical tools:
	* pingdom/new relic (real time stats about your users)
	* csssstats.com
	* calibreapp.com (speaker its author)
* takeaways:
	* monitor your work & set performance budgets
	* don't rely on what you think you know. prove it.

## Containerized Applications with Docker, Laura Frank



## Developing a Mindset for Web-Security, Frederic Hemberger



## Say Hello To Offline First, Ola Gasidlo



## Lightning Talks
