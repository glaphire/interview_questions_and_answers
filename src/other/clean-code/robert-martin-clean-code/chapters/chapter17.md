## Chapter 17. Smells and Heuristics

1. **Comments**
   - ___C1: Inappropriate information.___ It is inappropriate for a comment to hold information 
     better held in a different kind of system such as your source code control system, 
     your issue tracking system, or any other record-keeping system.
   - ___C2: Obsolete Comment___. A comment that has gotten old, irrelevant, and incorrect is obsolete.
   - ___C3. Redundant Comment___. A comment is redundant if it describes something that adequately describes itself.
   PHPDocs without any additional information are also obsolete.
   - ___C4. Poorly written comment.___ A comment worth writing is worth writing well.
   - ___C5. Commented-Out Code.___ That code sits there and rots, getting less and less relevant with every passing day.
   The only acceptable reason is super quick hotfix, but it will be fixed soon (_from personal experience_).

2. **Environment**
    - ___E1. Build requires more than one step___. Building a project should be a single trivial operation. 
    You should not have to check many little pieces out from source code control (at least minimize the steps for PHP apps).
    - ___E2: Tests Require More Than One Step___. You should be able to run all the unit tests with just one command.

3. **Functions**
    - ___F1: Too Many Arguments___. More than three is very questionable and should be avoided with prejudice.
    - ___F2: Output Arguments___. Output arguments are counterintuitive (_Not relatable to PHP apps_).
    - ___F3: Flag Arguments___. Boolean arguments loudly declare that the function does more than one thing.
    - ___F4: Dead Function___. Methods that are never called should be discarded.

4. **General**
    - ___G1: Multiple Languages in One Source File___. 
    The ideal is for a source file to contain one, and only one, language (not a mix of PHP, JavaScript, HTML and other).
    - ___G2: Obvious Behavior Is Unimplemented___. Following “The Principle of Least Astonishment” 
      any function or class should implement the behaviors that another programmer could reasonably expect.
    - ___G3: Incorrect Behavior at the Boundaries___. Look for every boundary condition and write a test for it.
    - ___G4: Overridden Safeties___. It is risky to override safeties. Exerting manual control over serialVersionUID may be
      necessary, but it is always risky. Turning off certain compiler warnings (or all warnings!)
      may help you get the build to succeed, but at the risk of endless debugging sessions. Turning off failing tests and telling yourself you’ll get them to pass later is as bad as pretending
      your credit cards are free money.
    - ___G5: Duplication___. The most obvious form of duplication is identical or very similar pieces of code.
      A more subtle form is the _switch/case_ or _if/else_ chain that appears again and again in
      various modules, always testing for the same set of conditions. These should be replaced
      with polymorphism.<br/>
      Still more subtle are the modules that have similar algorithms, but that don’t share
      similar lines of code. This is still duplication and should be addressed by using the 
      Template Method or Strategy Pattern.
   <hr/>

    - ___G6: Code at Wrong Level of Abstraction___. It is important to create abstractions that separate higher level general concepts from lower
      level detailed concepts. For example, constants, variables, or utility functions that pertain only to the detailed
      implementation should not be present in the base class. The base class should know nothing about them.
    - ___G7: Base Classes Depending on Their Derivatives___. The most common reason for partitioning concepts 
      into base and derivative classes is so that the higher level base class concepts can be independent of 
      the lower level derivative class concepts. Therefore, when we see base classes mentioning the names of their 
      derivatives, we suspect a problem. In general, base classes should know nothing about their derivatives.
    - ___G8: Too Much Information___. A well-defined interface does not offer very many functions to depend upon,
      so coupling is low. A poorly defined interface provides lots of functions that you must call, so coupling is high.
    - ___G9: Dead Code___. Dead code is code that isn’t executed. Delete it.
    - ___G10: Vertical Separation___. Variables and function should be defined close to where they are used. 
      Local variables should be declared just above their first usage and should have a small vertical scope. We
      don’t want local variables declared hundreds of lines distant from their usages.
   <hr/>

    - ___G11: Inconsistency___. If you do something a certain way, do all similar things in the same way. This goes back
      to the principle of least surprise. Be careful with the conventions you choose, and once chosen, 
      be careful to continue to follow them.
    - ___G12: Clutter___. Of what use is a default constructor with no implementation? All it serves to do is clutter
      up the code with meaningless artifacts. Variables that aren’t used, functions that are never
      called, comments that add no information. All these things are clutter and
      should be removed.
    - ___G13: Artificial Coupling___. Things that don’t depend upon each other should not be artificially coupled. 
      For example, general _enums_ should not be contained within more specific classes because this forces the
      whole application to know about these more specific classes. The same goes for general
      purpose static functions being declared in specific classes. <br/>
      An _artificial coupling_ is a coupling between two modules that serves no direct purpose.
      It is a result of putting a variable, constant, or function in a temporarily
      convenient, though inappropriate, location. This is lazy and careless.
      Take the time to figure out where functions, constants, and variables ought to be
      declared.
    - ___G14: Feature Envy___. The methods of a class should be interested in the variables and functions of the class
      they belong to, and not the variables and functions of other classes. 
      When a method uses accessors and mutators of some other object to manipulate the data within that object,
      then it envies the scope of the class of that other object.
      It wishes that it were inside that other class so that it could have direct access to the
      variables it is manipulating.
    - ___G15: Selector Arguments___. There is hardly anything more abominable than a dangling _false_ argument at the end 
      of a function call. What does it mean? What would it change if it were _true_? Not only is the
      purpose of a selector argument difficult to remember, each selector argument combines
      many functions into one. Selector arguments (also known as flag arguments) are just a lazy way to avoid 
      splitting a large function into several smaller functions.
    <hr/>

    - ___G16: Obscured Intent___. Run-on expressions, Hungarian notation,
      and magic numbers all obscure the author’s intent. Avoid this approaches.
    - ___G17: Misplaced Responsibility___. The principle of the least surprise comes into play here. 
      Code should be placed where a reader would naturally expect it to be. 
      The PI constant should go where the trig functions are declared. 
      The OVERTIME_RATE constant should be declared in the _HourlyPayCalculator_ class.
    - ___G18: Inappropriate Static___. You should prefer nonstatic methods to static methods. When in doubt,
      make the function nonstatic. If you really want a function to be static, make sure that there
      is no chance that you’ll want it to behave polymorphically.
    - ___G19: Use Explanatory Variables___. One of the more powerful ways to make a program readable is to break 
      the calculations up into intermediate values that are held in variables with meaningful names.
    - ___G20: Function Names Should Say What They Do___.
    <hr/>

    - ___G21: Understand the Algorithm___. Before you consider yourself to be done with a function, 
      make sure you understand how it works. It is not good enough that it passes all the tests. 
      You must know 10 that the solution is correct.<br/>
      Often the best way to gain this knowledge and understanding is to refactor the function 
      into something that is so clean and expressive that it is obvious how it works.
    - ___G22: Make Logical Dependencies Physical___. If one module depends upon another,
      that dependency should be physical, not just logical.
      The dependent module should not make assumptions (in other words, logical dependencies) 
      about the module it depends upon. Rather it should explicitly ask that module for all
      the information it depends upon.
    - ___G23: Prefer Polymorphism to If/Else or Switch/Case___. Use the following “ONE SWITCH” rule: 
      _There may be no more than one switch statement for a given type of selection. 
      The cases in that switch statement must create polymorphic objects that take the place 
      of other such switch statements in the rest of the system._
    - ___G24: Follow Standard Conventions___. 
    - ___G25: Replace Magic Numbers with Named Constants___.
   <hr/>

    - ___G26: Be Precise___. Expecting the first match to be the only match to a query is probably naive. 
     Using floating point numbers to represent currency is almost criminal. Avoiding locks and/or transaction
      management because you don’t think concurrent update is likely lazy at best. 
      Declaring a variable to be an _ArrayList_ when a _List_ will fit is overly constraining. 
      Making all variables protected by default is not constraining enough.
      When you make a decision in your code, make sure you make it precisely.
    - ___G27: Structure over Convention___. Enforce design decisions with structure over convention. 
      Naming conventions are good, but they are inferior to structures that force compliance.
      For example, switch/cases with nicely named enumerations are inferior to base classes with abstract methods.
      No one is forced to implement the switch/case statement the same way each time; 
      but the base classes do enforce that concrete classes have all abstract methods implemented.
    - ___G28: Encapsulate Conditionals___. Boolean logic is hard enough to understand without having to see 
    - it in the context of an _if_ or _while_ statement. Extract functions that explain the intent of the conditional.
    ```java
    if (shouldBeDeleted(timer)) //better
   //VS 
   if (timer.hasExpired() && !timer.isRecurrent())
   ```
   - ___G29: Avoid Negative Conditionals___. Negatives are just a bit harder to understand than positives. 
   - So, when possible, conditionals should be expressed as positives.
    ```java
   if (buffer.shouldCompact()) //better
   //VS
   if (!buffer.shouldNotCompact())
   ```
   - ___G30: Functions Should Do One Thing___. (another SRP rephrasing).
   <hr/>

   - ___G31: Hidden Temporal Couplings___. Temporal couplings are often necessary, but you should not hide the coupling.
     Structure the arguments of your functions such that the order in which they should be called is obvious. 
     Enforce order by proper output+input chaining or hide the order in private methods.
   - ___G32: Don’t Be Arbitrary___. Have a reason for the way you structure your code, and make sure that reason is 
     communicated by the structure of the code. If a structure appears arbitrary, others will feel empowered
     to change it. If a structure appears consistently throughout the system, others will use it
     and preserve the convention.
   - ___G33: Encapsulate Boundary Conditions___.
     Boundary conditions are hard to keep track of. Put the processing for them in one place.
     Don’t let them leak all over the code.
   - ___G34: Functions Should Descend Only One Level of Abstraction___.
   - ___G35: Keep Configurable Data at High Levels___. 
   - ___G36: Avoid Transitive Navigation___. In general. we don’t want a single module
     to know much about its collaborators. More specifically, if A collaborates with B, and B collaborates with C, 
     we don’t want modules that use A to know about C . (For example, we don’t want `a.getB().getC().doSomething();`.).
     This is sometimes called the Law of Demeter.<br/>
     Rather we want our immediate collaborators to offer all the services we need. 
     We should not have to roam through the object graph of the system, hunting for the method we
     want to call. Rather we should simply be able to say: `myCollaborator.doSomething()`.