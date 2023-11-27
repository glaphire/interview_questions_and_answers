## Chapter 7. Error Handling

1. **Use Exceptions Rather Than Return Codes**
- Exceptions are "heavy objects" that better to not overuse (in PHP),
but they create clear communication between layers in comparison to manual error logging
or returning array with error message.
2. **Write Your Try-Catch-Finally Statement First**
- _try_ blocks are like transactions. Your _catch_ has to leave your program in a
  consistent state, no matter what happens in the _try_ . For this reason it is good practice to
  start with a _try - catch - finally_ statement when you are writing code that could throw
  exceptions.
- Try to write tests that force exceptions, and then add behavior to your handler 
  to satisfy your tests. This will cause you to build the transaction scope of the _try_ block 
  first and will help you maintain the transaction nature of that scope.

3. **Use Unchecked Exceptions**
- Disclaimer - checked/unchecked exceptions are feature-specific for Java language.
- Unchecked exceptions are descendants of RuntimeException.
- Checked exceptions are those that compiler "may" expect, and they are not a part of broken business logic:
    - ClassNotFoundException
    - InterruptedException
    - InstantiationException
    - IOException
    - SQLException
    - IllegalAccessException
    - FileNotFoundException
- Checked exceptions can sometimes be useful if you are writing a critical library: You
  must catch them, but usually they should be left as they are.
- The price of checked exceptions is an Open/Closed Principle violation. 
  If you _throw_ a checked exception from a method in your code and the _catch_ is three levels
  above, you must declare that exception in the signature of each method between you and
  the _catch_ . This means that a change at a low level of the software can force signature
  changes on many higher levels.

4. **Provide Context with Exceptions**
- Each exception that you throw should provide enough context to determine the source and
  location of an error. (TLDR: add stacktrace, pass additional data in context when needed).

5. **Define Exception Classes in Terms of a Caller’s Needs**
- Try to name, group and wrap handling homogenous exceptions instead of handling them one by one.
  (TBH this point is obvious and title in the book doesn't reflect it).

6. **Define the Normal Flow**
- User ["Special Case" enterprise pattern](https://martinfowler.com/eaaCatalog/specialCase.html)
  when you want to encapsulate checking third-party exceptions
```java
//handling external API exception
try {
  MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
  m_total += expenses.getTotal();
} catch(MealExpensesNotFound e) {
  m_total += getMealPerDiem();
}

//VS 

public class PerDiemMealExpenses implements MealExpenses { //"Special Case"
  public int getTotal() {
    // return the per diem default
  }
}

MealExpenses expenses = expenseReportDAO.getMeals(employee.getID());
m_total += expenses.getTotal();
```

7. **Don’t Return Null**
- Avoid return nulls for each method. Sometimes returning empty array (for lists),
throwing an exception or Special Case pattern are more appropriate.
```java
//too much null checks
public void registerItem(Item item) {
  if (item != null) {
    ItemRegistry registry = peristentStore.getItemRegistry();
    if (registry != null) {
      Item existing = registry.getItem(item.getID());
      if (existing.getBillingPeriod().hasRetailOwner()) {
        existing.register(item);
      }
    }
  }
}
```

8. **Don’t Pass Null**
- Returning null from methods is bad, but passing null into methods is worse. Unless you
  are working with an API which expects you to pass null , you should avoid passing null in
  your code whenever possible.
- In most programming languages there is no good way to deal with a null that is
  passed by a caller accidentally. Because this is the case, the rational approach is to forbid
  passing null by default.

```java
public class MetricsCalculator
  {
    public double xProjection(Point p1, Point p2) {
      return (p2.x – p1.x) * 1.5;
  }
}

calculator.xProjection(null, new Point(12, 13)); //NullPointerException occurs

//VS

//Assertions are shortest way to check for nulls
public class MetricsCalculator
{
  public double xProjection(Point p1, Point p2) {
    assert p1 != null : "p1 should not be null"; 
    assert p2 != null : "p2 should not be null";
    
    return (p2.x – p1.x) * 1.5;
  }
}
```