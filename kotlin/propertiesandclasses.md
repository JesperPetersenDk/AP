[**HOME**](../index.md)

# Properties and classes

### Fields published as properties.
The default implementation of Kotlin property includes field and accessors (getter for val, and getter and setter for var). Thanks to that, we can always replace accessors default implementation with a custom one. 
Variables in Kotlin are always public. Setters and gettere are automaticlay created
Private variable i Java are the same as field in Kotlin.

### Properties backed by fields (Kotlin)

Fields cannot be declared directly in Kotlin classes. However, when a property needs a backing field, Kotlin provides it automatically. This backing field can be referenced in the accessors using the field identifier:

The field identifier can only be used in the accessors of the property.
  - accessor = set()
  - backing field = field

        class User{
        var firstName : String  //backing field generated 
            get() = firstName
            set(value) {firstName = value}

         var lastName : String   //backing field generated
              get() = lastName
              set(value) {lastName = value}

         val name : String                        
              get() = "{$firstName $lastName}"    

         var address : String = "XYZ"   
                                                    //no backing field generated
                                                   //because there is no default
                                                   //implementation of an accessor
                                                

The above code will throw an exception because the default accessor will be called (field). When set(value) {firstName = value} is called, then a recursive call happens and the compiler throws an Stack overflow exception because we are calling a setter  inside the setter.

        class User{
            var firstName : String  
                get() = field
                set(value) {field = value}

           var lastName : String  
                get() = field
                set(value) {field = value}
            
            
### Data classes and their use
We frequently create classes whose main purpose is to hold data. In such a class some standard functionality and utility functions are often mechanically derivable from the data. In Kotlin, this is called a data class and is marked as data:

      data class Developer(val name: String, val age: Int)

**Data classes has to have the following requirments:**
 * The primary constructor needs to have at least one parameter;
 * All primary constructor parameters need to be marked as val or var ;
 * Data classes cannot be abstract, open, sealed or inner;
 * (before 1.1) Data classes may only implement interfaces.
 
**When a class is marked as data class in kotlin, the compiler automatically creates the following:**
  * getters and setters for the constructor parameters
  * hashCode()
  * equals()
  * toString()
  * copy()

On the JVM, if the generated class needs to have a parameterless constructor, default values for
all properties have to be specified.

    data class User(var name: String = "", var age: Int = 0)
    var user = User()

You can call variabels also with "components"

           var user2 = User("Mads", 29)
              fun main() {
                var c = user.component2()
                println(c)
              }

## Sealed classes (Kotlin) and union types (Elm)

**Sealed classes** are used for representing restricted class hierarchies, when a value can have one of the types from a limited set, but cannot have any other type. They are, in a sense, an extension of enum classes: the set of values for an enum type is also restricted, but each enum constant exists only as a single instance, whereas a subclass of a sealed class can have multiple instances which can contain state.

* A sealed class is abstract by itself, it cannot be instantiated directly and can have abstract members.
* Sealed classes are not allowed to have non-private constructors (their constructors are private by default).

      sealed class Operation {
        data class Add(val value: Int) : Operation()
        data class Subtract(val value: Int) : Operation()
        data class Multiply(val value: Int) : Operation()
        data class Divide(val value: Int) : Operation()

        object Increment : Operation()
        object Decrement : Operation()
        }

The key benefit of using sealed classes comes into play when you use them in a when expression.
  * when all cases are covered you don't need an else statement
  * only works if you use when as an expression (useing the fresult) and not as a statment.

          fun execute(x: Int, op: Operation) = when (op) {
              is Operation.Add -> x + op.value
              is Operation.Substract -> x - op.value
              is Operation.Multiply -> x * op.value
              is Operation.Divide -> x / op.value
              Operation.Increment -> x + 1
              Operation.Decrement -> x - 1
            }

            fun main() {
              var add = Operation.Add(5)
              var sub = Operation.Subtract(5)

              println("Add operation ${execute(8, add)}")
              println("Sub operation: ${execute(20, sub)}")
              println("Increment operation: ${execute(20, Operation.Increment)}")
              println("Decrement operation: ${execute(20, Operation.Decrement)}")

            }

            ** Print Out **
            Add operation 13
            Sub operation: 15
            Increment operation: 21
            Decrement operation: 19


**Union types in Elm**

Custom types used to be referred to as “union types” in Elm. 
Example: Creating a chat room. Everyone needs a name, but maybe some users do not have a permanent account. They just give a name each time they show up. 
In the code below you can either have a Regular, Vistor or Anynomous to choose from.

        -- Custom Type
        type User
            = Regular String Int
            | Visitor String
            | Anonymous

        john = Regular "John" 25
        steve = Visitor "Steve"
        
              
How do we actually use custom types?
Say we want a toName function that decides on a name to show for each User. We need to use a case expression:

        toName : User -> String 
        toName user =           
          case user of          
            Regular name age -> 
              name              

            Visitor name ->     
              name              

            Anonymous ->        
              "Anonymous User"  

**Wild Cards**

The toName function we just defined works great, but notice that the age is not used in the implementation? When some of the associated data is unused, it is common to use a “wild card” instead of giving it a name:

      toName : User -> String
      toName user =
        case user of
          Regular name _ ->
            name

          Visitor name ->
            name

The _ acknowledges the data there, but also saying explicitly that nobody is using it.












