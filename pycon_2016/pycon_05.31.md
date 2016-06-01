# PyCon 2016

## Tuesday - 31.5.

### Building Realtime Data Pipelines with Python and AWS Lambda - Mercedes Coyle
* realtime system, if it process events as soon as it receives them
* legacy data system: basically a batch
	* scheduled jobs are not inteligent
	* avoid logging to disk as a method of data collection (needless disk trashing)
	* mangled data
* new system: go serverless, but needs to:
	* allow for streaming analytics
	* data source and storage agnostic
	* reduce system complexity
	* flexibility
* check slide for new architecture
* API gateway:
	* quick and  easy to setup
	* publish HTTP interface or use API keys
	* can trigger lambda or go directly to Kinesis stream
* Kinesis streams
	* HTTP put single or batched records
	* 7 day data retention
	* multiple subscriber
	* horizontally scalable
* EMR (Elastic Map Reduce)
	* managed Hadoop cluster
	* spin up, process, destroy
* AWS lambda
	* event driven push/pull
	* scales up/down automatically
	* supports Python, NodeJS and Java
	* stateless and asynchronous
	* can call other lambdas (so you can stack them)
	* can also do scheduled lambdas...
* lambda: anatomy
	* event handler that takes two arguments: event and context object
	* context object is metadata about the running function
	* lambda functions don't know anything about previous events
	* automatic retry on failure
* any print or logging statement is logged to CloudWatch
	* also get some basic metrics dashboards
* testing: as it is standalone you can run unit tests against it by mocking out event
* packaging:
	* need to crate a zip file of function code and any dependencies
	* can pip install <module> -t /project-dir/ and zip contents of that directory
	* or you can install the contents of <virtualenv>/lib/python2.7/...
* deployment:
	* AWS CLI from Travis CI job
	* Cloud Formation template
	* upload to S3 and deploy via Lambda
* summary:
	* python 2.7 only
	* faster development cycles and data insights
	* code more for business goals, less for infrastructure
	* factor in maintenance and operational costs when pricing out


### Remote Calls != Local Calls, Graceful Degradation when Services Fail - Daniel Riti
* we are moving from monolithic to distributed which increases number of dependencies
* with this we are increasing points of failure
* what approaches support graceful degradation when services, networks, data stores fail?
	* timeouts
	* circuit breaker pattern
	* retries
	* bulkhead pattern
* will talk only about first three
* how should we degrade when the time_service giving timestamp is unavailable?
	* present Unavailable to user
	* give up on requests after 3 seconds
	* provide fault isolation
* your code can't just wait forever for a response that might never come sooner or late, it needs to give up. Hope is not a design method
* timeouts are not perfect
	* easy to get started with
	* provide some fault isolation
	* response bound to timeout value
	* still applying load to unhealthy service(s)
* circuit breaker pattern
	* it prevents using one system instead of retrying
	* states: open (doesn't let traffic through), close (lets it through) or half-open (allowing single exploratory request through to check health)
* timeouts + circuit breaker pattern
	* fail fast and rapidly recover
	* you can experience flapping if applied to under provisioned services
* retries
	* when should it be used? does the benefit of obtaining a response from a service outweigh potentially increasing load on the service?
	* limit the number of retries per request
	* introduce delay between retry attempts (like exponential back off or randomised jitter)
* combinatorial retry explosion: if multiple layers all perform retry strategy
* use clear response codes
	* separate retrial and non retrial errors
	* return a specific status when overloaded
* retry budgets
	* per-request retry budget
	* per-client retry budget
	* server-wide retry budget
* monitor your retry rates


### Structured Data from Unstructured Text - Smitha Milli
* a trained linguist can diagram 97% of sentences (Google can 94%)
* ran out of time because of problems so don't watch it again