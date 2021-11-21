# Java Concurrency in Practice

## Fundamentals

### Thread Safety
* Writing thread-safe code is about managing access to *state*, and in particular to *shared, mutable state*.
* By *shared*, we mean that a variable could be accessed by multiple threads and by *mutable*, we mean its value could change during its lifetime.
* Whenever more than one thread accesses a given state variable, and one of them might write to it, they must all coordinate their access to it using synchronization.

##### How to ensure thread safety
* Don't share state variables across threads;
* Make the state variable immutable; or
* Use *synchronization* whenever accessing the state variable

#### What is thread safety?
A class is *thread-safe* if it behaves correctly when accessed from multiple threads, regardless of the scheduling or interleaving of the execution of those threads by the runtime environment, and with no additional synchronization on the part of the calling code.

##### Example: A Stateless Servlet
```
@ThreadSafe
public class StatelessFactorizer implements Servlet {
    public void service(ServletRequest req, ServletResponse resp) {
        BigInteger i = extractFromRequest(req);
        BigInteger[] factors = factor(i);
        encodeIntoResponse(resp, factors);
    }
}
```
```StatelessFactorizer``` is stateless; it has no fields and references no fields from other classes. The transient state for a particular computation exists solely in local variables that are stored on the thread's stack and are accessible only to the executing thread.
> Stateless objects are always thread-safe.

#### Atomicity
```
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
```UnsafeCountingFactorizer``` is not thread-safe since ```++count``` is not atomic, which means that it does not execute as a single, indivisible operation. Two threads may simultaneously access count, read the value, see that it is 9, and set it to 10.
To make ```UnsafeCountingFactorizer``` thread-safe, we can use an atomic variable *AtomicLong* instead of *long*.