# WebCamp Ljubljana 2015

## When do programmers work for free?, Filip Muki Dobranić
* danesjenovdan.si
* talk based on theory, but also on speaker's personal experience
* made bunch of applications: send yourself in the future an email to remind you what an asshole Slovenian president is (by neutering KPK), argument (http://agrument.danesjenovdan.si/),...
* institute that he belongs to will develop Slovenian theyworkforyou in next few months
* a lot of what he talks about is based on marxist thought, but he wants to destroy the notion that it has nothing to say and is equal to communism in Russia
* humans the only animal without ecosystem of its own: we have to work anywhere to create our home (??)
* what is (surplus) value? (needed to discuss work for free)
	* the only thing that creates value is physical stuff (and we add value by working with it)
	* if the only way to create value is through work, then those who don't take value from somebody else
	* with industrialisation we moved away from directly valued work and started to sell our hours in exchange for money
	* profitability improvements are ways of squeezing more work from people in same amount of time (unpaid more labor)
	* profits are based on free labor (??)
* unpaid work in 21st century
	* companies vs. households (eating is free labor needed to perform work)
	* Facebook (another dubious example)
	* volunteerism (another bad example)
	* internships (talks about free ones; exclude those who can't afford to work for free)
	* precarious work (freelancing; form of free labor?)
* work and pleasure
	* pleasure is that which has no purpose
	* indolence?
	* what happens, when pleasure is the goal?
	* jouissance (French word for pleasure used by Lacan; also describes the feeling of pleasure once you reach orgasm)
* laziness is something that is sold as something we should feel guilty about, but not pleasure (also says meaning and value of laziness hasn't changed through history)
* the science of motivation
	* if performance is tied to pay, their performance drops
	* solve it by giving enough money to put the question out of their heads (overpaying them)
	* even that is not solution; developers often give up once they conquer the hard part (not interesting anymore)
* open source
	* two different types:
		* investment in future (libraries to save time in future)
		* fun stuff you do over weekend
* in conclusion
	* politika (governing of everyday affairs)
	* (programmers) should never work (for free)
	* universal income


## (R)evolution(s) in Linux Distributions, Domen Kožar
* interested in Linux since high school
* devops also a new way of thinking about servers
* Linux distress that aren't
	* Gobo Linux: the filesystem is the database: each program resides in its own directory
	* Bedrock Linux: created with aim of making most of the benefits of various other Linux distributions available simultaneously and transparently (so you can e.g. install stuff from RedHat and Debian)
	* NixOS: you describe your system declaratively in one file (which packages to install, how to setup firewall...); Domen is its release manager
	* Qubes OS: security-focused desktop operating system that aims to provide security through isolation with Xen
* Containers: the end of the general purpose operating system
	* Docker compose
	* RancherOS: a minimalist distribution of Linux designed from the ground up to run Docker containers
* CoreOS: Gentoo -> Chrome OS -> Core OS
* RedHat (project atomic)/Ubuntu (snappy ubuntu): using containers to provide updated atomically and rolled back if needed
* Unikernels: operating system for cloud (Erlan on Xen, OSv, MirageOS)
	* next step from containers trying to minimise their sizes
* Mesos from Apache (not sure what it is; check)


## It's my way or the highway. How to make your code usable by others, Bogdan Habić
* OO: encapsulation, inheritance, polymorphism
* some examples of awful code
* reasons:
	* cowboy coding
	* tight deadlines
	* "I'll refactor this later"
* how to fix this?
	* you won't refactor later!
* he thinks optimising early makes it more readable and reusable because you think about it more
* DRY, SOLID principles (can't make architectural mistakes if you use them correctly), design patterns
* what makes a piece of code: "nice to use"?
	* easy to implement (shouldn't take more time to use lib than write one your own)
	* it's eloquent (source should be readable)
	* works like magic
* how to make magic (happen)?
	* the developers should not care about the internals (should not need to open source code to use it)
	* the code should "lead" the developer (should not be required to read pages of docs for hello world)
	* it should be modular (should be able to change how parts work without changing its source code)
* interfaces, abstract classes, traits and exceptions
* Strategy pattern and Command handler patterns
* Interfaces
	* contract good metaphor
	* known parameter types, known return types and it's self explanatory
* Abstract classes: looks at them like an interface
	* very PHP specific example
* Traits: great when adding functionality to existing class
	* example of metaprogramming with PHP to do this
* Exceptions: it's OK to break someones code
* Strategy pattern: lets changing behaviour of the code (he creates constructor with null value so developers can change its behaviour)
* Command Handler patterns: data objects with attributes and handlers with <attribute>_handler for mutation of those objects


## Using SASS in web development, Vladan Mitevski
* short intro to main SASS features
* @include (mixins) vs @extend
	* use mixins when CSS properties are same but values are different - add variables for values
	* use placeholders when both CSS properties and values are the same (@extend)
* best practices for writing SASS
	* import @extend first
	* import mixins second
	* import regular declarations third
	* use nested selectors at the end and mo more then 3 levels of nesting
	* media queries as closest to their relevant rules as possible
* how SASS can mess up CSS
	* repeating code when not using mixins proper way
	* using too much nesting can create very messy CSS (slow, hard to override)
	* importing partials can mess up organising CSS (not sure how)
* always have in mind how CSS will be compiled
