# Unit 18 - Concurrency

## Introduction
<br/>

### **RUNNABLE**

<br/>

```java
@FunctionalInterface public interface Runnable {
   void run();
}
```

<br/>

**Example :**
```java
Runnable sloth = () -> System.out.println("Hello World");
Runnable snake = () -> {int i=10; i++;};
Runnable beaver = () -> {return;};
Runnable coyote = () -> {};
```

<br/>

**Example :** Runnable doesn't return value
```java
Runnable capybara = () -> "";                 // DOES NOT COMPILE
Runnable Hippopotamus = () -> 5;              // DOES NOT COMPILE
Runnable emu = () -> {return new Object();};  // DOES NOT COMPILE
```

<br/>

**Another Way To Implement Runnable:**
```java
public class CalculateAverage implements Runnable {
       public void run() {
          // Define work here
       }
    }
```


<br/>

### **THREAD**

package is : *java.lang.Thread*  

Java does not provide any guarantees about the order in which a thread will be processed once it is started.

<br/>


Defining the task that a Thread instance will execute can be done two ways in Java:

- Provide a *Runnable* object or lambda expression to the *Thread* constructor.
- Create a class that extends *Thread* and overrides the *run()* method.

```java
public class PrintData implements Runnable {
   @Override public void run() { // Overrides method in Runnable
      for(int i = 0; i < 3; i++)
         System.out.println("Printing record: "+i);
   }
   public static void main(String[] args) {
      (new Thread(new PrintData())).start();
   }
}
 
public class ReadInventoryThread extends Thread {
   @Override public void run() { // Overrides method in Thread
      System.out.println("Printing zoo inventory");
   }
   public static void main(String[] args) {
      (new ReadInventoryThread()).start();
   }
}
```

<br/>

**Example :**
```java
public static void main(String[] args) {
    System.out.println("begin");
    (new ReadInventoryThread()).start();
    (new Thread(new PrintData())).start();
    (new ReadInventoryThread()).start();
    System.out.println("end");
}
```
**Prints :** The answer is that it is unknown until runtime. The following is just one possible output:
```
begin
Printing zoo inventory
Printing record: 0
end
Printing zoo inventory
Printing record: 1
Printing record: 2
```

<br/>


#### **CALLING RUN() INSTEAD OF START()**

**Example :**
```java
System.out.println("begin");
(new ReadInventoryThread()).run();
(new Thread(new PrintData())).run();
(new ReadInventoryThread()).run();
System.out.println("end");
```
Unlike the previous example, each line of this code will wait until the run() method is complete before moving on to the next line
**Example :**
```java

```

<br/>

## Concurrency API

The *Concurrency API* includes the *ExecutorService* interface, which defines services that create and manage threads for you.


**Example :**
```java
import java.util.concurrent.*;
public class ZooInfo {
   public static void main(String[] args) {
      ExecutorService service = null;
      Runnable task1 = () ->
         System.out.println("Printing zoo inventory");
      Runnable task2 = () -> {for(int i = 0; i < 3; i++)
            System.out.println("Printing record: "+i);};
      try {
         service = Executors.newSingleThreadExecutor();
         System.out.println("begin");
         service.execute(task1);
         service.execute(task2);
         service.execute(task1);
         System.out.println("end");
      } finally {
         if(service != null) service.shutdown();
      }
   }
}
```
**One of possible output :**
```
begin
Printing zoo inventory
Printing record: 0
Printing record: 1
end
Printing record: 2
Printing zoo inventory
```
With a single‐thread executor, results are guaranteed to be executed sequentially. Why last output is not sequentially? This is because the *main()* method is still an independent thread from the *ExecutorService*.

<br/>

📺 **ExecutorService methods :**
|Method name|Description|
|--- |--- |
|void execute(Runnable command)|Executes a Runnable task at some point in the future|
|Future\<?> submit(Runnable task)|Executes a Runnable task at some point in the future and returns a Future representing the task|
|\<T> Future\<T> submit(Callable\<T> task)|Executes a Callable task at some point in the future and returns a Future representing the pending results of the task|
|\<T> List\<Future\<T>> invokeAll(Collection\<? extends Callable\<T>> tasks) throws InterruptedException|Executes the given tasks and waits for all tasks to complete. Returns a List of Future instances, in the same order they were in the original collection|
|\<T> T invokeAny(Collection<? extends Callable\<T>> tasks) throws InterruptedException, ExecutionException|Executes the given tasks and waits for at least one to complete. Returns a Future instance for a complete task and cancels any unfinished tasks|


<br/>
<br/>


### **WAITING FOR RESULTS**

```java
Future<?> future = service.submit(() -> System.out.println("Hello"));
```
<br/>

📺 **Future methods :**

|Method name|Description|
|--- |--- |
|boolean isDone()|Returns true if the task was completed, threw an exception, or was cancelled|
|boolean isCancelled()|Returns true if the task was cancelled before it completed normally|
|boolean cancel(boolean mayInterruptIfRunning)|Attempts to cancel execution of the task and returns true if it was successfully cancelled or false if it could not be cancelled or is complete|
|V get()|Retrieves the result of a task, waiting endlessly if it is not yet available|
|V get(long timeout, TimeUnit unit)|Retrieves the result of a task, waiting the specified amount of time. If the result is not ready by the time the timeout is reached, a checked TimeoutException will be thrown.|
<br/>
<br/>

### **Callable**

The *java.util.concurrent.Callable* functional interface is similar to *Runnable* except that its *call()* method returns a value and can throw a checked exception.

```java
@FunctionalInterface public interface Callable<V> {
   V call() throws Exception;
}
```
<br/>

### **Runnable Vs Callable :**
```java
ExecutorService service = null;
try {
    Runnable runnable = () -> System.out.println("");   // no return value
    Callable callable = () -> 30*20;                    // Return <T>

    service = Executors.newSingleThreadExecutor();

    Future<?> submitRunnable = service.submit(runnable);
    Future<Integer> submitCallable = service.submit(callable);

    System.out.println(submitRunnable.get());   // always null
    System.out.println(submitCallable.get());   //  600
} finally {
    if(service != null) service.shutdown();
}
```

<br/>

### **Waiting for All Tasks to Finish**

First, we shut down the thread executor using the *shutdown()* method. Next, we use the *awaitTermination()* method available for all thread executors.

The method waits the specified time to complete all tasks, returning sooner if all tasks finish or an *InterruptedException* is detected.  

```java
ExecutorService service = null;
try {
   service = Executors.newSingleThreadExecutor();
   // Add tasks to the thread executor
   …
} finally {
   if(service != null) service.shutdown();
}
if(service != null) {
   service.awaitTermination(1, TimeUnit.MINUTES);
 
   // Check whether all tasks are finished
   if(service.isTerminated()) System.out.println("Finished!");
   else System.out.println("At least one task is still running");
}
```
If *awaitTermination()* is called before *shutdown()* within the same thread, then that thread will wait the full timeout value sent with* awaitTermination()*.

<br/>

### **SUBMITTING TASK COLLECTIONS**

**invokeAll() , invokeAny()**: Execute synchronously and take a Collection of tasks. These methods will wait until the results are available before returning control to the enclosing program

```java
ExecutorService service = null;
try {
    Callable<String> call1 = () -> "Call 1";
    Callable<String> call2 = () -> "Call 2";
    Callable<String> call3 = () -> "Call 3";
    List<Callable<String>> callList = List.of(call1, call2, call3);

    System.out.println("begin");

    service = Executors.newSingleThreadExecutor();
    List<Future<String>> futureList = service.invokeAll(callList);

    for (Future future : futureList) {
        System.out.println("future.get() = " + future.get());
    }

    String any = service.invokeAny(callList);
    System.out.println("any = " + any);

    System.out.println("End");
} finally {
    if (service != null) service.shutdown();
}
```
**Print :**
```java
begin
future.get() = Call 1
future.get() = Call 2
future.get() = Call 3
any = Call 1
End
```

<br/>

### **SCHEDULING TASKS**
<br/>

 The *ScheduledExecutorService*, which is a subinterface of *ExecutorService*, can be used for just such a task.
 We might even need to schedule the task to happen repeatedly, at some set interval.


```java
ScheduledExecutorService service = Executors.newSingleThreadScheduledExecutor();
```
<br/>

📺 **ScheduledExecutorService methods :**

|Method Name|Description|
|--- |--- |
|schedule(Callable\<V> callable, long delay, TimeUnit unit)|Creates and executes a Callable task after the given delay|
|schedule(Runnable command, long delay, TimeUnit unit)|Creates and executes a Runnable task after the given delay|
|scheduleAtFixedRate(Runnable command, long initialDelay, long period, TimeUnit unit)|Creates and executes a Runnable task after the given initial delay, creating a new task every period value that passes|
|scheduleWithFixedDelay(Runnable command, long initialDelay, long delay, TimeUnit unit)|Creates and executes a Runnable task after the given initial delay and subsequently with the given delay between the termination of one execution and the commencement of the next|

<br/>

<br/>

**Example :**
```java
ScheduledExecutorService service = Executors.newSingleThreadScheduledExecutor();
Runnable task1 = () -> System.out.println("Hello Zoo");
Callable<String> task2 = () -> "Monkey";
ScheduledFuture<?> r1 = service.schedule(task1, 10, TimeUnit.SECONDS);
ScheduledFuture<?> r2 = service.schedule(task2, 8,  TimeUnit.MINUTES);
```
The first task is scheduled 10 seconds in the future, whereas the second task is scheduled 8 minutes in the future.

<br/>
<br/>

**Example 2:** *The scheduleAtFixedRate()* method creates a new task and submits it to the executor every period, regardless of whether the previous task finished

```java
service.scheduleAtFixedRate(command, 5, 1, TimeUnit.MINUTES);
```
The *scheduleAtFixedRate()* method is useful for tasks that need to be run at specific intervals, such as checking the health of the animals once a day.

<br/>

<br/>

### **INCREASING CONCURRENCY WITH POOLS**
<br/>


📺 **Executors factory methods :**
|Method|Description|
|--- |--- |
|ExecutorService   newSingleThreadExecutor()|Creates a single‐threaded executor that uses a single worker thread operating off an unbounded queue. Results are processed sequentially in the order in which they are submitted.|
|ScheduledExecutorService   newSingleThreadScheduledExecutor()|Creates a single‐threaded executor that can schedule commands to run after a given delay or to execute periodically|
|ExecutorService   newCachedThreadPool()|Creates a thread pool that creates new threads as needed but will reuse previously constructed threads when they are available|
|ExecutorService   newFixedThreadPool(int)|Creates a thread pool that reuses a fixed number of threads operating off a shared unbounded queue|
|ScheduledExecutorService   newScheduledThreadPool(int)|Creates a thread pool that can schedule commands to run after a given delay or to execute periodically|

<br/>
<br/>

 - While a *single‐thread* executor will wait for a thread to become available before running the next task, a *pooled‐thread* executor can execute the next task concurrently. 

- Calling *newFixedThreadPool()* with a value of 1 is equivalent to calling *newSingleThreadExecutor()*. 

- The *newCachedThreadPool()* method will create a thread pool of unbounded size, allocating a new thread anytime one is required or all existing threads are busy.

- The *newScheduledThreadPool()* is identical to the *newFixedThreadPool()* method, except that it returns an instance of *ScheduledExecutorService* and is therefore compatible with scheduling tasks.
<br/>
<br/>

## Writing Thread‐Safe Code
<br/>

### **ATOMIC CLASSES**

<br/>

📺 **Atomic classes :**
|Class Name|Description|
|--- |--- |
|AtomicBoolean|A boolean value that may be updated atomically|
|AtomicInteger|An int value that may be updated atomically|
|AtomicLong|A long value that may be updated atomically|

<br/>


📺 **Common atomic methods :**

|Method name|Description|
|--- |--- |
|get()|Retrieves the current value|
|set()|Sets the given value, equivalent to the assignment = operator|
|getAndSet()|Atomically sets the new value and returns the old value|
|incrementAndGet()|For numeric classes, atomic pre‐increment operation equivalent to ++value|
|getAndIncrement()|For numeric classes, atomic post‐increment operation equivalent to value++|
|decrementAndGet()|For numeric classes, atomic pre‐decrement operation equivalent to ‐‐value|
|getAndDecrement()|For numeric classes, atomic post‐decrement operation equivalent to value‐‐|

<br/>

<br/>

### **SYNCHRONIZED**
<br/>

**Example :**  Two method definitions are equivalent.
```java
private void incrementAndReport() {
   synchronized(this) {
      System.out.print((++sheepCount)+" ");
   }
}
private synchronized void incrementAndReport() {
   System.out.print((++sheepCount)+" ");
}
```

<br/>

### **LOCK FRAMEWORK**
<br/>

#### <u>**ReentrantLock Interface**</u>
<br/>

*ReentrantLock* class supports the same features as a synchronized block, while adding a number of improvements.

- Ability to request a lock without blocking
- Ability to request a lock while blocking for a specified amount of time
- A lock can be created with a fairness property, in which the lock is granted to threads in the order it was requested.

<br/>

**Implementation #1 with a synchronized block :** 
<br/>

```java
Object object = new Object();
synchronized(object) {
   
// Protected code
}
```  
<br/>

**Implementation #2 with a Lock**
```java
Lock lock = new ReentrantLock();
try {
   lock.lock();
   
// Protected code
} finally {
   lock.unlock();
}
```
<br/>

📺 **Lock Methods**  
|Method|Description|
|--- |--- |
|void lock()|Requests a lock and blocks until lock is acquired|
|void unlock()|Releases a lock|
|boolean tryLock()|Requests a lock and returns immediately. Returns a boolean indicating whether the lock was successfully acquired|
|boolean tryLock(long,TimeUnit)|Requests a lock and blocks up to the specified time until lock is required. Returns a boolean indicating whether the lock was successfully acquired|

<br/>
<br/>

#### **Duplicate Lock Requests**

```java
Lock lock = new ReentrantLock();
if(lock.tryLock()) {
   try {
      lock.lock();
      System.out.println("Lock obtained, entering protected code");
   } finally {
      lock.unlock();
   }
}
```
The thread obtains the lock twice but releases it only once.



### **CYCLICBARRIER**

CyclicBarrier class coordinate to the tasks.

**Example 1:** Without CyclicBarrier
```java
public class LionPenManager {
    private void removeLions() {
        System.out.println("Removing lions");
    }

    private void cleanPen() {
        System.out.println("Cleaning the pen");
    }

    private void addLions() {
        System.out.println("Adding lions");
    }

    public void performTask() {
        removeLions();
        cleanPen();
        addLions();
    }

    public static void main(String[] args) {
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(4);
            var manager = new LionPenManager();
            for (int i = 0; i < 4; i++)
                service.submit(() -> manager.performTask());
        } finally {
            if (service != null) service.shutdown();
        }
    }
}
```
**sample output :**
```
Removing lions
Removing lions
Cleaning the pen
Removing lions
Cleaning the pen
Adding lions
Removing lions
Cleaning the pen
Adding lions
Adding lions
Cleaning the pen
Adding lions
```
<br/>

**Example 2:** Uses *CyclicBarrier*
```java
public class LionPenManager {
    private void removeLions() {
        System.out.println("Removing lions");
    }

    private void cleanPen() {
        System.out.println("Cleaning the pen");
    }

    private void addLions() {
        System.out.println("Adding lions");
    }

    public void performTask(CyclicBarrier c1, CyclicBarrier c2) {
        try {
            removeLions();
            c1.await();
            cleanPen();
            c2.await();
            addLions();
        } catch (InterruptedException | BrokenBarrierException e) {
            // Handle checked exceptions here
        }
    }

    public static void main(String[] args) {
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(4);
            var manager = new LionPenManager();
            var c1 = new CyclicBarrier(4);
            var c2 = new CyclicBarrier(4,
                    () -> System.out.println("*** Pen Cleaned!"));
            for (int i = 0; i < 4; i++)
                service.submit(() -> manager.performTask(c1, c2));
        } finally {
            if (service != null) service.shutdown();
        }
    }
}
```
<br/>

**Output :**
```
Removing lions
Removing lions
Removing lions
Removing lions
Cleaning the pen
Cleaning the pen
Cleaning the pen
Cleaning the pen
*** Pen Cleaned!
Adding lions
Adding lions
Adding lions
Adding lions
```

<br/>
<br/>

#### **THREAD POOL SIZE AND CYCLIC BARRIER LIMIT**

Number of available threads must to be at least as large as  CyclicBarrier limit value. If not, the code will hang indefinitely. The barrier would never be reached as the only threads available in the pool are stuck waiting for the barrier to be complete. This would result in a deadlock.

```java
ExecutorService service = Executors.newFixedThreadPool(2);
var c1 = new CyclicBarrier(4);
```

<br/>

#### **REUSING CYCLICBARRIER** 

If CyclicBarrier limit is 5 and have 15 threads that call await(), then the CyclicBarrier will be activated a total of three times.

<br/>
<br/>

## Using Concurrent Collections

### MEMORY CONSISTENCY ERRORS

When two threads try to modify the same nonconcurrent collection, the JVM may throw a *ConcurrentModificationException* at runtime.

Tt can happen with a single thread.

```java
var foodData = new HashMap<String, Integer>();
foodData.put("penguin", 1);
foodData.put("flamingo", 2);
for(String key: foodData.keySet())
   foodData.remove(key);
```
**Print :**
```java
Exception in thread "main" java.util.ConcurrentModificationException
	at java.base/java.util.HashMap$HashIterator.nextNode(HashMap.java:1493)
	at java.base/java.util.HashMap$KeyIterator.next(HashMap.java:1516)
	at concurrency.collections.App1.main(App1.java:10)
```

It throw ConcurrentModificationException while second iteration of loop, since the iterator on keySet() is not properly updated after the first element is removed. 

Changing the first line to use a ConcurrentHashMap will prevent the code from throwing an exception at runtime. 

<br/>

```java
var foodData = new ConcurrentHashMap<String, Integer>();
foodData.put("penguin", 1);
foodData.put("flamingo", 2);
for(String key: foodData.keySet())
    foodData.remove(key);
```

*ConcurrentHashMap* is ordering read/write access such that all access to the class is consistent. The iterator created by *keySet()* is updated as soon as an object is removed from the *Map*. 

<br/>
<br/>

### **CONCURRENT CLASSES**
<br/>

🟡 **Note :** Immutable objects can be accessed by any number of threads and do not require synchronization. By definition, they do not change, so there is no chance of a memory consistency error.


📺 **Concurrent collection classes**

|Class name|Java Collections Framework interfaces|Elements ordered?|Sorted?|Blocking?|
|--- |--- |--- |--- |--- |
|ConcurrentHashMap|ConcurrentMap|No|No|No|
|ConcurrentLinkedQueue|Queue|Yes|No|No|
|ConcurrentSkipListMap|ConcurrentMap SortedMap NavigableMap|Yes|Yes|No|
|ConcurrentSkipListSet|SortedSet NavigableSet|Yes|Yes|No|
|CopyOnWriteArrayList|List|Yes|No|No|
|CopyOnWriteArraySet|Set|No|No|No|
|LinkedBlockingQueue|BlockingQueue|Yes|No|Yes|

<br/>
<br/>

**Example :**
```java
Map<String,Integer> map = new ConcurrentHashMap<>();
map.put("zebra", 52);
map.put("elephant", 10);
System.out.println(map.get("elephant"));  // 10
 
Queue<Integer> queue = new ConcurrentLinkedQueue<>();
queue.offer(31);
System.out.println(queue.peek());  // 31
System.out.println(queue.poll());  // 31
```
<br/>
<br/>

### **SkipList Collections**
<br/>

The SkipList classes, ConcurrentSkipListSet and ConcurrentSkipListMap, are concurrent versions of their sorted counterparts, TreeSet and TreeMap, respectively.

**Example 1:**
```java
Set<String> gardenAnimals = new ConcurrentSkipListSet<>();
gardenAnimals.add("rabbit");
gardenAnimals.add("cat");
gardenAnimals.add("gopher");
System.out.println(gardenAnimals.stream().collect(Collectors.joining(",")));
```
**Print :**
```
cat,gopher,rabbit
```
<br/>

**Example 2:**
```java
Map<String, String> rainForestAnimalDiet = new ConcurrentSkipListMap<>();
rainForestAnimalDiet.put("koala", "bamboo");
rainForestAnimalDiet.put("cat", "kukoo");
rainForestAnimalDiet.entrySet()
        .stream()
        .forEach((e) -> System.out.println(e.getKey() + "-" + e.getValue()));  
```
**Print :**
```
cat-kukoo
koala-bamboo
```

🟡 When see *SkipList* or *SkipSet*, just think “sorted” concurrent collections, and the rest should follow naturally.
<br/>
<br/>


### **CopyOnWrite Collections**
<br/>

 - *CopyOnWriteArrayList*  
 - *CopyOnWriteArraySet*


<br/>

**Example 1:**
```java
List<Integer> favNumbers = new CopyOnWriteArrayList<>(List.of(4,3,42));

for(var n: favNumbers) {
   System.out.print(n + " ");
   favNumbers.add(9);
}
 
System.out.println("\nSize: " + favNumbers.size());
```
<br/>

**Print :**
```java
4 3 42
Size: 6
```
<br/>

**Example 2:**
```java
Set<Character> favLetters = new CopyOnWriteArraySet<>(List.of('a','t'));

for(char c: favLetters) {
   System.out.print(c+" ");
   favLetters.add('s');
}

System.out.println("\nSize: "+ favLetters.size());
```
<br/>

**Print :**
```java
a t 
Size: 3
```
<br/>

### **Blocking Queues**
<br/>

The *BlockingQueue* is just like a regular Queue, except that it includes methods that will wait a specific amount of time to complete an operation.

|Method name|Description|
|--- |--- |
|offer(E e, long timeout, TimeUnit unit)|Adds an item to the queue, waiting the specified time and returning false if the time elapses before space is available|
|poll(long timeout, TimeUnit unit)|Retrieves and removes an item from the queue, waiting the specified time and returning null if the time elapses before the item is available|

<br/>
<br/>

**Example :**
```java
try {
   var blockingQueue = new LinkedBlockingQueue<Integer>();
   blockingQueue.offer(39);
   blockingQueue.offer(3, 4, TimeUnit.SECONDS);
   System.out.println(blockingQueue.poll());
   System.out.println(blockingQueue.poll(10, TimeUnit.MILLISECONDS));
} catch (InterruptedException e) {
   // Handle interruption
}
```
<br/>

**Print :**
```
39
3
```
<br/>


### **SYNCHRONIZED COLLECTIONS**

<br/>

📺 **Synchronized collections methods**

||
|--- |
|synchronizedCollection(Collection\<T> c)|
|synchronizedList(List\<T> list)|
|synchronizedMap(Map\<K,V> m)|
|synchronizedNavigableMap(NavigableMap\<K,V> m)|
|synchronizedNavigableSet(NavigableSet\<T> s)|
|synchronizedSet(Set\<T> s)|
|synchronizedSortedMap(SortedMap\<K,V> m)|
|synchronizedSortedSet(SortedSet\<T> s)|

<br/>

<br/>

**Example :**
```java
var foodData = new HashMap<String, Object>();
foodData.put("penguin", 1);
foodData.put("flamingo", 2);
var synFoodData = Collections.synchronizedMap(foodData);
for(String key: synFoodData.keySet())
   synFoodData.remove(key);
```
This loop throws a *ConcurrentModificationException*, whereas our example that used *ConcurrentHashMap* did not.

<br/>

```java 
Exception in thread "main" java.util.ConcurrentModificationException
	at java.base/java.util.HashMap$HashIterator.nextNode(HashMap.java:1493)
	at java.base/java.util.HashMap$KeyIterator.next(HashMap.java:1516)
	at concurrency.collections.App4.main(App4.java:12)
```
<br/>
<br/>

## Threading Problems

### **LIVENESS**

*Liveness* is the ability of an application to be able to execute in a timely manner.

#### **Deadlock**

*Deadlock* occurs when two or more threads are blocked forever, each waiting on the other

```java 
class Food {
}

class Water {
}

public class Fox {
    public void eatAndDrink(Food food, Water water) {
        synchronized (food) {
            System.out.println("Got Food!");
            move();
            synchronized (water) {
                System.out.println("Got Water!");
            }
        }
    }

    public void drinkAndEat(Food food, Water water) {
        synchronized (water) {
            System.out.println("Got Water!");
            move();
            synchronized (food) {
                System.out.println("Got Food!");
            }
        }
    }

    public void move() {
        try {
            Thread.sleep(100);
        } catch (InterruptedException e) {
            // Handle exception
        }
    }

    public static void main(String[] args) {
        // Create participants and resources
        Fox foxy = new Fox();
        Fox tails = new Fox();
        Food food = new Food();
        Water water = new Water();
        // Process data
        ExecutorService service = null;
        try {
            service = Executors.newScheduledThreadPool(10);
            service.submit(() -> foxy.eatAndDrink(food, water));
            service.submit(() -> tails.drinkAndEat(food, water));
        } finally {
            if (service != null) service.shutdown();
        }
    }
}
```
<br/>

The result is that our program outputs the following, and it hangs indefinitely:
```
Got Food!
Got Water!
```


<br/>

#### **Starvation**

Starvation occurs when a single thread is perpetually denied access to a shared resource or lock. The thread is still active, but it is unable to complete its work as a result of other threads constantly taking the resource that they are trying to access.


<br/>

#### **Livelock**
Livelock occurs when two or more threads are conceptually blocked forever, although they are each still active and trying to complete their task. Livelock is a special case of resource starvation in which two or more threads actively try to acquire a set of locks, are unable to do so, and restart part of the process. 

Livelock is often a result of two threads trying to resolve a deadlock. 

<br/>
<br/>

## Parallel Streams

### **CREATING PARALLEL STREAMS**

<br/>

```java 
Stream<Integer> s1 = List.of(1,2).stream();
Stream<Integer> s2 = s1.parallel();

``` 
```java
Stream<Integer> s3 = List.of(1,2).parallelStream();
```

<br/>

🟡 The Stream interface includes a method *isParallel()* that can be used to test if the instance of a stream supports parallel processing.


#### **forEachOrdered()**
Similier foreach() but it forces a parallel stream to process the results in order at the cost of performance.

```java
List.of(1,2,3,4,5)
        .parallelStream()
        .map(w -> doWork(w))
        .forEachOrdered(s -> System.out.print(s + " "));
```
**Prints :**
```java
1 2 3 4 5 
```

<br/>


### **PARALLEL REDUCTIONS**

<br/>

```java
System.out.print(List.of(1,2,3,4,5,6)
   .stream()
   .findAny().get());
```
The result is that the output could be 4, 1, or really any value in the stream. You can see that with parallel streams, the results of findAny() are not as predictable.


This code frequently outputs the first value in the serial stream, 1, although this is not guaranteed. The findAny() method is free to select any element on either serial or parallel streams.

<br/>

### **Combining Results with reduce()**

<br/>


```java
<U> U reduce(U identity, 
   BiFunction<U,? super T,U> accumulator, 
   BinaryOperator<U> combiner)
```

<br/>

**Example:**
```java
System.out.println(List.of('w', 'o', 'l', 'f')
   .parallelStream()                                   
   .reduce("", 
      (s1,c) -> s1 + c, 
      (s2,s3) -> s2 + s3));  // wolf
```
On parallel streams, the reduce() method works by applying the reduction to pairs of elements within the stream to create intermediate values and then combining those intermediate values to produce a final result. Put another way, in a serial stream, wolf is built one character at a time. In a parallel stream, the intermediate values wo and lf are created and then combined.

<br/>


**Example**:  Using a problematic accumulator. 

```java
System.out.println(List.of(1,2,3,4,5,6)
    .parallelStream()
    .reduce(0, (a,b) -> (a - b)));  // PROBLEMATIC ACCUMULATOR
```
It may output ‐21, 3, or some other value.

We can omit a combiner parameter in these examples, as the accumulator can be used when the intermediate data types are the same.

<br/>

```java
 System.out.println(List.of("w","o","l","f")
   .parallelStream()
   .reduce("X", String::concat));  // XwXoXlXf
```
On a serial stream, it prints Xwolf, but on a parallel stream the result is XwXoXlXf.

<br/>
<br/>

### **Combining Results with collect()**

<br/>


```java
 <R> R collect(Supplier<R> supplier,
   BiConsumer<R, ? super T> accumulator,
   BiConsumer<R, R> combiner)
```

<br/>

**Example :**
```java
 Stream<String> stream = Stream.of("w", "o", "l", "f").parallel();
SortedSet<String> set = stream.collect(ConcurrentSkipListSet::new,
   Set::add,
   Set::addAll);
System.out.println(set);  // [f, l, o, w]
```
Recall that elements in a ConcurrentSkipListSet are sorted according to their natural ordering. 

<br/>

### ***Performing a Parallel Reduction on a Collector***

*Requirements for Parallel Reduction with collect():*

- The stream is parallel.
- The parameter of the collect() operation has the Characteristics.CONCURRENT -characteristic.
- Either the stream is unordered or the collector has the characteristic **Characteristics.UNORDERED**.

For example, while Collectors.toSet() does have the UNORDERED characteristic, it does not have the CONCURRENT characteristic. Therefore, the following is not a parallel reduction even with a parallel stream:


```java
stream.collect(Collectors.toSet());  // Not a parallel reduction
```
toConcurrentMap() and groupingByConcurrent(), that are both UNORDERED and CONCURRENT. These methods produce Collector instances capable of performing parallel reductions efficiently. 

<br/>

### **AVOIDING STATEFUL OPERATIONS**

<br/>
<br/>

**Example 1:**
```java
 public List<Integer> addValues(IntStream source) {
   var data = Collections.synchronizedList(new ArrayList<Integer>());
   source.filter(s -> s % 2 == 0)
      .forEach(i -> { data.add(i); });  // STATEFUL: DON'T DO THIS!
   return data;
}
```

```java
var list = addValues(IntStream.range(1, 11));
System.out.println(list);   // [2, 4, 6, 8, 10]
```

<br/>

**Example 1: with parallel stream**
```java
var list = addValues(IntStream.range(1, 11).parallel());
System.out.println(list);  // [6, 8, 10, 2, 4]
```

<br/>

**Example 1: solve with collect()**
```java
 public static List<Integer> addValues(IntStream source) {
   return source.filter(s -> s % 2 == 0)
      .boxed()
      .collect(Collectors.toList());
}
```
**Print :**
```
[2, 4, 6, 8, 10]
 ```

<br/>

```java
 
```

<br/>
<br/>
<br/>
<br/>
📺
☝ 
🟡

🔴
## QUESTIONS
- explain the difference between extending the Thread class and implementing Runnable.