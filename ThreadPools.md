### Thread Pools
#### What are thread pools?
- Collection of pre-initialized threads.
- Request is delegated to one of the idle threads sitting in the thread pool.
- It carries out the task and returns back to the pool.
- This way JVM or Server need not create new thread everytime and existing threads can be re-used - thus saving resources.

#### How are thread pools created?

- For creating the thread pools in Java - we make use of `Executors` class.
- Part of `java.util.concurrent` package
- It has mainly 4 static methods which return different implementations of thread pools :
  - `public static ExecutorService newCachedThreadPool()`
  - `public static ExecutorService newFixedThreadPool(int nThreads)`
  - `public static ScheduledExecutorService newScheduledThreadPool(int corePoolSize)`
  - `public static ExecutorService newWorkStealingPool()`

1. newCachedThreadPool -> Re-use previously constructed threads when available - or create new as needed. This thread pool is expandable.
2. newFixedThreadPool -> Re-uses out of a fixed number of threads maintained in a queue. If more than 2 threads are submitted, then they are held in queue and blocked until the current threads finish.
3. newScheduledThreadPool -> Executes threads within it at a given interval periodically.
4. newWorkStealingPool -> Threads if idle, actively look for work from other busy threads' queues. 
