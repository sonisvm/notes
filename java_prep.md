
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
			-	If run() is called directly, the method will run in the context of the current thread, rather than the new thread.
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

#### Synchronization
-	 Java provides two basic synchronization idioms: synchronized methods and synchronized statements.
-	Constructors cannot be synchronized.
-	Final fields, which cannot be modified after the object is constructed, can be safely read through non-synchronized methods, once the object is constructed.
-	Every object and every class has an intrinsic lock associated with it.
-	When a thread invokes a synchronized method, it automatically acquires the intrinsic lock for that method's object and releases it when the method returns.
-	When a static synchronized method is invoked, the intrinsic lock associated with the class is acquired.
-	 Synchronized statements must specify the object that provides the intrinsic lock.
-	A thread can acquire a lock that it already owns - reentrant synchronization.
- 	Reads and writes are atomic for reference variables and for most primitive variables (all types except long and double).
-	Reads and writes are atomic for all variables declared volatile (including long and double variables).
-	Changes to a volatile variable are always visible to other threads, thus reducing memory consistency errors.
-	All writes to a volatile variable are written to main memory, bypassing cache, and all reads are from the main memory.
-	When a volatile variable is written to memory, all the variables updated till the volatile variable are also written back to memory. Similarly, when a volatile variable is read, all the variables visible to the thread till then are read from memory.

#### Immutable Classes
-	The internal state does not change after construction.
-	Don't provide "setter" methods — methods that modify fields or objects referred to by fields.
-	Make all fields final and private.
-	Don't allow subclasses to override methods. The simplest way to do this is to declare the class as final. A more sophisticated approach is to make the constructor private and construct instances in factory methods.
-	Don't share references to the mutable objects. Never store references to external, mutable objects passed to the constructor; if necessary, create copies, and store references to the copies.
-	Create copies of your internal mutable objects when necessary to avoid returning the originals in your methods.

### JVM
-	JVM is an abstract computing machine.
-	It has an instruction set and manipulates areas of memory at run time.
-	The Java Virtual Machine knows nothing of the Java programming language, only of a particular binary format, the **class** file format. A class file contains Java Virtual Machine instructions (or bytecodes) and a symbol table, as well as other ancillary information.
-	The Java Virtual Machine imposes strong syntactic and structural constraints on the code in a class file. However, any language with functionality that can be expressed in terms of a valid **class** file can be hosted by the Java Virtual Machine.
-	Class Loader subsystem
	-	Class loader subsystem loads, links and initializes class files when the class is referred for the first time at runtime - dynamic class loading.
	-	The class loading process starts from loading the main class.
	-	There are three class loaders - Bootstrap class loader, Extension class loader and Application class loader.
	-	Bootstrap class loader loads the core Java classes.
	-	Extension class loader loads class files from extensions directory, e.g. security extensions.
	-	Application class loader loads application specific classes from the system class path.

### DDL vs DML

### Scalability with low latency

### SQL vs NoSQL

### Data modeling

### Client-server networking concepts

### Design Patterns
