## Chapter 11. Systems

1. **Separate Constructing a System from Using It**
    - Software systems should separate the startup process, when the application objects are 
      constructed and the dependencies are “wired” together, from the runtime logic that takes
      over after startup.
    - The startup process is a _concern_ that any application must address.
    - The _separation of concerns_ is one of the oldest and most important design techniques in our craft
    - Avoid lazy initialization, it can be hard to cover with tests and violates SRP:
   ```java
      public Service getService() {
         if (service == null)
            service = new MyServiceImpl(...); // Good enough default for most cases?
         return service;
      }
   ```
   - Separation of Main: One way to separate construction from use is simply to move all aspects of construction to
     main , or modules called by main , and to design the rest of the system assuming that all
     objects have been constructed and wired up appropriately (TLDR: move initialization into separate class or module).
   - Factories: abstract factory may help separate construction details from application code.
   - DI/IoC: Inversion of Control moves secondary responsibilities from an object to other objects that are dedicated
     to the purpose, thereby supporting the Single Responsibility Principle.

2. **Scaling Up**
   - Cross-Cutting Concerns: things like persistence framework tend to break domain boundaries,
     and it's acceptable when done fine.
   - "The agility provided by a POJO system with modularized concerns allows us to make optimal, 
     just-in-time decisions, based on the most recent knowledge. The complexity of these
     decisions is also reduced" (TLDR: more plain objects, less premature optimization).
   - Systems Need Domain-Specific Languages: "Domain-Specific Languages allow all levels of abstraction and all domains
     in the application to be expressed as POJOs, from high-level policy to low-level details."

_NOTE_: pure Java-related recommendations were skipped on purpose