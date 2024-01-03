## Chapter 12. Emergence

1. **Getting Clean via Emergent Design**
    - Simple design rules by Kent Beck:
        - Runs all the tests
        - Contains no duplication
        - Expresses the intent of the programmer
        - Minimizes the number of classes and methods
      
2. **Rule 1: Runs All the Tests**
   - Systems that aren’t testable
     aren’t veriﬁable.
   - Tight coupling makes it difficult to write tests. The more tests we write,
     the more we use principles like DIP and tools like dependency injection, interfaces, and
     abstraction to minimize coupling.

3. **Simple Design Rules 2–4: Refactoring**
   - The fact that we have tests eliminates the fear that cleaning up the code will break it.
   - No Duplication: The TEMPLATE METHOD pattern is a common technique for removing higher-level
     duplication (separating classes into step-blocks helps visualise duplication).
   - Expressive: follow and apply names and approaches that are understood by everyone inside industry.
   - Minimal Classes and Methods: High class and method counts are sometimes the result of pointless dogmatism. 
     Consider a coding standard that insists on creating an interface for each and every class. 
     Or consider developers who insist that fields and behavior must always 
     be separated into data classes and behavior classes. Such dogma should be resisted and a more
     pragmatic approach adopted.