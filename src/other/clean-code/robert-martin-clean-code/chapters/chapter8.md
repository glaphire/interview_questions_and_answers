## Chapter 8. Boundaries

1. **Using Third-Party Code**
- Keep boundaries management detaild inside the wrappers to prevent misusing objects
```java
public class Sensors {
    private Map sensors = new HashMap();
        public Sensor getById(String id) {
            //casting to Sensor is kept inside method
            return (Sensor) sensors.get(id); 
        }
}
```

2. **Exploring and Learning Boundaries**
- Consider writing tests (unit, integration) to clarify how to use tricky third-party libraries or APIs.
It's not your responsibility, but it saves time to understand how to use this code and increases stability.
Jim Newkirk calls them "learning tests".

3. **Learning Tests Are Better Than Free**
- Learning tests have a positive return on investment. When there
  are new releases of the third-party package, we run the learning tests to see whether there
  are behavioral differences.
- Covering third-party packages/APIs with tests helps migrate to the new versions faster and safer.

4. **Using Code That Does Not Yet Exist**
- Use Adapter design pattern when you want to isolate the part of code that is not yet implemented.
Sometimes it's okay to create a stub (from my own experience).