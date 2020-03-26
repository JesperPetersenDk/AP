[**HOME**](index.md)

## Functions as first class members

* Procedural programming
* Object oriented programming
* Functional programming
* Domain specific languages

## Procedural (Verfahrensorentiert) programming support

* Functions defined at package level
* Properties defined at package level
* Records defined as data classes

## Object oriented programming OOP support

### Enhanced readability
  * Properties taken seriously
  * Operator overloads
  * Extension functions (and properties)
  * Smart casting
  * No semicolons (almost)
### Robustness
  * Default is most restrictive
  * Functions are closed (not virtual) until opened
  * Values are always final
  * Null values are normally not allowed
  * Classes are closed (final) until opened
 
## Functional programming support

* Immutable objects are easily implemented with data classes
* Union types can be implemented with sealed classes
* Conditional expressions supports writing pure functions
* Tail recursive awareness supports efficient recursivity
* Streams and collections
* Lambdas and functions as first class members

## Coroutines

* Asynchronous
* Non-blocking
* Only syntax is the suspend keyword, rest is library
* Some coroutine builders
    * launch
    * async
    * runBlocking
    * createSequence
    * createIterator
    
