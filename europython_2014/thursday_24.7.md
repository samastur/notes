# Thursday, 24.7.

## Conversing with people living in poverty Novice (Simon Cross)
* avoids speaking of people as poor because it sounds like an innate condition
* works for non-profit Praekelt and is the lead engineer on Vumi that also has office in London
* Vumi is a text messaging system designed to reach out to those in poverty on a massive scale via their mobile phones (using Python and Twisted)
* messaging really has taken off
* poverty: lack of basic capacity to participate effectively in society (UN definition)
    * likes because it highlights isolation not lack of money
* Africa: 3x the size of Europe and 3/4 the size of Asia (1 billion people)
* 1000+ languages so i18n and l10n are important
* in 1994 Nigeria had 100 million people and 100k land lines and 0% internet access; even if you had access to phone you may not know who to call and the nearest government official could be 500km away (and the problem goes the other way too)
    * in that year the first mobile phone network launched in Africa
* 2012: mobile penetration 65% in Africa (in Uganda more mobile phones than lightbulbs)
    * mobile generation (mobiles are their laptops..)
* in early days they were consulting company which started with service call txt alert
* one of the exciting things of working in Africa is that failures are often not exceptions, but rule
* Vumi is a messaging engine attempting to provide reusable messaging framework ready for integration with different mobile operators
* [https://github.com/praekelt/vumi](https://github.com/praekelt/vumi)
* uses riak for data storage (riakasaurus - Twisted riak client)
* in Kenya they worked on project called Sisi Ni Amani where "peace officers" with mobile phones could notify if they were observing violence breaking out
* Wikipedia Zero: free access to Wikipedia over mobile phones (sending text over SMS messages)
* lowering barriers to entry:
    * connectivity is expensive
    * machines are expensive
    * we can't code everything ourselves
* help people solve their own problems! (they provide infrastructure they can use)
* vumi: in last year 7 million interacted with it and sent 44 million messages
* bigger plans:
    * Africa CDN
    * federated instant messaging (protocol)


## The Return of "The Return of Peer to Peer Computing"­. (Nicholas Tollervey)
* AIM: to create a context in which **you** may think about peer-to-peer computing

### Part 1: Motivations
* Edward Snowden (Nicholas was at Guardian when they were breaking stories)
* moral panic after revelations
* those who say privacy is dead are the ones that gain the most from surveillance
* William Hague clip ends:"...you won't be aware of all the things these agencies are doing."
* WRONG! it's false dichotemy
    * it's not **you** who determines if you have anything to hide (example academics, politicians, activists...)
    * it assumes surveillance results in correct data and sound judgement (example: obvious Twitter joke that led to imprisonment for a while)
    * rules and governments change
    * breaking the law isn't necessarily bad (Gandhi, Mandela, Socrates, Emily Pankhurst)
    * privacy is a fundamental human right (UN declaration on human rights)
* programming **is** politics (we're asking and answering questions about organisation, process, power and control)

### Part 2: Questions
* last year Holgar Krekel asked:
    * what digital world do I want to live in?
    * what software do I want to create?
    * what legacy do I live to my children?
* conclusion: is peer-to-peer and ubiquitous cryptography a way to address the concerns over power and control of digital world
* will skip crypto in this talk
* server can decide what it allows you to see and is a single point of failure
* sometimes hierarchy is good (especially when it is efficient and saves lives)
    * ideally those in power earned it
* authority derived from architecture is bad

### Part 3: What can we do
* we had sprints at which we grew community interested in:
    * re-decentralisation of the internet
    * promoting non-surveilled communication
    ...
* what are the fundamental elements of a secure P2P system and what can we do to build it?
* organise! at conferences and gatherings like this one
* prototype!

### Part 4: Outcomes
* prototypes and hacks
    * P2P cryptographic messaging (P2PCM)
    * Universal DHT (as a platform) (distributed hash table) so that any app could store data there
* P2PCM is a test card - if we solve this, then a lot of stuff is solved
* problem: secure decentralised message delivery to offline peers
* DHT could be used to signal when somebody is online or offline (discoverability and signalling)
* P4P2P - DHT within DHT (or namespaced/scoped DHT)

* what about economics? how is development funded? (fun work with people sharing your values)
* food/drink and discussion 17.30 in the foyer


## Elasticsearch from the bottom up (Alex Brasetvik)
* co-founder of Found AS - hosted hundreds of elasticsearch sites (3+ years of experience with ES)
* motivation:
    * why isn't my search for *foo-bar* matching 'foo-bar'?
    * why is it using so much memory?
* when working with ES you work with cluster with nodes; shard is basically a lucene index which can span more than one node
* lots of ES docs assume familiarity with Lucene
* within Lucene index you have segments which contain data structures like inverted index of terms, frequencies and values
* prefix searches are cheap so you all searches try to be converted to them* 
* when you work with search text processing is really important
* **WATCH VIDEO OF THIS TALK** (this notes are very much incomplete)
* field cache is loaded into memory if not specified otherwise which makes it fast, but uses tons of it
* lucene segments are immutable (lucene searches over them and merges results); when you delete it there's a bitmap that marks it deleted so it can be filtered out, but it is never changed (remains stored)
* lucene compresses everything and is very good at it; also caches everything
* adding documents may make index smaller when it triggers merging of segments and removing deleted data
* searching shards is pretty much same as searching segments; you search all and then ES merges results
* sharding and partitioning into separate indexes is similar; if you search over multiple shards or indexes it performs essentially the same operation
* if you store logs in ES, then it's best to put each day into its own index (removing is simply removing index and performance of one doesn't affect directly other)
* you can't split shard! (careful when you plan how to scale)
* the cluster also has cluster state that is replicated over all nodes like mappings (schema for index), routing table...
* when you don't get results you expect, text processing should be your first suspect
* queries are not cached like filters because they are scored (how well it matches)
	* when possible use filters and use queries only when you need scoring
* talk based on article on [foundation section of their website](http://found.no/foundation)
* having things in multiple shards can yield a different result than if everything is stored in one; it's also slower so you should prefer fewer of them


## Lessons learned from building Elasticsearch client (Honza Král)
* were not happy with existing clients because they all lacked something
* ES: distributed, REST API
	* different deployment environments
	* 96 API endpoints
	* 672 parameters
* what clients? multiple languages and they wanted them to be true to their language
* no opinions! (so there would be no excuses to using it)
	* low level
	* extensible/modular so you can change parts you don't like
* examples of how to use your own selector class or serializer
* it is very difficult to make a design that works for different languages so they created a prototype (spike) for python in ruby to explore this
* don't send a man to do a machine's job (like achieving consistency)
	* you don't want to manually track ~100 APIs with ~700 parameters
* they tried reference implementation for APIs, but it doesn't really scale well (they only have one person per language)
* next they looked at documentation; no go either
* they extracted API definitions:
	* machine parseable (JSON)
	* human readable (JSON)
	* pre-generated from Java (static languages FTW)
* documented:
	* URL path (including dynamic parts)
	* HTTP methods
	* Query parameters
	* Body descriptions
* they didn't capture all the dependencies (this is valid only with that...); they didn't want to handle this in client
* rest-api-spec is part of ES repo and is used in main test suite
	* serves as reference for developers
	* scripts to generate clients
* test everything!
* created unified test suite that enabled code to run also against all the clients
	* running as part of Java test suite
	* version specific
	* integration testing
* when they needed to write the 5th client, they only needed few weeks for that
* next stage will be writing more opinionated clients