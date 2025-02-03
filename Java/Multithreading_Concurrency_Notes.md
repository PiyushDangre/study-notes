# Java Multithreading & Concurrency Notes
<small>02<sup>nd</sup> Feb 2025</small>

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

