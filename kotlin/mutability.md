[**HOME**](../index.md)


### Mutability

- **in inheritance open classes and functions**
      Classes and functions in Kotlin are by default closed and need to be explicitly opened. 
      The keyword is **"open"**

- **in properties val and var**

Properties in Kotlin classes can be declared either as mutable using the var keyword, or as read-only using the val keyword.

      open class Person (var firstName: String, var lastName: String, val dateOfBirth: String)


- **in collections**

  - A read-only interface that provides operations for accessing collection elements.
  - A mutable interface that extends the corresponding read-only interface with write operations: adding, removing, and updating its elements.
  - For example in map() or filter() you get a copy of the original collections and not the original it self.
  
  

- **in the perspective of functional programming**
  - Functional programming is a paradigm that treats computation as the manipulation of value, and avoids changing state and mutable data.
  - In functional code, the output value of a function depends ONLY on the arguments that are passed to the function. In other words,           calling a function f twice with the same value for an argument x produces the same result f(x) each time.
