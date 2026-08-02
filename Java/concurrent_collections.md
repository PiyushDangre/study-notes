## Java Concurrent Collections 

### Blocking Queue

Excellent Video - https://www.youtube.com/watch?v=d3xb1Nj88pw&list=PLL8woMHwr36EDxjUoCzboZjedsnhLP1j4&index=18

Source - https://jenkov.com/tutorials/java-util-concurrent/blockingqueue.html

### Concurrent Map

Excellent Video - https://www.youtube.com/watch?v=OGXAs9fZYTc

Source - https://jenkov.com/tutorials/java-util-concurrent/concurrentmap.html

### CopyOnWriteArrayList

- It works using an ReentrantLock
- Read operation happens on snapshot of the original underlying array.
- Write operation happens using lock (reentrantlock). 
- Readers will see the  older version - thus making the data structure thread safe. 
- CopyOnWriteArrayList is used to avoid ConcurrentModificationException whenever during an iteration to the list, some other thread adds element - the loop will see older data, but at the same time avoid concurrentmodificationexception. 
- Ideal for read heavy scenarios as write-heavy is not suitable due to high memory usage. 


