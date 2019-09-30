
### Multithreading/Concurrency in Java

#### Threads
-	Every thread has a priority and can be a daemon.
-	When a thread creates a new thread, the priority of the new thread is set to the priority of the creating thread.
-	A new thread is marked as a daemon only if the creating thread is a daemon.
-	When a Java Virtual Machine starts up, there is usually a single non-daemon thread (which typically calls the method named main of some designated class).
-	The Java Virtual Machine continues to execute threads until either of the following occurs
	-	The exit method of class Runtime has been called and the security manager has permitted the exit operation to take place.
	-	All threads that are not daemon threads have died, either by returning from the call to the run method or by throwing an exception that propagates beyond the run method.
-	When JVM exits, all daemon threads are abandoned. Finally blocks are not executed and stack is not unwounded. **Hence, use daemon threads sparingly.**
-	Threads can be created in two ways
	-	Extending the Thread class
		-	Extend the class and implement the run() method. 
		-	Thread's life cycle begins in the run method.
		- 	Calling start() method of the subclass instance will start the thread.
		-	**Why call start() instead of run()?**
			-	Start() method will create a new thread by allocating a new stack and run() is then called in the context of the new thread.
			-	If runt() is called directly, the method will run in the context of the current thread, rather than the new thread.
	-	Implementing the Runnable interface
		-	The implementing class should implement run() method.
		-	An instance of this class is then passed to the Thread constructor.

```java
class Thread1 extends Thread {
    @Override
    public void run() {
        System.out.println("Running thread from Thread1 " + this.getId());
    }
}

class Thread2 implements Runnable{
    @Override
    public void run() {
        System.out.println("Running thread from Thread2");
    }
}
public class ThreadDemo {
    public static void main(String[] args) {
        Thread1 t1 = new Thread1();
        t1.start();

        new Thread(new Thread2()).start();
    }
}
```

-	Implementing Runnable interface is easier and flexible because the subclass can extend any class other than Thread class.
-	Threads can be suspended using sleep() method. This method yields the cpu to other threads.
-	Threads in sleep can be interrupted, so we can't assume that the thread will be asleep for precisely the time specified.
-	A thread sends an interrupt by invoking interrupt() on the Thread object for the thread to be interrupted.
-	The static method Thread.interrupted() can be used to check if the thread has been interrupted.
-	The interrupt mechanism is implemented using an internal flag known as the interrupt status. Invoking Thread.interrupt sets this flag.
-	When a thread checks for an interrupt by invoking the static method Thread.interrupted, interrupt status is cleared.
-	By convention, any method that exits by throwing an InterruptedException clears interrupt status when it does so.
-	The join() method allows one thread to wait for the completion of another.

### JVM, memory management, concurrent garbage collection

### DDL vs DML

### Scalability with low latency

### SQL vs NoSQL

### Data modeling

### Client-server networking concepts

### Design Patterns