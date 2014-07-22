# Tuesday, 22.7.

## Will I still be able to get a job in 2024 if I don't do TDD? (Emily Bache)
* what actually saying is will she be able to get a job that she want!
* Allan Kelly predicts by 2022:
	* programmer who don't routinelsy practice TDD will be unemployable
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
* talks about how changin running shoes to thin, five-finger ones she also needed to change style
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
* visualizing TDD: http://cyber-dojo.org
* practice helps with:
	* experience what TDD feels like when it works
	* recognize problems well suited to it
* for a lot of production code TDD is really hard to do; you need to be able to adapt to your local conditions (web layer, databases,...)
* she thought about TDD in more general sense:
	* work incrementally (might be bigger steps)
	* design has testable units that might be bigger
	* design API before implementation?
	* self-testing code may be useful even if tests take longer
* Test-Driven Development with Python by Harry J.W. Percival (recomended by Emily)
* what if conditions are unfavorable to TDD?
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
	* spike & stabilize: refactor your prototype into useful stuff (not TDD!)
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
