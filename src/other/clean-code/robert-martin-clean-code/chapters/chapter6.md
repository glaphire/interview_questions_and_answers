## Chapter 6. Objects and Data Structures

1. **Data Abstraction**
- Hide implementation details behind proper abstractions (objects and methods). 
Plain getters and setters may corrupt the data and the purpose of encapsulating it.
```java
public interface Vehicle { //too straighforward
    double getFuelTankCapacityInGallons();
    double getGallonsOfGasoline();
}
//VS
public interface Vehicle { //hides details, clarifies purpose
    double getPercentFuelRemaining();
}
```

2. **Data/Object Anti-Symmetry**\
- Objects hide their data behind abstractions and expose functions that operate on that data. 
Data structure expose their data and have no meaningful functions.
- Procedural code (code using data structures) makes it easy to add new functions without
changing the existing data structures. OO code, on the other hand, makes it easy to add
new classes without changing existing functions.

3. **The Law of Demeter**
- Law of Demeter says that a method f of a class C should only call
the methods of these:
   - C
   - An object created by f
   - An object passed as an argument to f 
   - An object held in an instance variable of C
In other words: module should not know about the innards of the objects it manipulates.
```java
final String outputDir = ctxt.getOptions().getScratchDir().getAbsolutePath(); //three chain calls
```
- Train Wrecks: chain calls for getters are acceptable when objects are data structures without internal logic:
```java
Options opts = ctxt.getOptions();
File scratchDir = opts.getScratchDir();
final String outputDir = scratchDir.getAbsolutePath();
//VS acceptable chaining
final String outputDir = ctxt.options.scratchDir.absolutePath;
```
- Hybrids: avoid creating classes that work half as object and half as data structure, they are sign of bad design.
- Hiding structures: avoid creating getters/setters when class in a plain structure.
Sometimes it may lead to false thinking that there is a hidden logic in those accessors.
When you have a logic to manipulate those accessors, just create a proper method in inside class. 

4. **Data Transfer Objects**
The quintessential form of a data structure is a class with public variables and no functions. 
This is called a data transfer object (DTO). They often become the ﬁrst in a series of translation stages 
that convert raw data in a database into objects in the application code.

5. **Active Record**
- Active Records are special forms of DTOs. They are data structures with public variables.
They typically have navigational methods like *save* and *find*. 
Typically these Active Records are direct translations from database tables, or other data sources. 
- Some developers start to add business logic to those classes. 
This is awkward because it creates a hybrid between a data structure and an object (Hello, Yii2 :)).
The solution, of course, is to treat the Active Record as a data structure and to create
separate objects that contain the business rules and that hide their internal data (which are
probably just instances of the Active Record).