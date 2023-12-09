## Chapter 9. Unit tests

1. **The Three Laws of TDD**
   1. You may not write production code until you have written a failing unit test.
   2. You may not write more of a unit test than is sufficient to fail, and not compiling is failing.
   3. You may not write more production code than is sufficient to pass the currently failing test.

   <br/><div>These three laws lock you into a cycle that is perhaps thirty seconds long.
   The tests and the production code are written together,
   with the tests just a few seconds ahead of the production code.</div>

2. **Keeping Tests Clean**
- Don't allow your unit tests to become messy, because it's the same as the bad designed production code - 
it starts to cost more and more to debug and maintain. 

3. **Tests Enable the -ilities**
- The more you covered by unit tests,
the more flexible is your code, and you have less fear to do refactorings and improvements.

4. **Clean Tests**
- Use [Build-Operate-Check pattern](https://medium.com/swlh/usual-production-patterns-applied-to-integration-tests-50a941f0b04a)
  to refactor and simplify tests:
  - Build — phase in which you prepare the test scenario.
    In this phase you normally put some data in the database;
  - Operate — phase in which you execute the method on the object/API you are testing;
  - Check — phase in which you check whether the executed method has made an expected
    impact on the system. In this phase, you normally query the database and check
    the result of the method execution.

```java
public void testGetDataAsXml() throws Exception {
    makePageWithContent("TestPageOne", "test page"); //build
    
    submitRequest("TestPageOne", "type:data"); //operate
    
    assertResponseIsXML(); //check
    assertResponseContains("test page", "<Test"); //check
}
```
- Domain-Specific Testing Language: create classes and functions that wrap 
the test-specific logic to create some sort of testing framework (API) for your needs.
- A Dual Standard: The code within the testing API does have a different set of engineering standards
  than production code. It must still be simple, succinct, and expressive,
  but it need not be as efficient as production code. After all, it runs in a test environment,
  not a production environment, and those two environment have very different needs.
```java
//clean but difficult to read
@Test
    public void turnOnLoTempAlarmAtThreashold() throws Exception {
        hw.setTemp(WAY_TOO_COLD);
        controller.tic();
        
        assertTrue(hw.heaterState());
        assertTrue(hw.blowerState());
        assertFalse(hw.coolerState());
        assertFalse(hw.hiTempAlarm());
        assertTrue(hw.loTempAlarm());
    }

//short and clean, but breaks the conventions with StringBuffer approach
@Test
    public void turnOnLoTempAlarmAtThreshold() throws Exception {
        wayTooCold();
        assertEquals("HBchL", hw.getState()); //Upper case means "on", lower one means "off"
    }
```

5. **One Assert per Test**
- Common convention in testing says that "every test function in a JUnit (PHPUnit etc.) test should have one
  and only one assert statement", but it's debatable, especially when separating tests produces a lot of duplicated code.
- Single Concept per Test: a better rule is that we want to test a single concept in each test function. We don’t
  want long test functions that go testing one miscellaneous thing after another.

6. **F.I.R.S.T.**
- Clean tests follow five other rules that form the above acronym:
  - **Fast**. Tests should be fast. They should run quickly. When tests run slow, you won’t want
    to run them frequently. If you don’t run them frequently, you won’t find problems early
    enough to fix them easily.
  - **Independent**. Tests should not depend on each other. One test should not set up the conditions
    for the next test. You should be able to run each test independently and run the tests in
    any order you like. When tests depend on each other, then the first one to fail causes a cascade
    of downstream failures, making diagnosis difficult and hiding downstream defects.
  - **Repeatable**. Tests should be repeatable in any environment. You should be able to run the
    tests in the production environment, in the QA environment, and on your laptop while
    riding home on the train without a network. If your tests aren’t repeatable in any environment,
    then you’ll always have an excuse for why they fail. You’ll also find yourself unable
    to run the tests when the environment isn’t available.
  - **Self-Validating**. The tests should have a boolean output. Either they pass or fail. You
    should not have to read through a log file to tell whether the tests pass. You should not have
    to manually compare two different text files to see whether the tests pass. If the tests aren’t
    self-validating, then failure can become subjective and running the tests can require a long
    manual evaluation.
  - **Timely**. The tests need to be written in a timely fashion. Unit tests should be written just
    before the production code that makes them pass. If you write tests after the production
    code, then you may find the production code to be hard to test. You may decide that some
    production code is too hard to test. You may not design the production code to be testable.