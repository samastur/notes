# WebCamp Ljubljana 2016

## Anže Žnidaršič, Using JavaScript ES2015 (ES6), ES2016, ES* in production
* going through "new" features: block variables, arrow functions, classes
* browser support getting better, but lacking in Safari, Android and IE11
* can use Babel to transpile
* dev workflow: local vagrant dev boxes; provisioning, updating and code deployment done through Ansible
* ES2016: Array.prototype.includes (element) and exponential operator


## Josip Maslać, Scaling just a bit
* founder of nabava.net (price comparison site)
* developed (front and back) in Java
* requirement: minimal change to the application code
* HAProxy front-end defines interface to which clients connect to
* HAProxy back-end is interface to servers that serve requests
* can be used also for balancing databases


## Slobodan Stojanović, Offline web apps
* pitch why this is important
* offline events: navigator.onLine - online/offline; doesn't work very well
* App Cache with known problems
* Service Workers: acts as a proxy server between web application and server
* WebSQL has a better performance than IndexDB (where it exists)
* Dexie.js - minimalistic wrapper for IndexDB
* slides.com/slobodan/offline-web-apps


## Mark Nadal, The Front-end Back-end without DevOps
* http://gundb.io
* http://gun.js.org/think.html (tutorial)
* what if db takes care of synchronising the state? that's the philosophy of Gun
* server can act as a relay server because it's the DB (Gun in browser) that takes care of syncing
* it's decentralised as DB is a JS app running in browser
* demo of live updates (realtime sync)
* conflict resolution (when two clients unreachable): last one wins?
* Gun is a graph database
* hope to soon release a SQL query engine on top of Gun


## Andrei Zvonimir Crnković, RxJS - Go reactive or go home
* Observable - subscriber pattern (observable = streams of events over time)
* transformations: .throttle and .filter
* what to watch out for:
	* hot and cold observables (cold only run when subscribed to; hot running independently of subscription)
	* merging of streams (of events) such as in RGB sliders (obsRed.join(obsGreen).join(obsBlue).map(hex))
* reactive extensions (Rx)
* should you use this? maybe (early days and lots of things don't yet work as you'd expect/as they should)
* http://andreicek.eu/rxjs


## Andraž Bajt, Purely functional front-end
* bad procedural programming: global mutable state, no modularity, hard to test, relevant pieces may be far apart
* what we want: modularity, local reasoning, reusability, comparability and abstractions
* this is the promise of Pure FP (functional programming)
	* everything is a value
	* pure functions (values go in and go out and nothing else happens outside)
	* no mutable state!
	* no effects!
* if all values go in, then we have **Components** (instance only depends on parameters)
	* is a value, has local reasoning and local testing; reusable
	* will still have a local state and may perform effects
	* may be good enough (AngularJS - directives, passing values around, services)
* React has purely functional components in last version (pure-isa; props -> virtual DOM)
* how do we handle (non-local) state?
	* you can encapsulate it in services
	* a special case of cross-component interaction? (events) => this approach scales
* Purifying Events?
	* problem with events is that it is implicit
	* Reactive components => make events explicit with Cycle.js (Component: [Observable] -> [Observable])
* Purifying state?
	* external with emitting events (Redux.js)
* problems: integrations and tooling still in early stages
* best practices are still being developed (community converging on Redux)