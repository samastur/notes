# CSSconf EU 2014

## CSS Performance Tooling
* slides: http://addyosmani.com/blog/slides-css-performance-tooling/
* 3 tiers of optimisation tooling
	* baseline: magnification, concatenation, image optimisation, compression, async scripts, leverage caching, WOFF2 for fonts, spiriting, avoid redirects
	* get fast, stay fast: inclining critical CSS, deferring non-critical assets to avoid render blocking, removing unused CSS, visual regression testing to verify change, performance benchmarking
	* nice to have: reduce duplicate colours, selectors, font-families, sizes
* optimising <JSConf.eu> to make it an instant experience
	* MEASURE, other you are guessing
	* use Speed Index to see how complete page is (lower score is always better)
	* performance budget:
		* ~1s page load / Speed Index under 1000 (can be max 200ms server response time)
	* use webpagetest and PageSpeed insights
* with PageSpeed aim for 85 on mobile and 90 on desktop
* Chrome's DevTools bandwidth tools are great!
* average page size: 1.8MB of which 1.2MB are images and 300K are scripts and styles (thank you Bootstrap)
* optimise
	* a lot of tools installed from npm
	* minify CSS, HTML, JS & images (packages for grunt and gulp exist)
	* cssshrink website
	* imagemin and svgmin
	* remove unused CSS (especially if you use a framework like bootstrap)
		* wrong way to think about it:
			* I just want to ship the CSS for current page
			* I'm going to ignore styles used in other pages
		* right way: what site-wide CSS am I shipping that is not used by any pages
			* pattern of unused CSS? maybe, check
		* uncss remove unused CSS across multiple files: loads files and execute Javascript, then checks which styles are used, CSS parser, then filters out unused and creates optimised (grunt-uncss, gulp-uncss)
			* works with Sass, Less, Stylus
			* css path: source -> sass -> css -> uncss -> minify -> done
			* most useful when using a CSS library or framework
			* warnings: can't identify with 100% certainty that all CSS will be in tested files, so test it!
	* CSS regression testing
		* useful for more than checking changes
		* regressions in your UI can be hard to identify
		* cost of manual testing gets higher with responsive layouts
		* can drive test with fake data if needed
		* solution: visual regression tooling + Resemble.js
		* PhantomCSS (screenshot comparison library)
		* CasperJS (navigating on PhantomJS)
		* not as useful on pages with varying content
	* optimise critical path CSS
		* render visible content first; don't render whole page immediately
		* scripting alone is not good enough (at least not in 2014!)
		* multi-part problem to automate; what we need to do:
			* extract stylesheets from HTML
			* generate the above the fold CSS
				* decide on target viewports. Multiple? One sweet spot?
				* keep it small and lightweight: under 14KB
			* inline critical-path CSS in <head>
			* asynchronously load the rest of your styles
		* critical built on penthouse for finding critical-path CSS; also criticalness
		* workflow: source -> extract stylesheets -> generate critical path CSS for a viewport ->inline styles in head -> output (CHECK SLIDE)
		* async load CSS with loadCSS.js
		* challenges:
			* background images or fonts missing (relative paths may need updating to absolute)
			* unstyled content showing (most common problem is with clearing floats; use the clear-fix pattern)
			* special glyphs not showing/showing incorrectly (hexadecimal format in CSS needs to be prepended with a backslash like \2192)
	* optimising animated image
		* check GIFBrewery!!
		* packed image diffs
		* video can reduce it further
	* for a while they have recommended mod_pagespeed and ngx_pagespeed which are easy to set up
		* can get close to hand optimisations just by using this module
* automate performance measurement (detect when you've called off the fast path)
	* WebPageTest CLI (webpagetest nom module)
	* grunt-perfbudget (budget in ms for render, KB for page weight)
	* psi module (PageSpeed Insights CLI)
* stylesheet complexity analysis
	*  easiest to keep it maintainable is to minimise repetition
	* colorguard (searches for a very similar colours in CSS)
	* Parker more general analysis tool
	* StyleStats website
* Rinse & Repeat