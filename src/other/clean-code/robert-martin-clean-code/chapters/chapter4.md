## Chapter 4. Comments

1. The older a comment is, and the farther away it is from the code it describe, 
because it's hard to maintain it.
2. Inaccurate comments are far worse than no comments at all.
3. Usually the urge of writing a comment should be used as motivation
to refactor the code to the point when it doesn't need an explanation anymore.

**Good comments**:
1. Legal Comments (info about copyright and/or license).
2. Information that is hard to describe via good code design (e.g. complex regular expressions).
3. Explanation of intent behind a decision.
4. Clariﬁcation (when there is no way to refactor for readability).
5. Warning of Consequences, ampliﬁcation.
6. TODO Comments.
7. Doc comments (phpdoc, library-specific metadata in the comments).

**Bad comments**:
1. Mumbling (unclear explanation in the comment).
2. Redundant comments ("Captain obvious" style).
3. Misleading, innacurate comments.
4. Mandated comments (e.g. phpdoc for methods and properties that are parsed by the IDE automatically).
5. Journal Comments (redundant because of version control systems).
6. Comment When You Can Use a Function or a Variable (refactor for better naming).
7. Position Markers (aka comment-separator to divide code into blocks).
8. Closing Brace Comments:
```java
while ((line = in.readLine()) != null) {
    lineCount++;
    charCount += line.length();
    String words[] = line.split("\\W");
    wordCount += words.length;
} //while
```
9. Attributions and Bylines - no needed because of VCS.
```/* Added by Rick */```
10. Commented-Out Code (acceptable as a short-term solution in critical situation).
11. HTML Comments (no need to format comment intentionally, better use an external tool if needed)
```java
/**
* Task to run fit tests.
* This task runs fitnesse tests and publishes the results.
* <p/>
* <pre>
* Usage:
* &lt;taskdef name=&quot;execute-fitnesse-tests&quot;
* classname=&quot;fitnesse.ant.ExecuteFitnesseTestsTask&quot;
* classpathref=&quot;classpath&quot; /&gt;
* OR
* &lt;taskdef classpathref=&quot;classpath&quot;
* resource=&quot;tasks.properties&quot; /&gt;
* <p/>
* &lt;execute-fitnesse-tests
* suitepage=&quot;FitNesse.SuiteAcceptanceTests&quot;
* fitnesseport=&quot;8082&quot;
* resultsdir=&quot;${results.dir}&quot;
* resultshtmlpage=&quot;fit-results.html&quot;
* classpathref=&quot;classpath&quot; /&gt;
* </pre>
*/
```
12. Nonlocal Information.
13. Too Much Information.
14. Inobvious Connection.