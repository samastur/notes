# Tuesday, 22.7.

## Will I still be able to get a job in 2024 if I don't do TDD? (Emily Bache)
* what actually saying is will she be able to get a job that she want!
* Allan Kelly predicts by 2022:
	* programmer who don't routinely practice TDD will be unemployable
	* it will not be acceptable to question TDD or claim TDD "unprovable"
* is this right? (she sees it as disruptive technology)
* TDD in narrowest sense:
	* a test must drive every line of production code
	* completely isolated units
	* design in tiny increments
	* you end up with hundreds of tests that will run in less than a second
* wonderful feeling of freedom & productivity
* she sees early adopters using this kind of TDD in 2014, not mainstream
* Kent Beck stopped developing tool for TDD in 2009 because he thought global market for this kind of tool was only few thousand people
* talks about how changing running shoes to thin, five-finger ones she also needed to change style
* that change didn't last because of Swedish autumn (cold feet!); she bought new shoes
* her two points:
	* early adopters like to stick out
	* will try technology without much proof of advantages
* to reach the mainstream TDD needs:
	* adaptions to become more practical
	* to do this without losing the essential benefits
* Essential benefits:
	* tests give design feedback
	* self-testing code that enables refactoring
	* wonderful feeling of freedom & productivity
* Code Kata - original idea by Dave Thomas: http://codekata.pragprog.com
	* helps experience TDD in ideal conditions
* more fun in coding dojo: coding with others
* what happens in the dojo?
	* write code, tests, collaborate, discuss
* she wrote a book on this topic (for those interested in finding more)
* visualising TDD: http://cyber-dojo.org
* practice helps with:
	* experience what TDD feels like when it works
	* recognise problems well suited to it
* for a lot of production code TDD is really hard to do; you need to be able to adapt to your local conditions (web layer, databases,...)
* she thought about TDD in more general sense:
	* work incrementally (might be bigger steps)
	* design has testable units that might be bigger
	* design API before implementation?
	* self-testing code may be useful even if tests take longer
* Test-Driven Development with Python by Harry J.W. Percival (recommended by Emily)
* what if conditions are unfavourable to TDD?
* problems not addressed by classic TDD:
	* I don't know what the user needs
	* I've never built anything like that before
	* I have lots of existing code with no tests
* Requirements?
	* double-loop TDD (inner loop of TDD should be augmented with acceptance test that needs to be written together with users)
	* acceptance test discovered through discussion with users
	* hopefully building what the user needs
* Never built anything like that before:
	* TDD is not going to lead you to the solution if you don't know how to solve the problem
	* spike solution (really just prototyping in limited time) to get to know your tools and problem
	* when you know enough start from scratch with TDD
	* spike & stabilise: refactor your prototype into useful stuff (not TDD!)
* Legacy code:
	* Working effectively with legacy code by Michael C. Feathers (should have called it Unit Test Anything)
	* it's really hard work because you have to take risks and you might make wrong choices (bake in bad design)
	* Unit Tests don't find integration errors
	* better start with Full-System or Integration Tests
* Approval Testing (she's one of the innovators of this technique)
	* how to check for correct behaviour?
* you don't write assert statements; you create input, check manually output for correctness and then use that as a test
* if she wrote assert statements, she might not write them for edge cases and these kind of tests also catch some of those errors
* you don't get all the benefits of TDD with these kind of testing (not design API before implementation and not testable units), but also has advantages (you can test spaghetti code)
* she thinks TDD hasn't crossed the chasm and she thinks it is better to think of it in more general sense which she thinks has already crossed the chasm to early majority
* it will become accepted that every piece of code ships with self-testing code
* benefits of working incrementally will also cross
* using tests to guide the design is something she is not sure will ever cross the chasm (because it might really work in some situations, but be extreme in others and really depends on developer's personality)
* you'll get a job in 2024 if you will write self-testing code, work incrementally & design well


## Statistics 101 for System Administrators (Roberto Polli)
* modules: scipy, matplotplib
* problem: "an episodic latency issue: what happened?"
* example on how to use the above modules to process and plot distribution
* uses Pearson's formula to calculate if two data series are related and you use scatter plot to see the result
* this approach does not find non-linear correlation!


## The Sorry State of SSL (Hynek)
* using "military grade encryption" does not make us safe
* only link: http://ox.cx/t (everything is there: talk and slides)
* SSL & TLS:
	* 1995: SSL 2.0
	* 1996: SSL 3.0 (still Netscape)
	* 1999: TLS 1.0, IETF
	* 2006: TLS 1.1
	* 2008: TLS 1.2
* browsers supported only TLS 1.0 until 2013 when they finally added TLS 1.2 (just using TLS is not enough)
* TLS provides:
	* identity (certificates)
	* confidentiality (encryption of the network traffic)
	* integrity (detect if something bad happens to TLS records)

### Servers
* OpenSSL >= 1.0.1c
* Apache >= 2.4.0
* nginx > 1.4
* be up to date
* trust chain important for client to verify that it can trust; missing trust chain is often mistake when setting up https
* disable SSL 2.0 and 3.0 if you can (WinXP needs it)
* TLS compression

#### Cipher suites
* cipher is algorithm used for encrypting data and most of them are block oriented (blocks of data that sometimes needs to be padded to fit block)
* CBC
* stream ciphers
* GCM 
* prefer this: AES128-GCM and ChaCha20
* need to fallback to AES128-CBC for some browsers (terrible on TLS1.0)
* ancient clients: 3DES-CBC
* none other is needed
* don't trust EXP-* (backdoored by design), DES or RC4 (broken in realtime by NSA)

#### Key exchange
* RSA (fast but leaking key makes possible decrypting everything that was ever sent using it)
* DHE (Diffie-Hellman): slow, but safer
* ECDHE (fast and safer than RSA)
* preferred use ECDHE -> DHE -> RSA

#### Integrity
* MACs (message authentication code)
* HMAC
* GCM

**Have the last word (prefer your ciphers to client's).**

You are done! (but test your results!)

### Clients
* only one job: verify (valid? trustworthy chain? correct hostname/service?)
* install CA trusted databases before you're trying to use them:
	* debian/Red Hat: ca-certificates
	* OSX: TEA or homebrew
	* Windows: wincertstore
	* or Mozilla/certifi
* Hostname verification:
	* OpenSSL to developers: LOL (considered out of scope)
* not verifying trust chain: I can pretend to be Google with any self-signed certificate
* not verifying hostname: I can pretend to be Google with any **valid** certificate
* **verify all things!**
* set some options: set acceptable ciphers and disable SSL 2.0
* that's all!

### Users
* Fundamental misconceptions:
	* no end-to-end security
	* metadata (DNS...)
* VPN?
	* provider sees **all** your traffic
	* same for CDN
* certificate warnings: read them (if expired 5 minutes ago it's mostly ok)
	* decline if you don't fully understand what is going on
* root certificate poisoning
* trust issues (trust database usually is not under users control)
* CAs can be abused by governments, but they are also hacked, screw up, cooperate with court orders...
	* the whole system is just broken

**Rule of thumb: don't do it yourself if you can help it.**

### Python
* standard library vs. PyOpenSSL
* std lib:
	* terrible pre-3.3
	* very incomplete in 2.7 (PFS impossible to write before 3.3)
	* missing options
	* bound to Python's OpenSSL
* Hostname verification (missing the part from slide; **check it**)
* PyOpenSSL (2.6+, 3.2+):
	* more complete API coverage 
* cryptography.io
	* Python crypto w/o foot guns
	* PyCA
	* PyPy (heart) CFFI
	* gives pyOpenSSL momentum
	* but no hostname verification
	
#### Libraries & frameworks
* event let, gevent, genitor, Tornado insecure (no PFS, no good defaults, not configurable)
* Twisted used pyOpenSSL and has all of the above
* uWSGI: bad defaults but otherwise OK (own C code so trust?)
* Tornado clients better (verifies certificates and hostnames)
* Twisted depends (stuff is there, but does not do everything by default)
* don't use urllib2, use urllib3/requests (verifies certificates, hostnames and has good defaults)

### Summary
* keep TLS out of Python if you can
* use pyOpenSSL-powered requests for HTTPS
* write servers in Twisted
* use pyOpenSSL

## Message-passing concurrency for Python (Sarah Mount)
Interesting talk, but I couldn't take notes because I was standing. Worth checking out online.