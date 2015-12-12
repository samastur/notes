# FF 2015

## How to win at mobile accessibility
* talk about: barriers, UI patterns and state of tooling
* why should I bother:
	* sales potential
	* legal risk
	* innovation opportunity
	* it's the right thing to do
* Microsoft inclusive design toolkit
	* disabilities can be: permanent <---> situational (injury, new parent)
* almost half users use mobile screen reader either more often or almost as often than desktop screen reader
* accessibility is easier to get right on native platforms
* barriers to access for mobile a11y
	* locked-down zoom
	* hijacked scrolling
	* visual clutter (minimise cognitive load!)
	* ambitious visual icons (check your privilege, age, ability, demographic...)
	* conflicting gestures (other UI affordances needed)
	* fragmentation of screen sizes...
	* spotty HTML5/ARIA support (mobile is less mature than desktop)
* mobile UI patterns
	* you're competing with browser's Reader View
	* BBC mobile a11y guidelines (recommended)
	* use buttons (and other semantic elements)
	* use aria-label to label buttons and controls (one of easiest wins)
	* touch targets: generous padding for bigger targets (check menu of simplyaccessible.com)
	* crafting mobile tab order
	* hidden links shouldn't be reachable
		* hidding from screen readers:
			* display: none (fully hide, but not animatable)
			* disabling interactive elements in HTML (keep content visible, but hide from reader) with aria-hidden="true"
* state of tooling
	* pretty good desktop browser testing tools, on mobile so-so and have nothing to run screen reader and run that
	* there are differences between desktop and mobile for same tools (e.g. VoiceOver)
	* iOS Safari has an Accessibility Node Inspector
	* what would we prefer is an audit that tells you what's wrong without having to manually poke at each item
	* aXe Audit in Firefox Devtools (not really for checking mobile devices...yet)
	* Firefox WebIDE can be used to tether FF/Android and check; needs audit tool


