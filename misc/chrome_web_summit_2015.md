# Chrome Web Summit 2015

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
			* Publick-Key-Pins header
			* hard to do right in a way that all clients will match chain correctly; if you get it wrong you are sort of performing DDOS against yourself
			* new feature in Chrome: deploy PKP with violation reporting (practice mode: don't fail connections that violate this rule, but send me a report if they do; Public-Key-Pins-Report-Only)
