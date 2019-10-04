
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

### Java Memory Management

-	JVM memory is divided into multiple parts: Heap Memory, Non-Heap Memory, and Other.
-	Heap memory is the run time data area from which the memory for all java class instances and arrays is allocated.
-	Non-Heap memory is created at the JVM startup and stores per-class structures such as runtime constant pool, field and method data, and the code for methods and constructors.
-	Other memory is used by JVM to store the JVM code itself, JVM internal structures, loaded profiler agent code and data, etc.
-	The JVM heap is physically divided into two parts (or generations): nursery (or young space/young generation) and old space (or old generation).
	-	The nursery is a part of the heap reserved for allocation of new objects.
	-	The reasoning behind a nursery is that most objects are temporary and short lived.
	-	When the nursery becomes full, garbage is collected by running a special young collection, where all the objects that have lived long enough in the nursery are promoted (moved) to the old space. This garbage collection is called Minor GC.
	-	The nursery is divide into three parts –one Eden Memory and two Survivor Memory spaces.
	-	Most of the newly created objects are located in the Eden Memory space.
	-	When Eden space is filled with objects, Minor GC is performed and all the survivor objects are moved to one of the survivor spaces.
	-	Minor GC also checks the survivor objects and moves them to the other survivor space. So at a time, one of the survivor space is always empty.
	-	Objects that have survived many cycles of GC, are moved to the old generation memory space.
	-	When the old generation becomes full, garbage is collected there and the process is called as old collection.
	-	Old generation garbage collection is called as Major GC and usually takes longer time.
-	Metaspace contains the application metadata required by the JVM to describe the classes and methods used in the application.
-	Metaspace is not part of the heap.
-	Memory Pools are created by JVM memory managers to create pool of immutable objects.
-	Runtime constant pool is a per-class runtime representation of constant pool in a class.

### Garbage Collection

- Garbage collection happens in three steps
	-	Marking: This is the first step where garbage collector identifies which objects are in use and which ones are not in use.
		-	During the mark phase, all the objects that are reachable from Java threads, native handlers and other root sources are marked as alive, as well as the objects that are reachable from these objects and so forth.
	-	Normal Deletion: Garbage collector removes the unused objects and reclaims the free space to be allocated to other objects.
	-	Deletion with compacting: For better performance, after deleting unused objects, all the survived objects can be moved to be together. This will increase the performance of allocation of memory to newer objects.

### Design Patterns

-	Singleton Pattern
	-	Singleton pattern restricts the instantiation of a class and ensures that only one instance of the class exists in the Java virtual machine.
-	Factory Pattern
	-	Factory design pattern is used when we have a super class with multiple sub-classes and based on input, we need to return one of the sub-class.
-	Builder Pattern
	-	Setter methods will return the handle to the instance so that you can call further setters on it. Useful if the class has multiple parameters and all of them are not used all the time.

### Data structures in Java

#### Lists

| Structure  | Order | Thread-safe | Equals & hashCode | Permits null? | 
| ------------- | ------------- | ------------- | ------------- | ------------- |
| ArrayList | Insertion order  | No | Deep check | Yes |
| CopyOnWriteArrayList  | Insertion order  | Yes | Deep check | Yes |
| LinkedList  | Insertion order | No | Deep check | Yes |

List                 | Add  | Remove | Get  | Contains | Next | Data Structure
---------------------|------|--------|------|----------|------|---------------
ArrayList            | O(1) |  O(n)  | O(1) |   O(n)   | O(1) | Array
LinkedList           | O(1) |  O(1)  | O(n) |   O(n)   | O(1) | Linked List
CopyOnWriteArrayList | O(n) |  O(n)  | O(1) |   O(n)   | O(1) | Array

#### Maps

| Structure  | Order | Thread-safe | Equals & hashCode | Permits null? | 
| ------------- | ------------- | ------------- | ------------- | ------------- |
| TreeMap | Sorted by keys  | No | Deep check | Depends on the comparator |
| ConcurrentHashMap  | None  | Yes | Deep check | No |
| HashMap  | None | No | Deep check | Yes |
| LinkedHashMap  | Insertion order | No | Deep check | Yes |


Map                   |   Get    | ContainsKey |   Next   | Data Structure
----------------------|----------|-------------|----------|-------------------------
HashMap               | O(1)     |   O(1)      | O(h / n) | Hash Table
LinkedHashMap         | O(1)     |   O(1)      | O(1)     | Hash Table + Linked List
TreeMap               | O(log n) |   O(log n)  | O(log n) | Red-black tree
ConcurrentHashMap     | O(1)     |   O(1)      | O(h / n) | Hash Tables

Note: h means current capacity of the hash collection.


