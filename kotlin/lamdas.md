[**HOME**](../index.md)

# Lambdas

### what is a lambda 
A lambda is an anonymous function created on the fly. 
Under the hood, Kotlin in fact creates an anonymous class with the method and runs it.
### lambda type definitions (Kotlin/Elm)
(T1,... Tn) -> R

A function takes arguments T1-Tn and returns a result R.
### use of lambdas in collections
To manipulate items of a collection in a uniform matter, a higher-order function could be written to accept an item and a function as arguments. 

The higher-order function would in turn call the passed-in function passing each item to it.

Many collections have built-in higher-order functions, e.g. map(), reduce(), filter() that manipulate or evaluates each item of a list.
### difference between collections and streams (Kotlin)
Every call to a built-in higher-order function of a collection creates a new collection containing the resulting items.

Since the built-in higher-order functions can be changed, it is possible to do this: 

priceOfItemsThatShipToDK = myItems.map(item -> item.priceEur *= 7.5).filter(item -> item.shipsWorldwide == true).take(10)

Say we have an inventory of 100000 items whereof 2500 are shippable to Denmark, the call to map would yield a new collection of 100000 items with a price in DKK. 

Next, the filter call will yield a new collection with the 2500 shippable items.

Lastly, we only need 10 thereof, the take will make the final collection of 10 and return it.


With a stream, the complete list of operations for each item are chained and carried out on one item after the other only until the final condition is met, ie. a list of 10 items. 

Do note that there is a pivotal point below which there is no advantage of converting a collection to a stream to perform data operations. If you know the collection to be very small, don't bother, since it is also somewhat costly to produce the chaining operations of the stream. Conversely if the collection is large, do create a stream. If you don't know, create a stream.

One may think of collections to be about grouping/storing data and streams to be about operating on data.

### lambdas with receiver and DSLs
DSL - Domain Specific Language - a language comprised of keywords from the domain the application is itended for, e.g. HTML.

With lambdas we can create such a DSL with ease due to the fact that if the lambda is the last parameter of a function, it can be placed outside the function parens. See code example at the bottom.

The receiver of a lambda is the instance it is called on, e.g.:      

      fun Book.toc(build: Toc.() -> Unit = {}):Toc{...}

Here the receiver of the lambda is the Book instance. The lambda itself is an extension of the Toc class.
      
      

### higher order functions
Higher order functions can take a function as parameter and / or return a function.
Collections have built-in higher-order functions such as map(), filter(), reduce() and sort().


      fun main() {
          val product = { a: Int, b: Int -> a * b }
          val result = product(9, 3)
          println(result)

          println("-------------")

          callMe({ println("Hello!") })

          println("-------------")

          val p = partProduct(5)
          val res = p(7)
          println(res)

          println("-------------")

          val numbers = listOf("one", "two", "three", "four")
          val longerThan3 = numbers.filter { it.length > 3 }
          println(longerThan3)
      }


      //Higher order function
      fun callMe(greeting: () -> Unit) {
          greeting()
      }

      //Higher order function
      fun partProduct(a: Int): (b: Int) -> Int {
          return {b -> a * b}
      }
      
      
      
      // **************** DSL example ******************

      class Book(val title:String)
      {
          var toc:Toc? = null
          override fun toString():String
          {
              return if (toc != null) toc.toString() else "no toc, sorry."
          }
      }
      
      // global method to create a book.
      fun book(title:String, build: Book.() -> Unit = {}):Book
      {
          val b = Book(title)
          b.build()
          return b
      }
      
      // Table of contents.
      class Toc(){
          val chapters:MutableMap<Int, Chapter> = mutableMapOf()
          override fun toString():String
          {
              return chapters.map { """${it.value}${".".repeat(50-it.value.title.length)}${it.key}""" }.toList().joinToString ("\n")
          }
      }
      
      // Extension method on Book class to create table of contents.
      fun Book.toc(build: Toc.() -> Unit = {}):Toc
      {
          val i = Toc()
          i.build()
          this.toc = i
          return i
      }
            
      class Chapter(val title:String){
          override fun toString(): String {
              return title
          }
      }
      
      // Extension method on Toc to create chapter.
      fun Toc.chapter(page:Int, chTitle:String, build: Chapter.() -> Unit = {}):Chapter
      {
          var chapter:Chapter = Chapter(chTitle)
          chapter.build()
          // add a key-value pair to chapters, key is page, value is chapter.
          this.chapters += page to chapter
          return chapter
      }

      fun main() {

          val book = book("Bogen"){
              toc {
                  chapter(3,"1. Forord")
                  chapter(4,"2. Indledning")
                  chapter(6,"3. Det hele begyndte i december engang...")
                  chapter(25,"4. Fastelavn - hvad sker der nu?")
                  chapter(50,"5. Påske og festen er forbi.")
              }
          }
          println(book)
      }
