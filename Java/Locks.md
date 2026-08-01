## What are locks in Java

Source -> https://jenkov.com/tutorials/java-util-concurrent/lock.html
Excellent Video -> https://www.youtube.com/watch?v=MWlqrLiscjQ&t=88s

- Lock Interface -> represents lock which allows only one thread to operate in a critical section.

- When we use the synchronized keyword in Java, it uses the inbuilt or **intrinsic** locking mechanism which is available in every Object for ensuring the isolation or atomicity in the operation. 

- The Lock interface is how we will implement the isolation of operation in a multithreaded environment in a manual way. We can `.lock()` and `.unlock()` the lock over the object manually. This is the benefit over using `synchronized` method or block. 

- Another advantage is that the usage of manual `Lock` interface - we have option of ensuring the Thread Fairness. Thread fairness means once the lock is unlocked by one thread, the other thread will be served the lock (for locking) on the basis of first-come-first-served. So the next in line thread will be given the lock unlike any other newly spawned thread. This is by-default disabled in Lock interface bu we can pass Boolean param `true` in `ReentrantLock` constructor to enable this.

- We have `tryLock()` method which will lock the object and return Boolean whether it was able to acquire the lock or not. 

- `ReadWriteLock` interface allows multiple threads to read a certain resource, but only one to write it, at a time. It's implementation is `ReentrantReadWriteLock`.


## Compare and Swap (CAS) 

Excellent Video -> https://www.youtube.com/watch?v=ufWVK7CHOAk&list=PLL8woMHwr36EDxjUoCzboZjedsnhLP1j4&index=19
Source -> https://jenkov.com/tutorials/java-concurrency/compare-and-swap.html

- Guaranteed to be atomic by the chipset / Hardware not OS or JVM level.
- Hence it is more efficient
