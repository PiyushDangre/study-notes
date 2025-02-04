# Java Multithreading & Concurrency Notes
<small>02<sup>nd</sup> Feb 2025</small>

|| ॐ साई राम ||

## Intro

**Benefits of Multithreading**

- Better CPU utilization.
- Simpler program design in some situations.
- More responsive programs.
- More fair division of CPU resources between different tasks.

> More explanation here -> [Jenkov Blog](https://jenkov.com/tutorials/java-concurrency/benefits.html).

**Cons of Multithreading**

- More complex design _(More synchronization required)_.
- Context switching overhead _(CPU switching from one thread to another)_.
- Increased resource consumption _(memory consumption more)._

> More explanation here -> [Jenkov Blog](https://jenkov.com/tutorials/java-concurrency/costs.html).

**Concurrency Models**

- Parallel Workers _(Incoming jobs are assigned to different workers. )_
    - Shared State Workers
    - Stateless Workers
- Assembly Line _(Reactive/ Event Driven Systems)_
- Functional Parallelism _(you implement your program using function calls)_

> More Explanation -> [Jenkov Blog](https://jenkov.com/tutorials/java-concurrency/concurrency-models.html)

**Race Conditions**

Race conditions occur when two or more threads try to access a **_critical section_** of code (read data members) and their sequence of accessing the critical section makes a difference in the final result. 

There are two types of race condition patterns - 

1. Read-Modify-Write
2. Check-Then-Act

> To prevent the race conditions, we have to use synchronized blocks. 

**Thread Safety**

Code that is safe to call by multiple threads is called _Thread safe code_.

- Primitive Local variables
- Local object references (Only the object reference and not the object itself. Object is stored in heap)
- Object Member Variables

> If a resource is created, used and disposed within
the control of the same thread,
and never escapes the control of this thread,
the use of that resource is thread safe.

Race conditions occur only if multiple threads are **writing** to the same resource.
It does not occur if multiple threads are just **reading** the same resource. 

We can create the shared objects between threads as immutable to achieve thread safety.

_(To be continued .. )_