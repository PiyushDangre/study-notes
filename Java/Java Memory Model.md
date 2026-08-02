
## Java Memory Model


JMM is a specification which guarantees visibility of fields amidst reordering of instructions.

** Visibility of fields **

- Visibility of fields is not guaranteed for a field in a monitor object which is operated upon by two threads. This is because JVM makes use of multicore processor architecture.
- T1 -> running in core 1 --> has its own L1 cache and L2 cache which is shared 
- T2 -> running in core 2 --> has its own L1 cache and L2 cache which is shared 
- Initially value of field is stored in shared cache. When T2 amends the value - it is completely possible that the change happens in  T2 local cache and the change is not pushed to shared cache. So, when T1 reads it, it gets value from shared cache which is old. 
- So using keyword volatile will make the change push to shatred cache so that updated value is read by other thread. 


** Reordering of instructions **

Instructions can be re-ordered during compilation for performance improvement etc. 


- JMM rules have to be implemented by all JVMs.

- **Happens-before-guarantee** : Whatever happens before write to a volatile field in one thread - is visible to other threads after the same volatile field is read. Even though the operations happening before are not using volatile keyword. Same is applicable to when locks is used or synchronized keyword is used. 
