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


## Farewell and Welcome Home: Python in Two Genders (Naomi Ceder)
* disclaimer: opinions and life described are my own. I make no other warranties, either expressed or implied (common mistake when members of marginalised groups speak)
* did not want to give this talk, but was encouraged to do so
* transwitness protection program: they used to be required to completely abandon their previous identity completely
* in geeky groups there was far less pressure to perform masculine
* 41% of surveyed transponders have attempted suicide (only survivors could)
* she wanted to transition and still keep those things she valued
* people were supportive of transition and the more open she was the easier it was (for others as well as herself)
* the community's commitment to diversity is real
* it's not all rainbows and unicorns
    * almost always the only (openly) trans person in the room
    * she is not "real"
    * become a thing, a curiosity, a joke
    * people sometimes embarrassed to be seen with me
    * she doesn't have rights in many places (states in US, countries)
    * has a higher risk of violence and death (bulk of which is against transgender women of colour)
    * has lost friends, family, a career
* not invited to high school reunion out of 75 (the only other person serves in prison)
* she wouldn't change because she has seen things other haven't as both male and female, as both privileged and marginalised
* if you are marginalised, then you are needs are "special" and "extra"; never quite sure you're welcome
    * objecting to or even reporting something is "angry", hurting "your own cause", "bullying", a "witch hunt", or "asking for it"
* world is a very different place for marginalised that is painfully clear to marginalised and invisible to others
* what we need:
    * understand that everyone's different
    * listen (and believe them)
    * codes of conduct matter
    * outreach matters
    * allies matter (call out sexism etc. even in all-male groups)
    * safe spaces matter


## EuroPython Society stuff
* founded in 2004 in Goeteborg, Sweden to provide legal framework for the EuroPython conference series
    * managed location selection process and backed local organisers financially
* Berlin was picked in July 2013 (they were the only candidate)
* EPS purpose:
    * protect the IP behind EuroPython (trademarks, logos, social accounts)
    * select conference locations
    * provide services to local organisers
* the biggest problem was that transitions between locations never really worked out (a lot of reinventing the wheel, making same mistakes over and over again...)
* EuroPython has grown so expected conference quality and fun factor is high and conference needs to remain affordable and community friendly
* there were complaints this year about price!
* purpose of the EPS (2014 onwards)
    * decentralised work group model of organisation
    * work groups continue work across location changes: reduce loss of institutional knowledge
    * have the EPS enter the contracts, remove financial risk from the local team
    * local teams mostly for on-site support
    * for details, please see talk "EuroPython 2015" later on
* with these changes we hope to increase number of teams applying

### Organizing EuPy
* scalability is a **big** and real problem
* we still reinvent the wheel every ~2 years (and still discuss how to fix it)
* EuroPython **is** still a community conference
* current model is broken!
* why is it hard to scale with community?
    * local volunteers may not scale
    * knowledge loss
    * local organisers need a real organisation that can hold/mitigate the financial risk
* hence need/proposing a new structure
    * permanent workgroups
    * coordinated by the community and EPS from their definition, creation and their implementation
    * each workgroup will be responsible for the management of their related projects and tasks
    * any member of the EPS, EuPy community, EuPy workgroups or the Python community in general can make a proposal
    * the presented proposal will be evaluated by the related workgroup, the EPS and converted into real projects if accepted
* the WG structure
    * one WG chair person
    * WG members (numbers vary depending on WG)
* Teams
    * conference administration
    * finance
    * sponsors
    * communications
    * support
    * financial aid
    * marketing/design
    * program
    ... (distracted by a phone call :P)



## Elasticsearch meet up

### Demo of new ES python client
* new approach does not require creating deeply nested dict structures for ES queries; instead you can compose them using logical objects (e.g. Search, filter...)
* new client won't do validation and you will be able to create incorrect queries; reason for this is that it can work with different versions of ES because it doesn't want to guess which ES is used and what it supports
* the code is on github, but missing docs and proper exception handling ([elasticsearch-dsl-py](https://github.com/elasticsearch/elasticsearch-dsl-py))
* this client sits on the top of the new low-level one


### SQL on Elasticsearch?
* ES as primary storage?
* CRATE build SQL layer on top of ES
* [https://crate.io/](https://crate.io/)
* open source (paid support)
* also have python client called crate on PyPI
* Crate Data also adds BLOB storage, distributed accurate aggregations, partitioned tables, import/export, update by query, insert by query and integrated Admin-UI
* [Github](https://github.com/crate)
* does not work with Django yet because they don't do relations; you can probably use it as long as you can avoid that
* not targeted for lots of data (few millions of entries); intended use for env with lots of connections (because you can distribute work with ES cluster)
* by default they turn off ES api to protect data integrity
* ES by default automatically clusters so if you have two notebook on the same network and multicast on, they will cluster