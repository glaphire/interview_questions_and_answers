## Chapter 3. Functions

**1. Small!**
- functions should be as small as possible
- ideal max length for the function is one monitor (less than 100 lines, less than 150 characters per line).
- functions should not be large enough to hold nested structures.
- the indent level of a function should not be greater than one or two.

**2. Do One Thing**
- function is doing more than one thing if you can extract 
another one from it with a name that doesn't just describe a implementation.
- functions that do one thing cannot be reasonably divided into sections, so you need to extract them.

**3. One Level of Abstraction per Function**
- don't mix different levels of abstractions inside functions.
- *The Stepdown Rule* : Every line in a function is kept to the same level of abstraction,
one level below the name of the function.
- avoid switch statements, because they violate SRP and OCP from SOLID. Refactor it into Abstract Factory.
- use descriptive names for a function. Long names are acceptable as long as they are descriptive and easy to read.

**4. Function Arguments**
- Number of arguments:
  - Zero is the best.
  - One (monadic) and two (dyadic) are okay.
  - Three (triadic) are bad and should be avoided.
  - More than three (polyadic) requires special justification.
- The fewer arguments, the easier is to cover functions with tests.
- 'Events' as monadic functions without output args are quite common way to change the state of the system. 
- Avoid using 'Flag arguments', better create two separate functions.
- Be sure that both arguments in dyadic functions are justified, on same level of abstraction, and easy to keep in order.
- Sometimes instead of passing several arguments, you should pass an object that wraps them.
- Avoid creating side effects inside functions (when function does things that are not described in it's name).
- In general output arguments should be avoided. If your function must change the state
  of something, have it change the state of its owning object:
```java
appendFooter(s);
//vs
public void appendFooter(StringBuffer report);
//vs
report.appendFooter(); 
```

**5. Command Query Separation**
- functions should either do something or answer something, but not both. Either your
  function should change the state of an object, or it should return some information about
  that object.
```java
setAndCheckIfExists("username", "unclebob");
//vs
if (attributeExists("username")) {
   setAttribute("username", "unclebob");
}
```

**6. prefer exceptions to returning error codes**
- `Try/catch` blocks are ugly in their own right. They confuse the structure of the code and
  mix error processing with normal processing. So it is better to extract the bodies of the `try/catch`
  blocks out into functions of their own.
- a function that handles errors should do nothing else.

**7. Don't Repeat Yourself**
- avoid repeating of the logic, because it bloats the code and increases a chance of error.

**8. Structured Programming**
- Dijkstra said that every function, and every block within a function, should have one entry and one
  exit. Following these rules means that there should only be one `return` statement in a function, 
  no `break` or `continue` statements in a loop, and never, ever, any `goto` statements. TLDR: this rule is outdated.

