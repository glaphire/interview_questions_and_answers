## Chapter 10. Classes

1. **Class Organization (Java conventions, adapted for PHP), order**
- public constants
- private  constants
- public static/scalar variables
- private static/scalar variables
- public object variables
- private object variables
- constructor
- public methods
- private methods
- *protected methods/variables should be avoided, but can be necessary for tests

2. **Classes should be small as possible and follow SRP**
   - "Processor" or "Manager" or "Super" are usually markers of bad responsibility.
   - Avoid high cohesion in class (when properties and methods are too dependent on each other).

3. **Organizing for Change**
- Classes should follow SRP+OCP+DIP to reduce risk of changes.
