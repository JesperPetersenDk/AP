[**HOME**](../index.md)


## Extension functions

    package Kotlin

    fun String.removeFirstLastChar(): String =  this.substring(1, this.length - 1)

    fun main() {
        val myString= "Hello Everyone"
        val result = myString.removeFirstLastChar()
        println("Remove first and last char: $result")
    }

## Extension Properties

    // Extension property who check's if a String is empty or null
    val String?.empty : Boolean
        get() = (this == null || this.isEmpty())
        
    // Extension property who check's if a Int is higher or the same as 18
    val Int.toHigh : Boolean
        get() = (this >= 18)

## Inheritance

        package Kotlin

        open class Person(age: Int, name: String) {
            init {
                println("My name is $name.")
                println("My age is $age")
            }
        }
        class Teacher(age: Int, name: String): Person(age, name) {
            fun teach() {
                println("I teach programming")
            }
        }
        class Student(age: Int, name: String): Person(age, name) {
            fun learn() {
                println("I learn programming")
            }
        }

        class Dog(age: Int, name: String) {
            fun bark() {
                println("I'm barking")
            }
        }

        fun main() {
            val t1 = Teacher(25, "Kurt")
            t1.teach()
            println("Class: ${t1::class}")
            println("SuperClass: ${t1::class.supertypes.first()}")
            // println("SuperClass: ${t1::class.supertypes}") - Any missing
            println("-------------")

            val s1 = Student(29, "Ib")
            s1.learn()
            println("Class: ${s1::class}")
            println("SuperClass: ${s1::class.supertypes.first()}")

            println("-------------")

            val d1 = Dog(4, "Fido")
            d1.bark()
            println("Class: ${d1::class}")
            println("SuperClass: ${d1::class.supertypes.first()}")

            println("-------------")

            val a1 = Any()

            println("Class: ${a1::class}")
            println("SuperClass: ${a1::class.supertypes.firstOrNull()}")

        }
        
## DSL

* **Example 1**

        fun hvis(test: Boolean, block: () -> Unit): Boolean {
            if (test) block()
            return test
            }

        infix fun Boolean.ellers(block: () -> Unit) {
            if (!this) block()
            }

        fun main() {
            hvis(5 == 5) {
                println("Det var s'gu mærkeligt")
            } ellers {
                println("Pyha!")
                }
            println("DONE!")
            }
   
       data class Human(
        var name: String? = null,
        var age: Int? = null,
        var address: Address? = null
       )

       data class Address(
        var street: String? = null,
        var number: Int? = null,
        var city: String? = null
       )

* **Example 2**

      fun human(build: Human.() -> Unit): Human
      {
          val h = Human()
          //build(h) Why does this workes???????
          h.build()
          return h
      }

      val h1 = human{
              name = "Joerg"
              age = 42
              address{
                  street = "Baunehøjvej"
                  number = 52
                  city = "Lyngby"
              }
          }
