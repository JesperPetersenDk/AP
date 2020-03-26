[**HOME**](../index.md)

      package Reflection

      import kotlin.reflect.KFunction
      import kotlin.reflect.full.memberFunctions
      import kotlin.reflect.full.memberProperties

      class Person(val id: Int, var firstName: String, var lastName: String, var age: Int) {
          fun greet(person: Person) = "Hello ${person.firstName} from $firstName"
      }

      fun toJson(what: Any) =
          what::class.memberProperties
              .map { """ "${it.name}": "${it.call(what)}""""}
              .joinToString(",\n", "{\n","\n}\n")

      fun main() {
          val kurt = Person(7, "Kurt", "Hansen", 27)
          val ib = Person(8, "Ib", "Jensen", 33)

          println("-------------")

          val json = toJson(kurt)
          println(json)

          println("-------------")

          println(getMethod(kurt, "greet"))

          println(getMethod(kurt, "greet")?.call(kurt, ib))
      }

      fun getMethod(some1: Any, methodName: String): KFunction<*>? {
          val type1 = some1::class
          val greet = type1.memberFunctions
              .filter { it.name == methodName }
              .firstOrNull() ?: return null
          return greet
      }

OBS: En oneliner til at skrive ints UDEN gåseøjne i toJson():

      fun toJson(what: Any) =
                what::class.memberProperties.joinToString(
              ",\n",
              "{\n",
              "\n}\n"
          ) { """ "${it.name}": ${if (it.returnType == Int::class.createType()) it.call(obj) else "\"${it.call(obj)}\""} """ }
