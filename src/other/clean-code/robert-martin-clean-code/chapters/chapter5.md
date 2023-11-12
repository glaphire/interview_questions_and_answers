## Chapter 5. Formatting

1. **Vertical formatting**
- desirable length of a file is 200-500 lines long.
- The Newspaper Metaphor: put the highest-level code to the top, 
so it's easy to figure out the high-level goals of a module or class. 
- Vertical Openness Between Concepts: put blank lines between units of the code to separate it visually
(according to code standards in your language).
- Vertical Density: lines of code that are tightly related should appear vertically dense.
- Vertical Distance:
    - *Variable Declarations*: variables should be declared as close to their usage as possible. In rare cases a variable might be declared 
    at the top of a block or just before a loop in a longish function. Instance variables should be declared at the top of the class (Java convention).
    - *Dependent Functions*: if one function calls another, they should be vertically close,
      and the caller should be above the callee, if at all possible. This gives the program a natural ﬂow.
    - *Conceptual Afﬁnity*: the stronger that afﬁnity, the less vertical distance there should be between them.
    - *Vertical Ordering*: function that is called should be below a function that does the calling.

2. **Horizontal Formatting**
- Set the maximum line length to 120 characters (or according to coding standards of your language).
- Horizontal Openness and Density: use horizontal white space to associate things that are strongly related and disassociate
  things that are more weakly related.
- Horizontal Alignment: horizontal alignment of the data structures is not usually useful.
- Indentation: To make this hierarchy of scopes visible, indent the lines of source code in proportion to their position in the hiearchy.

3. **Team Rules**
- Accept the rules that were accepted in your team.
- A good software system is composed of a set of documents that read nicely. 
They need to have a consistent and smooth style.
\* (imho: try to negotiate to implement language code standards if whenever possible).