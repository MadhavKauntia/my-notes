# Java Concurrency in Practice

## Fundamentals

### Thread Safety

- Writing thread-safe code is about managing access to _state_, and in particular to _shared, mutable state_.
- By _shared_, we mean that a variable could be accessed by multiple threads and by _mutable_, we mean its value could change during its lifetime.
- Whenever more than one thread accesses a given state variable, and one of them might write to it, they must all coordinate their access to it using synchronization.

##### How to ensure thread safety

- Don't share state variables across threads;
- Make the state variable immutable; or
- Use _synchronization_ whenever accessing the state variable

#### What is thread safety?

A class is _thread-safe_ if it behaves correctly when accessed from multiple threads, regardless of the scheduling or interleaving of the execution of those threads by the runtime environment, and with no additional synchronization on the part of the calling code.

##### Example: A Stateless Servlet

```java
@ThreadSafe
public class StatelessFactorizer implements Servlet {
    public void service(ServletRequest req, ServletResponse resp) {
        BigInteger i = extractFromRequest(req);
        BigInteger[] factors = factor(i);
        encodeIntoResponse(resp, factors);
    }
}
```

`StatelessFactorizer` is stateless; it has no fields and references no fields from other classes. The transient state for a particular computation exists solely in local variables that are stored on the thread's stack and are accessible only to the executing thread.

> Stateless objects are always thread-safe.

#### Atomicity

```java
@NotThreadSafe
public class UnsafeCountingFactorizer implements Servlet {
    private long count = 0;

    public long getCount() { return count; }

    public void service(ServletRequest req, ServletResponse resp) {
        BigInteger i = extractFromRequest(req);
        BigInteger[] factors = factor(i);
        ++count;
        encodeIntoResponse(resp, factors);
    }
}
```

`UnsafeCountingFactorizer` is not thread-safe since `++count` is not atomic, which means that it does not execute as a single, indivisible operation. Two threads may simultaneously access count, read the value, see that it is 9, and set it to 10.
To make `UnsafeCountingFactorizer` thread-safe, we can use an atomic variable _AtomicLong_ instead of _long_.

#### Locking

- To preserve state consistency, update _related state variables_ in a single atomic operation.
- **Intrinsic Lock** - The only way to acquire an intrinsic lock is to enter a `synchronized` block or method guarded by that lock. Intrinsic locks in Java act as mutual exclusion locks, which means that at most one thread may own the lock.
- **Reentrancy** - If a thread tries to acquire a lock that it already holds, then it succeeds. Reentrancy means that locks are acquired on a per-thread rather than per-invocation basis. It is implemented by associating each lock with an acquisition count and owning thread. Each time that thread tries to acquire a lock, the acquisition count is incremented and each time it exits the `synchronized` block, the count is decremented. When the count reaches zero, the lock is released.

#### Guarding State With Locks

- For each mutable state variable that may be accessed by more than one thread, all accesses to that variable must be performed with the same lock held. In this case, we say that the variable is guarded by that lock.
- Every shared, mutable variable should be guarded by exactly one lock.
- For every invariant that involves more than one variable, _all_ the variables involved in that invariant must be guarded by the same lock.

### Sharing Objects

Synchronization ensures that when one thread modifies the state of an object, other threads can see the changes that were made.

#### Visibility

In general, there is no guarantee that a reading thread will see a value written by another thread on a timely basis or even at all.

```java
public class NoVisibility {
    private static boolean ready;
    private static int number;

    private static class ReaderThread extends Thread {
        public void run() {
            while(!ready) {
                Thread.yield();
            }
            System.out.println(number);
        }
    }

    public static void main(String[] args) {
        new ReaderThread().start();
        number = 42;
        ready = true;
    }
}
```

In this example, `NoVisibility` could loop forever because the value of `ready` might never become visible to the reader thread. It can also print zero because write to `ready` might be made visible before write to `number`. This phenomenon is known as reordering.

> Locking is not just about mutual exclusion; it is also about memory visibility. To ensure that all threads see the most up-to-date values of shared mutable variables, the reading and writing threads must synchronize on a common lock.

##### Volatile Variables

When a field is declared **volatile**, the compiler and runtime are put on notice that this variable is shared and operations on it should not be re-ordered with other memory operations.

Use volatile variables only when the following conditions are met:

- Writes to the variable do not depend on its current value, or you can ensure that only a single thread ever updates the value;
- The variable does not participate in invariants with other state variables; and
- Locking is not required for any other reason while the invariant is being accessed.

#### Publication and Escape
