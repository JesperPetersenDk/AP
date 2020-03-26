[**HOME**](../index.md)

### coroutines for iterators and sequences
A sequence or an iterator can be lazily built with a specialized coroutine scope, e.g. a SequenceScope.

In the scope every call to yield suspends the coroutine until the next request for an item of the sequence.

This way the sequence is built as the elements of it are requested.

      main(args: Array<String>) {

    // The sequence is lazily built as we yield values to it.
    val sequenceObj:Sequence<Int> = sequence {
        println("yielding 23")
        yield(23) // suspends the coroutine until next element is requested (an iterator will ask hasNext() on the sequence).

        println("yielding 1 to 6")
        yieldAll(1..6) // again yields the coroutine

        println("yielding 42")
        yield(42)
        println("done")
    }
    sequenceObj.forEach(::println) // here the iterator will call hasNext() on the sequence.
    println(sequenceObj.count())

### coroutines for concurrency
Coroutines runs on threads by default. These threads are often managed by a thread pool.

delay() does not block the thread, the thread is simply returned to the pool and the coroutine state is stored to be resumed at a later time.

When the delay time is over, the next free thread of the pool continues execution of the coroutine.

launch starts a coroutine - fire and forget.

runBlocking starts a coroutine and blocks the thread that started it awaiting the completion of the coroutine.

async starts a coroutine and returnes an object of type Deferred. The object can be awaited.

      package Coroutines

      import kotlinx.coroutines.delay
      import kotlinx.coroutines.launch
      import kotlinx.coroutines.runBlocking
      import kotlin.concurrent.thread

      fun main() {
          thread {
              doWorkWithThreds(1_000_000, 100_000)
          }
          doWorkWithCoroutines(1_000_000, 100_000)


      }

      fun doWorkWithCoroutines(limit: Int, step: Int) = runBlocking {
          for (i in 1..limit) {
              launch {
                  delay(100)
                  if(i % step == 0) print("CR$i ")
              }
          }
          println()
      }

      fun doWorkWithThreds(limit: Int, step: Int) {
          for (i in 1..limit) thread {
              Thread.sleep(100)
              if( i% step == 0) print("T$i ")
          }
}
