

# 10 - Annotation

Which of the following annotations are retained for run time?
@SafeVarargs
@FunctionalInterface
@Deprecated

# 18 - Concurrency

Callable Vs Runnable
- A Callable cannot be passed to Thread for Thread creation but a Runnable can be. i.e. new Thread( aRunnable ); is valid. But new Thread( aCallable ); is not.
Therefore, if you want to execute a task directly in a Thread, a Callable is not suitable. You will need to use a Runnable.
You can achieve the same by using an ExecutorService.submit( aCallable ), but in this case, you are not controlling the Thread directly.

- Both Callable and Runnable can be used to execute a task asynchronously. If the task does not return any result, neither is more appropriate than the other. However, if the task returns a result, which you want to collect asynchronously later, Callable is more appropriate.

- Both can be used with an ExecutorService because ExecutorService has overloaded submit methods: Observe that even though a Runnable's run() method cannot return a value, the ExecutorService.submit(Runnable) returns a Future. The Future's get method will return null upon successful completion

```java
<T> Future<T> submit(Callable<T> task)
Future<?> submit(Runnable task)
```
 



- public interface Callable<V>
   A task that returns a result and may throw an exception. Implementers define a single method with no arguments called call -
   V call() throws Exception

   The Callable interface is similar to Runnable, in that both are designed for classes whose instances are potentially executed by another thread. A Runnable, however, does not return a result and cannot throw a checked exception.


<br/> 

## Deadlock, Starvation, and Livelock

1. Deadlock describes a situation where two or more threads are blocked forever, waiting for each other. For example, two threads T1 and T2 need a File and a Printer. T1 acquires the lock for the file and is about to acquire the lock for the Printer but before it could acquire the lock, T2 acquires the lock for the Printer and tries to acquire the lock for the file (which is already held by T1). So now, both the threads keep waiting for ever for each other to release their locks and neither will be able to proceed.

2. Starvation describes a situation where a thread is unable to gain regular access to shared resources and is unable to make progress. This happens when shared resources are made unavailable for long periods by "greedy" threads. For example, suppose an object provides a synchronized method that often takes a long time to return. If one thread invokes this method frequently, other threads that also need frequent synchronized access to the same object will often be blocked.

3. Livelock: A thread often acts in response to the action of another thread. If the other thread's action is also a response to the action of another thread, then livelock may result. As with deadlock, livelocked threads are unable to make further progress. However, the threads are not blocked — they are simply too busy responding to each other to resume work. For example, after acquiring the File lock, T1 tries to acquire the Printer lock. Finding the Printer lock to be already taken, it releases the lock for the File and notifies T2. At the same time, T2 tries to acquire the File lock and seeing that it is already taken it releases Printer lock and notifies T1. This process can go on and on, both the threads releasing and acquiring the locks in tandem but none of them getting both the locks at the same time. So neither of the threads is blocked but neither of the threads is able to do any real work. All they are doing is notifying each other.

**Starvation**  
Starvation occurs when one thread cannot access the CPU because one or more other threads are monopolizing the CPU. Thread starvation may be caused because of different thread priorities. A lower-priority thread can be starved by higher-priority threads if the higher-priority threads do not yield control of the CPU from time to time.


**Thrashing**  
Thrashing occurs when a program makes little or no progress because threads perform excessive context switching. This may leave little or no time for the application code to execute.

# 21 - JDBC

   A Connection is always in auto-commit mode when it is created.  As per the problem statement, an update was fired without explicitly disabling the auto-commit mode, the changes will be committed right after the update statement has finished execution. 

   When a connection is created, it is in auto-commit mode. i.e. auto-commit is enabled. This means that each individual SQL statement is treated as a transaction and is automatically committed right after it is completed. (A statement is completed when all of its result sets and update counts have been retrieved. In almost all cases, however, a statement is completed, and therefore committed, right after it is executed.)

The way to allow two or more statements to be grouped into a transaction is to disable the auto-commit mode. Since it is enabled by default, you have to explicitly disable it after creating a connection by calling con.setAutoCommit(false);

- When a JDBC Connection is created, it is in auto-commit mode.
- A CallableStatement is easier to build and call from JDBC code than a PreparedStatement.
- Starting JDBC 4.0, the JDBC Driver class is not required to be loaded explicitly in the code any more.


# General

1. Only String, byte, char, short, int, (and their wrapper classes Byte, Character, Short, and Integer), and enums can be used as types of a switch variable.


2. TreeMap -> NavigatedMap -> SortedMap -> Map

3. **Console object be acquired case** : When the JVM is started from an interactive command line without redirecting the standard input and output streams.



## Wrapper Classes

Byte, Character, Short, Integer, Long, Float, and Double.  


Note that Byte, Short, Integer, Long, Float and Double extend from Number which is an abstract class. An object of type Double, for example, contains a field whose type is double, representing that value in such a way that a reference to it can be stored in a variable of reference type. These classes also provide a number of methods for converting among primitive values, as well as supporting such standard methods as equals and hashCode.  

- Wrapper classes are immutable.

<br/>

## integral types 

byte, short, int, long, and char



# OOP

- Non-static nested classes (aka inner classes) can contain final static fields (but not static methods).

- Anonymous classes cannot be static.


---- 

A method can be overridden by defining a method with the same signature(i.e. name and parameter list) and return type as the method in a superclass. The return type can also be a subclass of the original method's return type.

Only methods that are accessible can be overridden. A private method cannot, therefore, be overridden in subclasses, but the subclasses are allowed to define a new method with exactly the same signature.

A final method cannot be overridden.

An overriding method cannot exhibit behavior that contradicts the declaration of the original method. An overriding method therefore cannot return a different type (except a subtype) or throw a wider spectrum of exceptions than the original method in the superclass. This, of course, applies only to checked exceptions because unchecked exceptions are not required to be declared at all.

A subclass may have a static method with the same signature as a static method in the base class but it is not called overriding. It is called hiding because the concept of polymorphism doesn't apply to static members.

----- 


# 11 - Module



## provides
The provider module must specify the service interface and the implementing class that implements the service interface.  

For example,
provides org.printservice.api.Print with com.myprinter.PrintImpl

## uses

A uses clause is used by the module that uses a service. 

For example, uses org.printservice.api.Print;
 
 <br/>

**module definition that implements a service :**  

 For example, if an abc.print module implements an org.printing.Print service interface defined in PrintServiceAPI module using com.abc.PrintImpl class, then this is how its module-info should look:

```java
module abc.print{
   requires PrintServiceAPI; //required because this module defines the service interface org.printing.Print
   provides org.printing.Print with com.abc.PrintImpl;
}
```
A module named customer that uses Print service may look like this:
```java
module customer{
   requires PrintServiceAPI; //required because this module defines the service interface org.printing.Print
   uses org.printing.Print; //specifies that this module uses this service

   //observe that abc.print module is not required.
}
```



Bottom Up Approach for modularizing an application

While modularizing an app using the bottom-up approach, you basically need to convert lower level libraries into modular jars before you can convert the higher level libraries. For example, if a class in A.jar directly uses a class from B.jar, and a class in B.jar directly uses a class from C.jar, you need to first modularize C.jar and then B.jar before you can modularize A.jar.

Thus, bottom up approach is possible only when the dependent libraries are modularized already.

Top Down Approach for modularzing an application

While modularizing an app in a top-down approach, you need to remember the following points -

1. Any jar file can be converted into an automatic module by simply putting that jar on the module-path instead of the classpath. Java automatically derives the name of this module from the name of the jar file.

2. Any jar that is put on classpath (instead of module-path) is loaded as a part of the "unnamed" module.

3. An explicitly named module (which means, a module that has an explicitly defined name in its module-info.java file) can specify dependency on an automatic module just like it does for any other module i.e. by adding a requires <module-name>; clause in its module info but it cannot do so for the unnamed module because there is no way to write a requires clause without a name.  In other words, a named module can access classes present in an automatic module but not in the unnamed module.

4. Automatic modules are given access to classes in the unnamed module (even though there is no explicitly defined module-info and requires clause in an automatic module). In other words, a class from an automatic module will be able to read a class in the unnamed module without doing anything special.

5. An automatic module exports all its packages and is allowed to read all packages exported by other modules. Thus, an automatic module can access: all packages of all other automatic modules + all packages exported by all explicitly named modules + all packages of the unnamed module (i.e. classes loaded from the classpath).


Thus, if your application jar A directly uses a class from another jar B, then you would have to convert B into a module (either named or automatic). If B uses another jar C, then you can leave C on the class path if B hasn't yet been migrated into a named module. Otherwise, you would have to convert C into an automatic module as well.

Note:
There are two possible ways for an automatic module to get its name:
1. When an Automatic-Module-Name entry is available in the manifest, its value is the name of the automatic module.
2. Otherwise, a name is derived from the JAR filename (see the ModuleFinder JavaDoc for the derivation algorithm) - Basically, hyphens are converted into dots and the version number part is ignored. So, for example, if you put mysql-connector-java-8.0.11.jar on module path, its module name would be mysql.connector.java




## About JDK

- **The foundational APIs of the Java SE platform are found in java.base module.**  
*java.base module defines the "foundational APIs" of the Java SE Platform. The exam also uses the term "core packages" for the same.*

- **The modular JDK is divided of two kinds of modules - the standard modules and the non-standard module**

The modular structure of the JDK implements the following principles:
  1. Standard modules, whose specifications are governed by the JCP, have names starting with the string "java.".

  2. All other modules are merely part of the JDK, and have names starting with the string "jdk."


There are several modules in the Java SE platform. These modules are categorized into two categories - java se and jdk. These two are not really modules but just names of two categories.

Java SE
The Java Platform, Standard Edition (Java SE) APIs define the core Java platform for general-purpose computing. These APIs are in modules whose names start with java. It contains modules such as: java.base, java.logging, java.sql, java.desktop, java.xml, and java.se.

JDK
The Java Development Kit (JDK) APIs are specific to the JDK and will not necessarily be available in all implementations of the Java SE Platform. These APIs are in modules whose names start with jdk. It contains modules such as: jdk.accessibility, jdk.javadoc, jdk.jartool, jdk.jlink, and jdk.net.




# Annotations
## Meta Annotations
 - @Retention 
 - @Documented 
 - @Target
 - @Inherited
 - @Repeatable



<br/> 

# Localizations

🟡 When a resource bundle is not found for a given locale, the default locale is used to load the resource bundle.


# FOCUS


1- Deadlock, Starvation, Livelock, Thrashing incele ?  
2- Automatic, unnamed and named modules ?
3- Stream Collectors