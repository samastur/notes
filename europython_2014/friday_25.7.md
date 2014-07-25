# Friday, 25.7.

## Improving automated testing with pytest (holger krekel)
* Holgar also founded the PyPy project!
* py.test:
	* automatic test discovery, no-boilerplate test code
	* useful information when a test fails
	* cross-project, many options and plugins (Django's unittest extensions can't be used with some other projects)
	* modular and parametrisable fixtures
	* distribute tests to multiple hosts
* per-project contest.py with fixtures, hooks
* test discovery:
	* walks over the filesystem and discovers test_*.py test files
	* discovers test_ functions and Test classes
	* (also discovers unit test.TestClass deriving classes)
* automatic discovery avoids boilerplate
* py.test just use assert statements
	* py.test does not re-evaluate anymore to get values, it does AST transforms instead to store values so it can show them on failure (avoids side-effects)
* option --collect-only just collects them, -x stops on first failure
* different in py.test to unit test is that assertTrue on condition can tell you what parts of it were
* asserting expected exceptions: `with pytest.raises(ZeroDivisionError):`
* it has type specialised reporting (e.g. it can tell you at which index do string differ)
* mark functions or classes with mark objects: `@pytest.mark.<marker_name>`
	* `pytest -m codec_x` (run tests marked with codec_x)
	* `pytest -m "not codec_x"`
	* `pytest -m ""codec_x or codec_y`
* example uses:
	* marking slow tests so you can skip them when they are not important to run
* skip a test if (`pytest.skip()`):
	* skip tests that can not run on certain platforms
	* skip if dependency is missing
* xfail a test if (`pytest.xfail()`):
	* the implementation is currently lacking
	* it fails on a certain platform but should work
* don't skip everything because it's not telling that underlying code is not working correctly; use xfail when needed to communicate that something is failing that shouldn't, but is known to be doing so (can give reason why)
* `pytest.skip("reason")` for skipping inside of test or declaratively: `skipif = pytest.mark.skipif` and then `@skipif(sys.platform == "win32", reason="POSIX only")`
* you can use xfail as a TODO list (and add bug number as reason)
* test fixture:
	* sets up object or apps for testing
	* provides tests code with "base" app objects
	* very important to avoid repetitive test code
* in py.test realised via dependency injection:
	* fixture functions create and return fixture values
	* test functions and classes can request and use them
* injected dependency is in general a good way of constructing code to test (it shifts creation responsibility to the caller)
* if you skip in fixture, then **all** tests that use that fixture will automatically be skipped
* you can specify what fixture provide in its doctoring which can be displayed by using option `--fixture` on your tests (it notifies you if doctoring is missing)
* fixtures:
	* are returned from a fixture function
	* each fixture has a name (the function name)
	* test functions get it injected as an argument by name
	* can be cached on per scope bases (call and teardown phases are called with every call, but setup phase depends on scope)
	* fixture functions can add finalisers
	* fixture functions can be parametrised
* fixture functions can accept a special `request` object and use `request.addfinalizer` to register teardown functions which will be called after a test function finished
	* so if you create a fixture with scope of a session and add a finaliser to request in it, then that function will be called once after all tests have finished
* `pytest.yield_fixture` another way of adding finaliser (by having finalised's code be run after yield statement)
* parametrised fixtures: a way to run same test multiple times with different parameters
	* `@pytest.fixture(params=[MySQL(), Postgres()])` would result in two test runs, one for MySQL and one with Postgres
* **aside**: `assert 0, db` it will show what db is (Python thingie) useful for seeing what gets in when you don't know
* you can also directly parametric test using `@pytest.mark.parametrize("params", [param_values])`
	* `@pytest.mark.parametrize("arg1,arg2", [(1, 1), (2, 3)])` would provide arg1 and arg2 with first tuple and then second tuple