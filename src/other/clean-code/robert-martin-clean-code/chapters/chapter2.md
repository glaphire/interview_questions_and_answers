## Chapter 2. Meaningful names

**1. Use Intention-Revealing Names**
```java
int d; // elapsed time in days

//vs

int elapsedTimeInDays;
int daysSinceCreation;
int daysSinceModification;
int fileAgeInDays;
```

**2.Avoid Disinformation**

- Avoid leaving false clues that obscure the meaning of code. 
- Do not use abbreviations that can be misinterpreted (like 'hp' for hypotenuse).
- Do not use misleading names, such as `List`, `Container` etc. if in reality they don't represent this abstraction.
- Use consistent spelling.
- Avoid long similar names which don't vary enough (XYZControllerForEfficient**Handling**OfStrings vs XYZControllerForEfficient**Storage**OfStrings).
- Don't mix characters with similar look (0 vs O, l vs I).

**3. Make Meaningful Distinctions**
- Avoid Disinformation: `a1, a2` vs `source, destination`. 
- Decrease 'noise words': 
  - `Product, ProductInfo, ProductData` vs `Product, Info, Data`,
  - `NameString` vs `Name`,
  - `UserTable` vs `UserTable`.
  - `CustomerObject` vs `Customer`.
- Avoid using `a, the` in the naming unless it's really necessary.

**4.Use Pronounceable Names**
- `genymdhms` (generation date, year, month, day, hour, minute,
  and second) vs `generationTimestamp`.
- 
**5.Use Searchable Names**
- `MAX_CLASSES_PER_STUDENT` const vs `7`.
- single-letter names can ONLY be used as local variables inside short methods (like indices in loops).
- The length of a name should correspond to the size of its scope.

**6.Avoid Encodings**
- idk how to interpret this, but I think it's about not mixing English with other languages in naming.
- Hungarian notations is considered outdated.
- No member prefixes in names (like 'm_' for class property or 'I' for interface).

**7.Avoid Mental Mapping**
- Readers shouldn’t have to mentally translate your names into other names they already know. 
- Use consistent naming in the entire project (smth like Ubiquitous Language).

**8.Class Names**
- Classes and objects should have noun or noun phrase names like `Customer`, `WikiPage`, `Account`, and `AddressParser`. 
Avoid words like `Manager`, `Processor`, `Data`, or `Info` in the name of a class. A class name should not be a verb.

**9.Method Names**
- Methods should have verb or verb phrase names like `postPayment`, `deletePage` , or `save`.
Accessors, mutators, and predicates should be named for their value and preﬁxed with `get`,
`set`, and `is`.

**10.Don’t Be Cute**
- Don't use literary techniques or cultural references in names:
  - `whack()` vs `kill()`.
  - `eatMyShorts` vs `abort()`.

**11.Pick One Word per Concept**
- Don't use synonyms for same job stick with one (inside one unit of code):
  - `fetch, retrieve, get`
  - `DeviceManager` and `ProtocolController` in the same layer.

**12.Don’t Pun**
- Avoid forcing the same naming for sake of consistensy when the semantic is different:
  - `add` vs `append` vs `insert`.

**13.Use Solution Domain Names**
- Use Computer Science language when you mean to implement an abstraction from it - pattern name, infrastructural approach etc.

**14.Add Meaningful Context**
- Names can have very different meanings when not surrounded by context (like class name and properties):
  - `state` as a part of postal address or as a state of entity.
  - `address` as an internet address or postal address.
- Use more previse naming in inherited classes if necessary:
  - Address -> accountAddress, customerAddress.
  - Address -> PostalAddress, MAC, URI (less verbose, more precise).
