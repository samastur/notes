# Chrome Web Summit 2015

## Other people's notes
* Daniel O'Connor: https://danoc.me/blog/chrome-dev-summit-2015-notes/

## Accessibility
* 1 in 6 people have some form of disability
* important to be able to zoom in on UI
* need to think of people who are:
	* fully blind and need to use screen reader or braille display
	* hearing impaired who use captions
	* have motor or dexterity impairment and typing is difficult or mouse control is
	* neurological diverse (dyslexic): need to use spellcheck, autocomplete
* even otherwise healthy people sometimes need help:
	* broken arm
	* when in a loud room
* there is a set of design principles that you can use to achieve a lot without testing with all devices
* take advantage of built-in (html5) accessibility when possible
* Polymer provides custom elements with certain guarantees about accessibility
* they focused on team upskilling to share responsibility
* automated testing to catch low-hanging fruit
* thorough checklist-based manual testing
* https://github.com/webcomponents/gold-standard/wiki
* manual testing checklist:
	* focus/keyboard: focusable, focus visible, keyboard support
	* semantics: declared semantics, meaningful structure, labels
	* flexible UI: sufficient contrast, redundant colour, high contrast, magnification
* prioritisation: based on how important the element is and how critical the issue is
* walkthrough case study of paper-checkbox element
	* focusable: can you navigate to it with Tab and Shift-Tab
* tabIndex (0 - natural order defined by DOM order, -1 focusable, but no in the tab order, >0 in manual tab order)
* styling focus with :host:focus #star { background-color: #fee; }
* iron-a11y-keys use if you use Polymer
	* mix-in behaviour for keyboard handling
	* shows an example of use (added as behavior)
* test with screen reader
	* free:
		* OS X/iOS: VoiceOver (built-in)
		* Windows: NVDA (free/open source) at nvaccess.org
		* Chrome OS: ChromeVox (built-in)
		* Android: TalkBack (built-in)
* declared semantics: does the components expose its semantics by wrapping/extending a native element, or using ARIA roles, states and properties?
* element semantics provided:
	* role (the structural role/UI idiom of this element)
	* value
	* state
	* properties (e.g. minimum value)
* for custom we need to provide those values ourselves using WAI-ARIA
* label able elements: button, input, keygen, meter, output, progress, select, textarea
* aria-label can provide label for assistive technology-only
* aria-labelledby specifies ID of element that provides label
* write regression tests for a11y bugs
* Flexible UI
	* redundant colour - when the component uses colour to communicate information, does it also provide the same information another way?
	* magnification - does the component render correctly when magnified
	* sufficient contrast - are labels, icons, etc. perceivable and usable by low vision users?
* 1 in 12 males are colour blind (use NoCoffee vision simulator at http://bit.ly/no-coffee)
* minimum contrast should be 4.5:1 unless large-scale text where it can be 3:1 (which often not good enough)
	* best 7:1
* bit.ly/a11y-devtools (accessibility extension)
	* example looks great (shows contrast and makes suggestion you can use to see how compliant would look like)


## Asking for Permission - respectful opinionated UI
* 20% click through rate from push notifications (VP from Beyond the Rack)
* opt-in rate for geolocation for Android Chrome? 6%!
* presumed reason: because sites are asking for permission at the wrong time
* opt-in for notifications is at 17%
* explain why you are asking for a permission/provide clear value; it manages user expectations and app can still ask for permission at a later time
* deprecating powerful capabilities over insecure origins (geolocation, getUserMedia...) so https only
* Permissions API at https://w3c.github.io/permissions/
* Design principles
	1. provide clear value
		* communicate intent
		* images are better than words (localise better - really?)
	1. ask at the right time
		* worst time to ask: on load
		* better: on user gesture if they can indicate they want that feature (e.g. turning on notifications or tries to take a photo)
	1. only ask for what you need
		* bundle when appropriate: .requestAll() (e.g. notification when at certain location)
		* ...but don't take APIs hostage (ask for what you don't need)
	1. give users control
		* allow users to change their mind in app itself: .revoke()
	1. handle failure gracefully
		* don't crash or not communicate what is happening
		* design for resilience; site should not fail when users do what you don't expect them to


## Deploying HTTPS - The Green Lock and Beyond
* lots of reasons why it is a good idea to use it
* in future required to use powerful web features (e.g. camera access)
* HTTPS 102:
	* find and understand misconfigurations
	* keep server config yup with crypto deprecations and new attacks
	* prevent sensitive data from being sent over insecure HTTP
	* protect users from HTTP downgrade attacks
	* detect and remove insecure HTTP content
	* worry about rogue CAs mis-issuing certificates for your domain
* understanding the lock icon
	* neutral state with no security (list of paper icon); not really neutral and Chrome will introduce negative state indicator instead of neutral
	* green lock: secure connection
	* https with same icon as neutral because Chrome found problems (used to be grey lock with yellow badge)
	* red slashed lock icon: certificate expired or wrong one...
* introducing new DevTools security panel to find and fix problems that prevent the Green Lock
	* shows state
	* keeps up to date information about your configuration (algorithms get broken...)
	* shows non-secure origins from which sub resources are loaded
* getting to green
	* what can downgrade the lock icon:
		* most common: mixed content (loading sub resources over http)
		* green lock can be downgraded to red if user choose to load scripts from insecure sources (which chrome blocks by default on https site)
	* use Content Security Policy to catch mixed content: Content-Security-Policy-Report-Only: default-src https:; report-uri https://example.com/reportingEndpoint
	* check report-uri.io
	* Content-Security-Policy: upgrade-insecure-requests; (tells browser to try to load http resources over https)
* beyond green
	* always protect sensitive cookies: Set-Cookie: session_id=abc; Secure (this cookie should only be sent over HTTPS)
	* turn on always-HTTPS: Strict-Transport-Security: max-age=31536000; includeSubDomains (my site and subdomains should always be accessed over HTTPS and remember this for 313536000 seconds)
	* protecting against mis-issued certificates
		* solution 1: certificate transparency (log all certificates publicly and can monitor logs for your website)
		* solution 2: public key pinning (for websites with more operation experience) - lets site describe what its public key and certificate chain should always look like
			* Public-Key-Pins header
			* hard to do right in a way that all clients will match chain correctly; if you get it wrong you are sort of performing DDOS against yourself
			* new feature in Chrome: deploy PKP with violation reporting (practice mode: don't fail connections that violate this rule, but send me a report if they do; Public-Key-Pins-Report-Only)


## DevTools in 2015 - Authoring to the Max
* can reorder tabs in DevTools and added overflow menu for tabs you don't want to have there all the time
* improved context menus
* colour picker and colour palettes; shades of colours come up if you long press on a colour
	* also coming up a line for seeing and working with colour contrast
* a new Device Mode for a new mobile-first world
	* can show media queries (together with breakpoints?)
* mobile emulation is not the same as actual phone
	* previously had to do remote debugging through inspect
	* new: inspect devices within DevTools, can connect from there and debug any pages open on phone
* rule of thumb: a visual design practise is out of fashion when browser vendors have added a way to do it in CSS (Paul Bakaus)
* new animation inspection mode
	* can redesign animation curve with mouse
	* can capture animations that you can then replay (and see/change structure of)
* more power to Javascript developers
	* inline variables while debugging
	* proactive compilation if you change code while debugging to notify you of syntax errors
	* can blackbox code (e.g. frameworks) through which you don't want to step during debugging
	* object formatters (can style/format objects produced from transpilers)
	* better tools for working with Service Workers in Resources panel
* Layout mode (WYSIWYG for DevTools; maybe)
	* more sensible tools (resizing rarely used on non-images); change paddings and margins
	* with scroll wheel can change rule


## Engaging with the Real World: Web Bluetooth and Physical Web
* Web Bluetooth is already available
* from Bluetooth PoV there's Device, which has Service(s)
	* in Services can be multiple values that you can read called Characteristic
* when you request device (navigator.bluetooth.requestDevice) you also specify which services you are interested in
* can listen to events produced by characteristics
* Polymer: platinum-bluetooth
* BLE peripheral simulator at tinyurl.com/BLE-SIM
* could phone know about pages available in an area? (google is working on that)
	* situational discoverability of apps
	* web needs a discovery service
		1. Discovery
		2. Fetch/Rank
		3. Interact
* Google playing with few things around BLE picking up urls from beacons that can't trace you
	* use proxy for protecting privacy as long as there are enough of them
* Eddystone - Google's beacon format (UID + TLM + URL)
* http://physical-web.org


## HTTP2 102
* tl;dr: switch!
* if you invest, you can squeeze out lots of performance
* http/1.x hacks: concatenate it, minify it, sprite it...
* http/1.x flaws:
	* flaw #1: HOL blocking (aka one resource at a time syndrome); can't use connection until transfer is finished
		* going from 2 to 6 connections just postpones problem
	* flaw #2: headers repeat a lot across requests (because it is supposed to be stateless protocol and sessions had to be invented) and headers are not gripped
* http2 is protocol upgrade; it starts as 1.x and upgrade to 2 only if clients supports it
* http2 is TLS encrypted connection and has only one connection
	* what used to be one TCP connection is now a logical stream which are chopped into frames so if one starts blocking another one can take over and transfer its data on same connection
* headers frames and data frames
* not necessary to do concatenation, inlining, spiriting anymore (vulcanize?) which made caching inefficient as sprite could be invalidated by one item changing
* since headers are now separated, they can also be compressed (hpack is a header compression specifically for HTTP)
	* we can ask for value that were sent in previous request while decoding current so we don't need to retransmit it (which makes multiple origins more expensive)
	* no more need for sharding or multiple CDNs
* features to exploit if you invest more:
	* PUSH - allows to respond to request that hasn't been sent yet (offer resources that are complimentary to requested)
* still needed: GZIP/Deflate, first render, CDNs/DNS lookup, Cache-Control
* should use now?
	* browsers ahead of servers (all major browsers support http2)
	* NGINX, Apache support it
	* bit.ly/http2implementations
* local dev: github.com/GoogleChrome/simplehttp2server (http2 and push)
* production
	* tier 1: put your static assets on a h2 CDN
	* tier 2: h2 reverse proxy in front of your server (CloudFlare does this now)
	* tier 3: full h2 deployment
* future?
	* manifest.json for static hosters to specify which items to push for which resource
	* WebSockets - maybe not a thing anymore (still work, but might be suboptimal as long polling is fine again); time will tell


## Increase Engagement with Web Push Notifications
* mobile web has bigger reach than apps (8.9 to 3.5 of something)
* how to improve engagement? web push (= reach + engagement)
* you can subscribe notifications that you can receive even months after you have closed the original website's tab
* Facebook: the mobile web is growing (number of users is growing as fast as users of app)
	* desktop also still important
* FB: built proprietary push implementations on Opera and UC browser and now know users want them
* more examples of happy users of this technology
* implemented using Service Worker which keeps a connection open to Push Service which pushes notifications
* pushManager.subscribe sends in Chrome's case message to Google cloud messaging :(
* push doesn't tell SW what it needs to display, just that event happened and it has to fetch that message from notification Web Server (so not Google)
* UX
	* send notifications users care about (is it urgent and important?)
	* don't request permission on page load
	* make permissions controllable
	* de-duplicate with native (same message from native app and web; not solved yet)
	* refocus existing windows, don't create new tabs (example at 21:00-)
	* summarise notifications (if more than one message, show summary of all)
	* load instantly from a notification tap
* what's next?
	* custom actions (can build experiences that work within notifications)
	* payload support (site will be able to send serialised JSON through push service to client to skip network request back)


## Instant Loading with Service Workers
* App Shell + Service Worker (SW)
* demo shows how new pages load almost instantaneously with SW loaded
* the model that will be discussed is the surest way of building apps that return in under a second (which is their recommendation)
* 1 second delay leads to 11% fewer page views and 16% decrease in customer satisfaction
* What's an App Shell?
* Anatomy of an App Shell: HTML + styles + Javascript - content (basically what Cordova gives you + urls)
* cache the App Shell on SW install event (GET /shell and cache its contents)
* after install event comes activate event where we can clean up cache
* then goes SW goes into idle until there's a request for network resource
* Service Workers without the Work: libraries sw-precache (shell) and sw-toolbox (content); both battle tested
* sw-precache (found on npm)
	* integrated with Gulp and other tools; creates a service worker code for you for your folder tree of shell resources (includes hashes used in caching so only modified elements get refetched)
	* sw-precache --verbose
* sw-toolbox: for caching app's dynamic content; loaded by SW at runtime
	* canonical implementation of different caching strategies
		* cache-first, network-fallback (toolbox.cacheFirst)
		* network-first, cache-fallback (toolbox.networkFirst)
		* cache/network race (picks from cache, but updates it again from network; toolbox.fastest)
	* thinking strategically and you can choose different ones for different url patterns
	* can specify timeouts (e.g.: {networkTimeoutSeconds:3} with networkFirst)
	* rein in your caches (maxEntries...)
* SW are progressive enhancement
	* on first visit there is no SW so pages need to handle that scenario anyway
	* so your architecture shouldn't change to support browsers without SW
* SW lets you:
	* serve the HTML for the landing page cache-first
	* ...even if that page is server-rendered
* check iFixit API demo at: ifix-pwa.appspot.com and github.com/GoogleChrome/sw-precache/tree/master/app-shell-demo
* www.code-labs.io/codelabs/sw-precache/
* github.com/Google/web-starter-kit (boilerplate and tooling for multi-device development)
* github.com/GoogleChrome/application-shell (Vanilla JS)


## Polymer - State of the Union
* Web Components library with components
* 1.0:
	* re-designed data binding
	* styling system using custom properties
	* lightweight shadow DOM shim
* sets of components (elements.polymer-project.org):
	* Fe (iron) - core elements
	* Md (paper) - UI components following material design guidelines
	* Go (Google Web) - components for Google API's and services
	* Pt (platinum) - push notifications, offline caching and more
	* Au (gold) - E-commerce elements
* Web Components are happening
* over 1 million pages use Polymer
* over 320 Google projects use it
* every feature has a cost (thinking what cost are they transferring with adding it)
* 1.2 is 20% faster than 1.0 (conservative; in certain cases can be more)
	* ergonomics + performance (a11y better, nicer to use...)
* for 99% of needs you can get everything you need with Polymer and Service Worker: web is now a first class application platform
* web has become a first class development platform (don't build walls between yourself and the platform)
* polymer is like a scalpel right now: can do powerful stuff but still need to be careful
* it should be easy...to use components, build, share, optimise and assemble apps from components
* common polymer mistakes:
	* missing imports
	* binding to a non-existing property
	* incorrect class binding
	* unbalanced delimiters
	* missing computed property functions
	* broken observers
	...
* polylint: for catching mistakes without needing to transfer not-required code
* polydev: chrome DevTools extension for profiling web components
* how do I:
	* structure my app?
	* lazy-load sections of my app?
	* handle routing?
	* internationalise?
* Carbon elements - work in progress to above problems (should be as close to platform as possible)
	* app layout elements (more general solutions than Material Design layout components): for example tabs on top on desktop that move in sidebar on mobile
	* https://polymerlabs.github.io/app-layout/
	* other things more work in progress


## Progressive Web Apps
* SW important because they keep urls (if you can't link to it, it's not web)
* the cost of user to try an app is between 1 and 2$; cost of loyal user has gone up to 4$
* low friction is the web secret weapon (every step reduces adoption by 20%)
* adding to desktop is different from bookmarks because you might bookmark a smaller bit of a whole (a specific URL not root one)
* Chrome's Progressive Web App Install Heuristics:
	* needs Service Worker (manifest _start_url_ must always load)
	* TLS (site must be at a secure origin)
	* engagement (2 visits to scope'd documents over 5+ min)
	* manifest (valid manifest with required properties)
* Opera thinks progressive web apps are a great match for budget phones
* Web Manifests store metadata for a Progressive Web App: icons, description, colours, and related info that lets browsers create high-quality experiences for the launcher icon, task switcher and splash screen
* a bit before 20 minute it starts to explain manifest contents
* @media (display-mode: standalone)! (at least in Opera)
* manifest creator: brucelawson.github.io/manifest
* manifest validator: mounirlxmouri.github.io/manifest-validator
* chrome://inspect for debugging on devices and use DevTools for development
* chrome://flags: use to bypass user engagement checks for development
* things to consider:
	* without URL bar, app needs to provide own navigation (refresh & back/forward UI)
	* share UI: integrate with native: https://goo.gl/8pF0ca
	* onbeforeinstall Event: lets you prevent banner, re-show at opportune time, enables install rate tracking for analytics
	* deep linking considerations: working on it!