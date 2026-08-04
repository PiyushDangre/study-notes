
#### Spring Boot Transaction Management

- @transactional annotation is used

Introduction Video - 
- **PART 1**: https://www.youtube.com/watch?v=b7Pev6i8fso
- **PART 2**: https://www.youtube.com/watch?v=GqpQ3J40Op8

**ACID Properties** 
- Atomicity
- Consistency
- Isolation
- Durability

**Transaction Propagation**

- REQUIRED (Default)
- REQUIRES_NEW
- NESTED 
- MANDATORY
- NEVER
- NOT_SUPPORTED
- SUPPORTS 

**Problems which isolation levels solve**

- *Dirty Reads* : Reads uncommited change of concurrent txn
- *Non-repeatable reads*: Reads diff value of a row in a re-read of the row if concurrent txn makes some change to the row and commits.
- *Phantom reads*: Gets more/less number of rows if concurrent txn modifies the rows in a concurrent txn and commits. 

**LOCKS in DB**

- SHARED LOCK
- EXCLUSIVE LOCK

**Transaction Isolation Levels**
- DEFAULT
= READ_UNCOMMITTED - Most unsafe (PostGres/Oracle do not support)
- READ_COMMITTED - Prevents only dirty reads. (Default in Postgres/Oracle)
- REPEATABLE_READ - Prevents dirty and non-repeatable reads. Locks the entire row. 
- SERIALIZABLE - Highest level and prevents all problems. But very slow and lowest concurrency access rate as concurrent calls are executed sequentially. 

----------------


Here are compact markdown tables you can keep as a Spring Transaction Management cheat sheet.

## 1. ACID Properties

| Property        | Meaning                                                                       | Example                                                 |
| --------------- | ----------------------------------------------------------------------------- | ------------------------------------------------------- |
| **Atomicity**   | Transaction is all-or-nothing. Either all operations succeed or all rollback. | Money debited from A and credited to B together.        |
| **Consistency** | Transaction moves DB from one valid state to another valid state.             | Account balances remain valid after transfer.           |
| **Isolation**   | Concurrent transactions should not interfere with each other.                 | One transaction should not see partial work of another. |
| **Durability**  | Once committed, data survives crashes/restarts.                               | Committed transfer remains even after DB restart.       |

---

## 2. Transaction Propagation Modes

| Propagation            | Existing Transaction Present? | Behavior                                            | Typical Use Case                   |
| ---------------------- | ----------------------------- | --------------------------------------------------- | ---------------------------------- |
| **REQUIRED** (Default) | Yes                           | Join existing transaction                           | Normal service methods             |
|                        | No                            | Create new transaction                              |                                    |
| **REQUIRES_NEW**       | Yes                           | Suspend existing transaction and create new one     | Audit logs, notifications          |
|                        | No                            | Create new transaction                              |                                    |
| **NESTED**             | Yes                           | Create savepoint inside current transaction         | Partial rollback support           |
|                        | No                            | Create new transaction                              |                                    |
| **MANDATORY**          | Yes                           | Join existing transaction                           | Must run within transaction        |
|                        | No                            | Throw exception                                     |                                    |
| **NEVER**              | Yes                           | Throw exception                                     | Method must not run in transaction |
|                        | No                            | Execute without transaction                         |                                    |
| **NOT_SUPPORTED**      | Yes                           | Suspend transaction and execute non-transactionally | Reporting/read-only operations     |
|                        | No                            | Execute without transaction                         |                                    |
| **SUPPORTS**           | Yes                           | Join existing transaction                           | Transaction optional               |
|                        | No                            | Execute without transaction                         |                                    |

### Quick Memory Trick

| Propagation   | Shortcut                     |
| ------------- | ---------------------------- |
| REQUIRED      | Join if exists, else create  |
| REQUIRES_NEW  | Always create new            |
| NESTED        | Savepoint inside current     |
| MANDATORY     | Must already exist           |
| NEVER         | Must not exist               |
| NOT_SUPPORTED | Suspend current transaction  |
| SUPPORTS      | Use transaction if available |

---

## 3. Concurrency Problems

| Problem                 | What Happens?                                             | Example                                                                       |
| ----------------------- | --------------------------------------------------------- | ----------------------------------------------------------------------------- |
| **Dirty Read**          | Reads uncommitted data from another transaction           | T2 reads salary change made by T1 before T1 commits                           |
| **Non-Repeatable Read** | Same row returns different values within same transaction | T1 reads salary = 1000, T2 updates to 2000 and commits, T1 reads again → 2000 |
| **Phantom Read**        | Same query returns different number of rows               | T1 gets 10 employees, T2 inserts employee and commits, T1 gets 11 employees   |

### Memory Flow

| Problem             | Row Changed? | Row Count Changed? |
| ------------------- | ------------ | ------------------ |
| Dirty Read          | Maybe        | Maybe              |
| Non-Repeatable Read | ✅ Yes        | ❌ No               |
| Phantom Read        | ✅ Possible   | ✅ Yes              |

---

## 4. Database Locks

| Lock Type              | Read Allowed?     | Write Allowed? | Used By                |
| ---------------------- | ----------------- | -------------- | ---------------------- |
| **Shared Lock (S)**    | ✅ Yes             | ❌ No           | SELECT operations      |
| **Exclusive Lock (X)** | ❌ No (for others) | ✅ Yes          | INSERT, UPDATE, DELETE |

### Lock Compatibility

| Existing Lock  | New Shared Lock | New Exclusive Lock |
| -------------- | --------------- | ------------------ |
| Shared Lock    | ✅ Allowed       | ❌ Blocked          |
| Exclusive Lock | ❌ Blocked       | ❌ Blocked          |

---

## 5. Transaction Isolation Levels

| Isolation Level      | Dirty Reads | Non-Repeatable Reads | Phantom Reads | Notes                                 |
| -------------------- | ----------- | -------------------- | ------------- | ------------------------------------- |
| **READ_UNCOMMITTED** | ❌ Possible  | ❌ Possible           | ❌ Possible    | Lowest isolation, highest concurrency |
| **READ_COMMITTED**   | ✅ Prevented | ❌ Possible           | ❌ Possible    | Default in PostgreSQL & Oracle        |
| **REPEATABLE_READ**  | ✅ Prevented | ✅ Prevented          | ❌ Possible*   | Default in MySQL InnoDB               |
| **SERIALIZABLE**     | ✅ Prevented | ✅ Prevented          | ✅ Prevented   | Highest isolation, lowest concurrency |

* In theory REPEATABLE_READ allows phantom reads according to SQL standard. MySQL InnoDB often prevents many phantom-read cases using next-key locking.

---

## 6. Isolation Levels vs Problems (Most Important Interview Table)

| Isolation Level  | Dirty Read | Non-Repeatable Read | Phantom Read |
| ---------------- | ---------- | ------------------- | ------------ |
| READ_UNCOMMITTED | ❌          | ❌                   | ❌            |
| READ_COMMITTED   | ✅          | ❌                   | ❌            |
| REPEATABLE_READ  | ✅          | ✅                   | ❌            |
| SERIALIZABLE     | ✅          | ✅                   | ✅            |

### Fastest Way to Remember

| Level            | Think                                 |
| ---------------- | ------------------------------------- |
| READ_UNCOMMITTED | Nothing protected                     |
| READ_COMMITTED   | Protects committed data only          |
| REPEATABLE_READ  | Same row always same                  |
| SERIALIZABLE     | Transactions behave one after another |

---

## 7. Complete Spring Transaction Interview Map

| Topic               | Key Question                                   |
| ------------------- | ---------------------------------------------- |
| ACID                | Why transactions exist?                        |
| Propagation         | Which transaction should this method run in?   |
| Isolation           | What concurrent data can this transaction see? |
| Locks               | How does DB enforce isolation?                 |
| Dirty Read          | Read uncommitted data?                         |
| Non-Repeatable Read | Same row changes?                              |
| Phantom Read        | Result set changes?                            |
| REQUIRED            | Join or create?                                |
| REQUIRES_NEW        | Always new transaction?                        |
| SERIALIZABLE        | Safest but slowest?                            |

This single page covers about **80–90% of Spring transaction questions** commonly asked in Java/Spring interviews.
