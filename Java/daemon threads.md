### Daemon Thread

- Low priority thread
- It is a service provider thread which runs in background - and should not be used as a main thread.
- Doesn't prevent the JVM from exiting even when all the main threads have exited.
- JVM automatically closes daemon threads if main threads are done.
- Ex. Garbage Collector.

to convert to daemon --> `Thread.setDaemon(true)`

Thread cannot be marked as daemon once it has started ie. `start` method has been called upon it. 

### ThreadGroup

- Represents a group of threads.

Creation -> `ThreadGroup pGroup = new ThreadGroup("Parent ThreadGroup");`
Add thread in thread group -> `Thread t1 = new Thread(pGroup, this);`

### JVM Shutdown Hook

- we can create a thread object with some run() method implementation and pass that object as a hook to JVM. This is called shutdown hook.
- Before the JVM shuts down - the thread is run. 
- JVM can shut down due to :
    - The program ends
    - The System.exit() method is called.
    - the runtime halt method is called -> Runtime.getRuntime().halt()

How to add the shutdown hook?

`Runtime.getRuntime().addShutdownHook(Thread t)`

Now the thread will run just before the JVM is shut down.

Similarly can remove also - 

`Runtime.getRuntime().removeShutdownHook(Thread t)`
