
# Unit 15 - Functional Programming


## Working with Built‐in Functional Interfaces 


📺 *Common functional interfaces*

|Functional interface|Return type|Method name|# of parameters|
|--- |--- |--- |--- |
|Supplier<T>|T|get()|0|
|Consumer<T>|void|accept(T)|1 (T)|
|BiConsumer<T, U>|void|accept(T,U)|2 (T, U)|
|Predicate<T>|boolean|test(T)|1 (T)|
|BiPredicate<T, U>|boolean|test(T,U)|2 (T, U)|
|Function<T, R>|R|apply(T)|1 (T)|
|BiFunction<T, U, R>|R|apply(T,U)|2 (T, U)|
|UnaryOperator<T>|T|apply(T)|1 (T)|
|BinaryOperator<T>|T|apply(T,T)|2 (T, T)|

   
 

<br/> 


 

🔴 **Note :** Many functional interfaces are defined in the *java.util.function* package.

<br/> <br/> 

###  **SUPPLIER**   



```java
@FunctionalInterface
public interface Supplier<T> {
   T get();
}
```   

<br/> 
 

**Example 1:** 
```java
Supplier<LocalDate> s1 = LocalDate::now;
Supplier<LocalDate> s2 = () -> LocalDate.now();
 
LocalDate d1 = s1.get();
LocalDate d2 = s2.get();
 
System.out.println(d1);     // 2021–04–04
System.out.println(d2);     // 2021–04–04
```  

<br/> 
 

**Example 2:** 
```java
Supplier<ArrayList<String>> s3 = ArrayList<String>::new;
ArrayList<String> a1 = s3.get();
a1.add("Hello");

System.out.println(a1);     // [Hello]
```   

<br/> 



### **CONSUMER AND BICONSUMER**  



```java
@FunctionalInterface
public interface Consumer<T> {
   void accept(T t);
}
```   
```java
@FunctionalInterface
public interface BiConsumer<T, U> {
   void accept(T t, U u);
}
```  


<br/> 
 

**Example 1:** 
```java
Consumer<String> c1 = System.out::println;
Consumer<String> c2 = x -> System.out.println(x);
 
c1.accept("Annie");     // Annie
c2.accept("Annie");     // Annie
```     

<br/> 
 

**Example 2:** 

```java
var map = new HashMap<String, String>();
BiConsumer<String, String> b1 = map::put;
BiConsumer<String, String> b2 = (k, v) -> map.put(k, v);
 
b1.accept("chicken", "Cluck");
b2.accept("chick", "Tweep");
 
System.out.println(map);        // {chicken=Cluck, chick=Tweep},
```   


<br/>    


### **PREDICATE AND BIPREDICATE**  



```java
@FunctionalInterface
public interface Predicate<T> {
   boolean test(T t);
}
```  
```java
@FunctionalInterface
public interface BiPredicate<T, U> {
   boolean test(T t, U u);
}
```   


<br/> 
 

**Example 1:** 
```java
Predicate<String> p1 = String::isEmpty;
Predicate<String> p2 = x -> x.isEmpty();
 
System.out.println(p1.test(""));  // true
System.out.println(p2.test(""));  // true
```   


<br/> 
 

**Example 2:** 
```java
BiPredicate<String, String> b1 = String::startsWith;
BiPredicate<String, String> b2 = (string, prefix) -> string.startsWith(prefix);
 
System.out.println(b1.test("chicken", "chick"));     // true
System.out.println(b2.test("chicken", "chick"));     // true

BiPredicate<String, String> b3 = String::contains;
System.out.println(b3.test("Hello World", "ell"));   // true
```  

<br/>     
  

### **FUNCTION AND BIFUNCTION**

```java
@FunctionalInterface
public interface Function<T, R> {
   R apply(T t);
}
```   
```java
@FunctionalInterface
public interface BiFunction<T, U, R> {
   R apply(T t, U u);
}
```   

<br/> 
 

**Example 1:** 
```java
Function<String, Integer> f1 = String::length;
Function<String, Integer> f2 = x -> x.length();
 
System.out.println(f1.apply("cluck"));      // 5
System.out.println(f2.apply("cluck"));      // 5
```  

<br/> 
 

**Example 2:** 
```java
BiFunction<String, String, String> b1 = String::concat;
BiFunction<String, String, String> b2 = (string, toAdd) -> string.concat(toAdd);
 
System.out.println(b1.apply("baby ", "chick")); // baby chick
System.out.println(b2.apply("baby ", "chick")); // baby chick
```   


### **UNARYOPERATOR AND BINARYOPERATOR**

```java
@FunctionalInterface
public interface UnaryOperator<T> extends Function<T, T> { }
```      
```java
@FunctionalInterface
public interface BinaryOperator<T> extends BiFunction<T, T, T> { 
}
```  

*UnaryOperator* and *BinaryOperator* are a special case of a *Function*. They require all type parameters to be the same type.   

A *UnaryOperator* transforms its value into one of the same type.  
For example, incrementing by one is a unary operation.  
*UnaryOperator* extends *Function*.

A *BinaryOperator* merges two values into one of the same type. Adding two numbers is a binary operation.   
*BinaryOperator* extends *BiFunction*.  


This means that method signatures look like this:

```java
T apply(T t);         // UnaryOperator
 
T apply(T t1, T t2);  // BinaryOperator
```   


<br/> 
 

**Example 1:** 

```java
UnaryOperator<String> u1 = String::toUpperCase;
UnaryOperator<String> u2 = x -> x.toUpperCase();
 
System.out.println(u1.apply("chirp"));  // CHIRP
System.out.println(u2.apply("chirp"));  // CHIRP  
```   

<br/> 
 

**Example 2:** 
```java
UnaryOperator<Double> areaOfCirle = (r) -> 3.14 * (r * r);
System.out.println(areaOfCirle.apply(10.0));        // 314.0
```   


<br/> 
 

**Example 3:** 
```java
BinaryOperator<String> b1 = String::concat;
BinaryOperator<String> b2 = (string, toAdd) -> string.concat(toAdd);
 
System.out.println(b1.apply("baby ", "chick")); // baby chick
System.out.println(b2.apply("baby ", "chick")); // baby chick
```  

<br/>     
  


### **CONVENIENCE METHODS ON FUNCTIONAL INTERFACES**

|Interface instance|Method return type|Method name|Method parameters|
|--- |--- |--- |--- |
|Consumer|Consumer|andThen()|Consumer|
|Function|Function|andThen()|Function|
|Function|Function|compose()|Function|
|Predicate|Predicate|and()|Predicate|
|Predicate|Predicate|negate()|—|
|Predicate|Predicate|or()|Predicate|


<br/> 
 

**Example 1 :** Without use convenience method 

```java
Predicate<String> brownEggs = s -> s.contains("egg") && s.contains("brown");
Predicate<String> otherEggs = s -> s.contains("egg") && ! s.contains("brown");
```   


<br/> 
 

**Example 1.2 :** Apply convenience method :
```java
Predicate<String> brownEggs = egg.and(brown);
Predicate<String> otherEggs = egg.and(brown.negate());
```   


<br/> 
 

**Example 2 :**
```java
Consumer<String> c1 = x -> System.out.print("1: " + x);
Consumer<String> c2 = x -> System.out.print(",2: " + x);
 
Consumer<String> combined = c1.andThen(c2);
combined.accept("Annie");                    // 1: Annie,2: Annie
```  


<br/> 
 

**Example 3 :**

```java
Function<Integer, Integer> before = x -> x + 1;
Function<Integer, Integer> after = x -> x * 2;
 
Function<Integer, Integer> combined = after.compose(before);
System.out.println(combined.apply(3));   // 8
```   

<br/> 
<br/> 



🟡 **Example 4 : Mixed**

```java
Function<String, String> f1 = x ->x + "_f1";
Function<String, String> f2 = x ->x + "_f2";
Function<String, String> f3 = x ->x + "_f3";
Function<String, String> f4 = x ->x + "_f4";
Function<String, String> f5 = x ->x + "_f5";		 

Function<String, String> f6 = f1.andThen(f2).compose(f3).andThen(f4).compose(f5);	
Function<String, String> f7 = f1.andThen(f2).compose(f3).compose(f4).andThen(f5);	

System.out.println(f6.apply("Start"));   
System.out.println(f7.apply("Start"));   
``` 
**Print :**  
``` 
Start_f5_f3_f1_f2_f4
Start_f4_f3_f1_f2_f5
``` 

<br/> 

<br/> 
    
  

## Returning an Optional  

An Optional is created using a factory. You can either request an empty Optional or pass a value for the Optional to wrap.


```java
public static Optional<Double> average(int... scores) {
    if (scores.length == 0)
        return Optional.empty();
    int sum = 0;
    for (int score : scores)
        sum += score;
    return Optional.of((double) sum / scores.length);
}
```   


<br/> 
 

**Example 1 :**

```java
System.out.println(average(90, 100)); // Optional[95.0]
System.out.println(average());        // Optional.empty
```  

<br/>

**Example 2 :**
```java
Optional<Double> opt = average();
System.out.println(opt.get()); // NoSuchElementException
```   
**Error:**
```
java.util.NoSuchElementException: No value present
```   

<br/>

When creating an Optional, it is common to want to use empty() when the value is null. You can do this with an if statement or ternary operator  
```java
Optional o = (value == null) ? Optional.empty() : Optional.of(value);
```  
Java provides a factory method to do the same thing.  
```java
Optional o = Optional.ofNullable(value);
```   
 
<br/> 
   

**Optional instance methods**

|Method|When Optional is empty|When Optional contains a value|
|--- |--- |--- |
|get()|Throws an exception|Returns value|
|ifPresent(Consumer c)|Does nothing|Calls Consumer with value|
|isPresent()|Returns false|Returns true|
|orElse(T other)|Returns other parameter|Returns value|
|orElseGet(Supplier s)|Returns result of calling Supplier|Returns value|
|orElseThrow()|Throws NoSuchElementException|Returns value|
|orElseThrow(Supplier s)|Throws exception created by calling Supplier|Returns value|   

 
<br/> 
    





**Example 3 :**
```java
Optional<Double> opt = average(90, 100);
opt.ifPresent(System.out::println);
```   

<br/> 
    

<br/> 
    

### **DEALING WITH AN EMPTY OPTIONAL**
   

<br/> 
 

**Example 4 :**

```java
Optional<Double> opt = average();
System.out.println(opt.orElse(Double.NaN));
System.out.println(opt.orElseGet(() -> Math.random()));
```  
**Print :**
```
NaN
0.49775932295380165
```   
<br/> 
 

**Example 5 :** Alternatively, we can have the code throw an exception if the Optional is empty.
```java
Optional<Double> opt = average();
System.out.println(opt.orElseThrow());
```   
**Print :**
```
Exception in thread "main" java.util.NoSuchElementException: 
   No value present
   at java.base/java.util.Optional.orElseThrow(Optional.java:382)
```   
Alternatively, we can have the code throw a custom exception if the Optional is empty.  

```java
Optional<Double> opt = average();
 System.out.println(opt.orElseThrow(
    () -> new IllegalStateException()));
```   
**Print :**
```
Exception in thread "main" java.lang.IllegalStateException
   at optionals.Methods.lambda$orElse$1(Methods.java:30)
   at java.base/java.util.Optional.orElseThrow(Optional.java:408)
```  

<br/> 
 

**Example 6 :** 
```java
System.out.println(opt.orElseGet(
   () -> new IllegalStateException())); // DOES NOT COMPILE
```  
The opt variable is an **Optional\<Double>**.

<br/> 
 

**Example 7 :** 
```java
Optional<Double> opt = average(90, 100);
System.out.println(opt.orElse(Double.NaN));
System.out.println(opt.orElseGet(() -> Math.random()));
System.out.println(opt.orElseThrow());
```   
*It prints out 95.0 three times.*  


<br/> 



### **IS OPTIONAL THE SAME AS NULL?**  

There wasn't a clear way to express that null might be a special value. By contrast, returning an Optional is a clear statement in the API that there might not be a value in there.  

Another advantage of Optional is that you can use a functional programming style with ifPresent() and the other methods rather than needing an if statement. 


<br/> 
  
  
<br/> 
   



## **Using Streams**  

A stream in Java is a sequence of data.  

### **UNDERSTANDING THE PIPELINE FLOW**  




Steps of Streams :

- **Source:** Where the stream comes from.
- **Intermediate operations:** Transforms the stream into another one. Since streams use *lazy evaluation*, the intermediate operations do not run until the terminal operation runs. 
- **Terminal operation:** Actually produces a result. The stream is no longer valid after a terminal operation completes.


🔴 **Note:** Once a terminal operation has been run, the stream cannot be used again.


<br/> 
  

📺 Intermediate vs. terminal operations

|Scenario|Intermediate operation|Terminal operation|
|--- |--- |--- |
|Required part of a useful pipeline?|No|Yes|
|Can exist multiple times in a pipeline?|Yes|No|
|Return type is a stream type?|Yes|No|
|Executed upon method call?|No|Yes|
|Stream valid after call?|Yes|No|

   
<br/> 
   

### **CREATING STREAM SOURCES**

Represented by the Stream\<T> interface, defined in the *java.util.stream package*.  

#### **Creating Finite Streams**

```java
Stream<String> empty = Stream.empty();          // count = 0
Stream<Integer> singleElement = Stream.of(1);   // count = 1
Stream<Integer> fromArray = Stream.of(1, 2, 3); // count = 3
```   

Converting a Collection to a stream.
```java
var list = List.of("a", "b", "c");
Stream<String> fromList = list.stream();
```  

<br/> 
  

#### **Creating Infinite Streams**

```java
Stream<Double> randoms = Stream.generate(Math::random);
Stream<Integer> oddNumbers = Stream.iterate(1, n -> n + 2);
```   

<br/> 
 

**Example 1 :** 
What if you wanted just odd numbers less than 100?
```java
 Stream<Integer> oddNumberUnder100 = Stream.iterate(
    1,                // seed
    n -> n < 100,     // Predicate to specify when done
    n -> n + 2);      // UnaryOperator to get next value
```   

#### **Reviewing Stream Creation Methods**  

|Method|Finite or infinite?|Notes|
|--- |--- |--- |
|Stream.empty()|Finite|Creates Stream with zero elements|
|Stream.of(varargs)|Finite|Creates Stream with elements listed|
|coll.stream()|Finite|Creates Stream from a Collection|
|coll.parallelStream()|Finite|Creates Stream from a Collection where the stream can run in parallel|
|Stream.generate(supplier)|Infinite|Creates Stream by calling the Supplier for each element upon request|
|Stream.iterate(seed,  unaryOperator)|Infinite|Creates Stream by using the seed for the first element and then calling the UnaryOperator for each subsequent element upon request|
|Stream.iterate(seed,  predicate, unaryOperator)|Finite or infinite|Creates Stream by using the seed for the first element and then calling the UnaryOperator for each subsequent element upon request. Stops if the Predicate returns false|



   
<br/> 
   


   
<br/> 
   

### **USING COMMON TERMINAL OPERATIONS**

|Method|What happens for infinite streams|Return value|Reduction|
|--- |--- |--- |--- |
|count()|Does not terminate|long|Yes|
|min()  max()|Does not terminate|Optional<T>|Yes|
|findAny()  findFirst()|Terminates|Optional<T>|No|
|allMatch()  anyMatch()  noneMatch()|Sometimes terminates|boolean|No|
|forEach()|Does not terminate|void|No|
|reduce()|Does not terminate|Varies|Yes|
|collect()|Does not terminate|Varies|Yes|
 
<br/>  

#### **count()**

```java
long count()
```  

```java
Stream<String> s = Stream.of("monkey", "gorilla", "bonobo");
System.out.println(s.count());   // 3
```  

<br/> 
 

#### **min() and max()**

```java
Optional<T> min(Comparator<? super T> comparator)
Optional<T> max(Comparator<? super T> comparator)
```   

```java
Stream<String> s = Stream.of("monkey", "ape", "bonobo");
Optional<String> min = s.min((s1, s2) -> s1.length()-s2.length());
min.ifPresent(System.out::println); // ape
```

<br/> 
 

#### **findAny() and findFirst()**

The *findAny()* and *findFirst()* methods return an element of the stream unless the stream is empty. If the stream is empty, they return an empty *Optional*.

```java
Optional<T> findAny()
Optional<T> findFirst()
```   
```java
Stream<String> s = Stream.of("monkey", "gorilla", "bonobo");
Stream<String> infinite = Stream.generate(() -> "chimp");
 
s.findAny().ifPresent(System.out::println);        // monkey (usually)
infinite.findAny().ifPresent(System.out::println); // chimp
```  

<br/> 
 
#### **allMatch(), anyMatch(), and noneMatch()**

These methods search a stream and return information about how the stream pertains to the predicate.
These may or may not terminate for infinite streams. It depends on the data.

```java
boolean anyMatch(Predicate <? super T> predicate)
boolean allMatch(Predicate <? super T> predicate)
boolean noneMatch(Predicate <? super T> predicate)
```   
```java
var list = List.of("monkey", "2", "chimp");
Stream<String> infinite = Stream.generate(() -> "chimp");
Predicate<String> pred = x -> Character.isLetter(x.charAt(0));
 
System.out.println(list.stream().anyMatch(pred));  // true
System.out.println(list.stream().allMatch(pred));  // false
System.out.println(list.stream().noneMatch(pred)); // false
System.out.println(infinite.anyMatch(pred));       // true
```  
*This shows that we can reuse the same predicate.*     

<br/> 
 


🔴 **Note:** allMatch(), anyMatch(), and noneMatch() return a *boolean*. By contrast, the find methods return an *Optional* because they return an element of the stream.

<br/> 
 


#### **forEach()**

forEach() on an infinite stream does not terminate. Since there is no return value, it is not a reduction.

```java
void forEach(Consumer<? super T> action)
```   
```java
Stream<String> s = Stream.of("Monkey", "Gorilla", "Bonobo");
s.forEach(System.out::print); // MonkeyGorillaBonobo
```  

<br/> 
 

You can't use a traditional for loop on a stream.
```java
Stream<Integer> s = Stream.of(1);
for (Integer i  : s) {} // DOES NOT COMPILE
```  
🔴 **Note:**  You can call *forEach()* directly on a *Collection* or on a *Stream*. 

<br/> 
  

#### **reduce()**
The *reduce()* method combines a stream into a single object. It is a reduction, which means it processes all elements.   

The **accumulator** combines the current result with the current value in the stream.

```java
T reduce(T identity, BinaryOperator<T> accumulator)
 
Optional<T> reduce(BinaryOperator<T> accumulator)
 
<U> U reduce(U identity, 
   BiFunction<U,? super T,U> accumulator, 
   BinaryOperator<U> combiner)
```   
```java
var array = new String[] { "w", "o", "l", "f" };
var result = "";
for (var s: array) result = result + s;
System.out.println(result); // wolf
```  

```java
Stream<String> stream = Stream.of("w", "o", "l", "f");
String word = stream.reduce("xx", (s, c) -> s + c);
System.out.println(word); // xxwolf
```

```java
Stream<String> stream = Stream.of("w", "o", "l", "f");
String word = stream.reduce("yy", String::concat);
System.out.println(word); // yywolf
```

```java
Stream<Integer> stream = Stream.of(3, 5, 6);
System.out.println(stream.reduce(1, (a, b) -> a*b));  // 90
```

🔴 **Note:** There are three choices for what is in the *Optional* :

- If the stream is empty, an empty *Optional* is returned.
- If the stream has one element, it is returned.
- If the stream has multiple elements, the accumulator is applied to combine them.

```java
BinaryOperator<Integer> op = (a, b) -> a * b;
Stream<Integer> empty = Stream.empty();
Stream<Integer> oneElement = Stream.of(3);
Stream<Integer> threeElements = Stream.of(3, 5, 6);
 
empty.reduce(op).ifPresent(System.out::println);         // no output
oneElement.reduce(op).ifPresent(System.out::println);    // 3
threeElements.reduce(op).ifPresent(System.out::println); // 90
```

<br/> 


**Example :** ?????????????????????
```java
Stream<String> stream = Stream.of("w", "o", "l", "f!");
int length = stream.reduce(0, (i, s) -> i+s.length(), (a, b) -> a+b);
System.out.println(length); // 5
```

<br/> 
 

#### **collect()**

The collect() method is a special type of reduction called a **mutable reduction**.  
It is more efficient than a regular reduction because we use the same mutable object while accumulating. Common mutable objects include StringBuilder and ArrayList. 

```java
<R> R collect(Supplier<R> supplier, 
   BiConsumer<R, ? super T> accumulator, 
   BiConsumer<R, R> combiner)
 
<R,A> R collect(Collector<? super T, A,R> collector)
```   
```java
Stream<String> stream = Stream.of("w", "o", "l", "f");
 
StringBuilder word = stream.collect(
   StringBuilder::new,
   StringBuilder::append, 
   StringBuilder::append)
 
System.out.println(word); // wolf
```  

<br/> 
 


 **Example 2:** 
 
  
```java
Stream<String> stream = Stream.of("w", "o", "l", "f");
 
TreeSet<String> set = stream.collect(
   TreeSet::new, 
   TreeSet::add,
   TreeSet::addAll);
 
System.out.println(set); // [f, l, o, w]
```   
<br/> 
 

 **Example 3:** 
```java
Stream<String> stream = Stream.of("w", "o", "l", "f");
TreeSet<String> set = 
   stream.collect(Collectors.toCollection(TreeSet::new));
System.out.println(set); // [f, l, o, w]
```   

**Example 4:**
```java
Stream<String> stream = Stream.of("w", "o", "l", "f");
Set<String> set = stream.collect(Collectors.toSet());
System.out.println(set); // [f, w, l, o]
```  
*Set() makes no guarantees as to which implementation of Set you'll get*   


   
<br/> 

   
<br/> 


### **USING COMMON INTERMEDIATE OPERATIONS**

#### **filter()**
```java
Stream<T> filter(Predicate<? super T> predicate)
```   
```java
Stream<String> s = Stream.of("monkey", "gorilla", "bonobo");
s.filter(x -> x.startsWith("m"))
   .forEach(System.out::print); // monkey
```   

<br/> 
 


#### **distinct()**
```java
Stream<T> distinct()
```  
```java
Stream<String> s = Stream.of("duck", "duck", "duck", "goose");
s.distinct()
   .forEach(System.out::print); // duckgoose
```   

<br/> 
 


#### **limit() and skip()**  

The limit() and skip() methods can make a Stream smaller, or they could make a finite stream out of an infinite stream.
```java
Stream<T> limit(long maxSize)
Stream<T> skip(long n)
```  
```java
Stream<Integer> s = Stream.iterate(1, n -> n + 1);
s.skip(5)
   .limit(2)
   .forEach(System.out::print); // 67
```  
*The skip() operation returns an infinite stream starting with the numbers counting from 6, since it skips the first five elements*

<br/> 
 


#### **map()**
```java
<R> Stream<R> map(Function<? super T, ? extends R> mapper)
```  
```java
Stream<String> s = Stream.of("monkey", "gorilla", "bonobo");
s.map(String::length)
   .forEach(System.out::print); // 676
```  

<br/> 
 


#### **flatMap()**
This is helpful when you want to remove empty elements from a stream or you want to combine a stream of lists
```java
<R> Stream<R> flatMap(
   Function<? super T, ? extends Stream<? extends R>> mapper)
```  
```java
List<String> zero = List.of();
var one = List.of("Bonobo");
var two = List.of("Mama Gorilla", "Baby Gorilla");
Stream<List<String>> animals = Stream.of(zero, one, two);
 
animals.flatMap(m -> m.stream())
   .forEach(System.out::println);
```  
**Print :**
```  
Bonobo
Mama Gorilla
Baby Gorilla
```  

<br/> 
 


#### **sorted()**
```java
Stream<T> sorted()
Stream<T> sorted(Comparator<? super T> comparator)
```  

<br/>

**Example 1:**
```java
Stream<String> s = Stream.of("brown-", "bear-");
s.sorted()
   .forEach(System.out::print); // bear-brown-
```

<br/> 


**Example 2:**
```java
Stream<String> s = Stream.of("brown bear-", "grizzly-");
s.sorted(Comparator.reverseOrder())
   .forEach(System.out::print); // grizzly-brown bear-
```

<br/> 
 
**Example 3:**
```java
s.sorted(Comparator::reverseOrder); // DOES NOT COMPILE
```
Take a look at the method signatures again. Comparator is a functional interface. This means that we can use method references or lambdas to implement it. The Comparator interface implements one method that takes two String parameters and returns an int. However, Comparator::reverseOrder doesn't do that. It is a reference to a function that takes zero parameters and returns a Comparator
<br/> 
 

#### **peek()**
 It is useful for debugging because it allows us to perform a stream operation without actually changing the stream.
```java
Stream<T> peek(Consumer<? super T> action)
```  
```java
var stream = Stream.of("black bear", "brown bear", "grizzly");
long count = stream.filter(s -> s.startsWith("g"))
   .peek(System.out::println).count();              // grizzly
System.out.println(count);                          // 1
```

<br/> 
 

##### **DANGER: CHANGING STATE WITH PEEK()**

 
```java
var numbers = new ArrayList<>();
var letters = new ArrayList<>();
numbers.add(1);
letters.add('a');

Stream<List<?>> stream = Stream.of(numbers, letters);
stream.map(List::size).forEach(System.out::print); // 11
```  
```java
Stream<List<?>> bad = Stream.of(numbers, letters);
bad.peek(x -> x.remove(0))
    .map(List::size)
    .forEach(System.out::print); // 00
```

<br/> <br/> 
 
## **Working with Primitive Streams**

<br/> 

- **IntStream:** Used for the primitive types int, short, byte, and char
- **LongStream:** Used for the primitive type long
- **DoubleStream:** Used for the primitive types double and float

<br/> 

<table border="1">
<thead>
<tr>
<td><b>Method</b></td>
<td><b>Primitive stream</b></td>
<td><b>Description</b></td> </tr> </thead>
<tbody>
<tr>
<td >
<code>OptionalDouble average()</code> </td>
<td >
<code>IntStream</code> <br> <code>LongStream</code> <br> <code>DoubleStream</code> </td>
<td >The arithmetic mean of the elements</td> </tr>
<tr>
<td >
<code>Stream&lt;T&gt; boxed()</code> </td>
<td >
<code>IntStream</code> <br> <code>LongStream</code> <br> <code>DoubleStream</code> </td>
<td >A <code>Stream&lt;T&gt;</code> where <code>T</code> is the wrapper class associated with the primitive value</td> </tr>
<tr>
<td >
<code>OptionalInt max()</code> </td>
<td >
<code>IntStream</code> </td>
<td  rowspan="3">The maximum element of the stream</td> </tr>
<tr>
<td >
<code>OptionalLong max()</code> </td>
<td >
<code>LongStream</code> </td> </tr>
<tr>
<td >
<code>OptionalDouble max()</code> </td>
<td >
<code>DoubleStream</code> </td> </tr>
<tr>
<td >
<code>OptionalInt min()</code> </td>
<td >
<code>IntStream</code> </td>
<td  rowspan="3">The minimum element of the stream </td> </tr>
<tr>
<td >
<code>OptionalLong min()</code> </td>
<td >
<code>LongStream</code> </td> </tr>
<tr>
<td >
<code>OptionalDouble min()</code> </td>
<td >
<code>DoubleStream</code> </td> </tr>
<tr>
<td >
<code>IntStream range(int a, int b)</code> </td>
<td >
<code>IntStream</code> </td>
<td  rowspan="2">Returns a primitive stream from <code>a</code> (inclusive) to <code>b</code> (exclusive) </td> </tr>
<tr>
<td >
<code>LongStream range(long a, long b)</code> </td>
<td >
<code>LongStream</code> </td> </tr>
<tr>
<td >
<code>IntStream rangeClosed(int a, int b)</code> </td>
<td >
<code>IntStream</code> </td>
<td  rowspan="2">Returns a primitive stream from <code>a</code> (inclusive) to <code>b</code> (inclusive) </td> </tr>
<tr>
<td >
<code>LongStream rangeClosed(long a, long b)</code> </td>
<td >
<code>LongStream</code> </td> </tr>
<tr>
<td >
<code>int sum()</code> </td>
<td >
<code>IntStream</code> </td>
<td  rowspan="3">Returns the sum of the elements in the stream </td> </tr>
<tr>
<td >
<code>long sum()</code> </td>
<td >
<code>LongStream</code> </td> </tr>
<tr>
<td >
<code>double sum()</code> </td>
<td >
<code>DoubleStream</code> </td> </tr>
<tr>
<td >
<code>IntSummaryStatistics summaryStatistics()</code> </td>
<td >
<code>IntStream</code> </td>
<td  rowspan="3">Returns an object containing numerous stream statistics such as the average, min, max, etc. </td> </tr>
<tr>
<td >
<code>LongSummaryStatistics summaryStatistics()</code> </td>
<td >
<code>LongStream</code> </td> </tr>
<tr>
<td >
<code>DoubleSummaryStatistics summaryStatistics()</code> </td>
<td >
<code>DoubleStream</code> <span epub:type="pagebreak" id="Page_710" role="doc-pagebreak"></span></td>  </tr> </tbody> </table>




<br/>
<br/>
<br/>

### **MAPPING STREAMS**

<br/>

📺 Mapping methods between types of streams

|Source stream class|To create Stream|To create DoubleStream|To create IntStream|To create LongStream|
|--- |--- |--- |--- |--- |
|Stream<T>|map()|mapToDouble()|mapToInt()|mapToLong()|
|DoubleStream|mapToObj()|map()|mapToInt()|mapToLong()|
|IntStream|mapToObj()|mapToDouble()|map()|mapToLong()|
|LongStream|mapToObj()|mapToDouble()|mapToInt()|map()|

<br/>

**Example :**
```java
Stream<String> objStream = Stream.of("penguin", "fish");
IntStream intStream = objStream.mapToInt(s -> s.length());
``` 

<br/> 

📺 Function parameters when mapping between types of streams

|Source stream class|To create Stream|To create DoubleStream|To create IntStream|To create LongStream|
|--- |--- |--- |--- |--- |
|Stream\<T>|Function<T,R>|ToDoubleFunction\<T>|ToIntFunction\<T>|ToLongFunction\<T>|
|DoubleStream|Double Function\<R>|DoubleUnary Operator|DoubleToInt Function|DoubleToLong Function|
|IntStream|IntFunction\<R>|IntToDouble Function|IntUnary Operator|IntToLong Function|
|LongStream|Long Function\<R>|LongToDouble Function|LongToInt Function|LongUnary Operator|


<br/> 
<br/> 

### **Using Optional with Primitive Streams**


**Example :**
```java
var stream = IntStream.rangeClosed(1,10);
OptionalDouble optional = stream.average();

optional.ifPresent(System.out::println);                  // 5.5
System.out.println(optional.getAsDouble());               // 5.5
System.out.println(optional.orElseGet(() -> Double.NaN)); // 5.5
``` 

<br/> 

📺 Optional types for primitives

||OptionalDouble|OptionalInt|OptionalLong|
|--- |--- |--- |--- |
|Getting as a primitive|getAsDouble()|getAsInt()|getAsLong()|
|orElseGet() parameter type|DoubleSupplier|IntSupplier|LongSupplier|
|Return type of max() and min()|OptionalDouble|OptionalInt|OptionalLong|
|Return type of sum()|double|int|long|
|Return type of average()|OptionalDouble|OptionalDouble|OptionalDouble|

<br/> 
<br/> 
<br/> 

### **LEARNING THE FUNCTIONAL INTERFACES FOR PRIMITIVES** 
<br/> 

#### **Functional Interfaces for boolean**

<br/> 

```java
boolean getAsBoolean()
``` 

<br/> 

**Example :**
```java
BooleanSupplier b1 = () -> true;
BooleanSupplier b2 = () -> Math.random()> .5;
System.out.println(b1.getAsBoolean());  // true
System.out.println(b2.getAsBoolean());  // false
``` 

<br/> 

#### **Functional Interfaces for double, int, and long**

<br/>

📺Common functional interfaces for primitives
<table border="1">
<thead>
<tr>
<td>Functional interfaces</td>
<td># parameters</td>
<td>Return type</td>
<td>Single abstract method</td> </tr> </thead>
<tbody>
<tr>
<td>
<code>DoubleSupplier</code> <br> <code>IntSupplier</code> <br> <code>LongSupplier</code> </td>
<td>0</td>
<td>
<code>double</code> <br> <code>int</code> <br> <code>long</code> </td>
<td>
<code>getAsDouble</code> <br> <code>getAsInt</code> <br> <code>getAsLong</code> </td> </tr>
<tr>
<td>
<code>DoubleConsumer</code> <br> <code>IntConsumer</code> <br> <code>LongConsumer</code> </td>
<td>1 (
<code>double</code>)<br> 1 (
<code>int</code>)<br> 1 (
<code>long</code>) </td>
<td>
<code>void</code> </td>
<td>
<code>accept</code> </td> </tr>
<tr>
<td>
<code>DoublePredicate</code> <br> <code>IntPredicate</code> <br> <code>LongPredicate</code> </td>
<td>1 (
<code>double</code>)<br> 1 (
<code>int</code>)<br> 1 (
<code>long</code>) </td>
<td>
<code>boolean</code> </td>
<td>
<code>test</code> </td> </tr>
<tr>
<td>
<code>DoubleFunction&lt;R&gt;</code> <br> <code>IntFunction&lt;R&gt;</code> <br> <code>LongFunction&lt;R&gt;</code> </td>
<td>1 (
<code>double</code>)<br> 1 (
<code>int</code>)<br> 1 (
<code>long</code>) </td>
<td>
<code>R</code> </td>
<td>
<code>apply</code> </td> </tr>
<tr>
<td>
<code>DoubleUnaryOperator</code> <br> <code>IntUnaryOperator</code> <br> <code>LongUnaryOperator</code> </td>
<td>1 (
<code>double</code>)<br> 1 (
<code>int</code>)<br> 1 (
<code>long</code>) </td>
<td>
<code>double</code> <br> <code>int</code> <br> <code>long</code> </td>
<td>
<code>applyAsDouble</code> <br> <code>applyAsInt</code> <br> <code>applyAsLong</code> </td> </tr>
<tr>
<td>
<code>DoubleBinaryOperator</code> <br> <code>IntBinaryOperator</code> <br> <code>LongBinaryOperator</code> </td>
<td>2 (
<code>double, double</code>)<br> 2 (
<code>int</code>, <code>int</code>)<br> 2 (
<code>long</code>, <code>long</code>) </td>
<td>
<code>double</code> <br> <code>int</code> <br> <code>long</code> </td>
<td>
<code>applyAsDouble</code> <br> <code>applyAsInt</code> <br> <code>applyAsLong</code> </td> </tr> </tbody> </table>

<br/> 
<br/> 

📺 Primitive‐specific functional interfaces
<table border="1">
<thead>
<tr>
<td>Functional interfaces</td>
<td># parameters</td>
<td>Return type</td>
<td>Single abstract method</td> </tr> </thead>
<tbody>
<tr>
<td>
<code>ToDoubleFunction&lt;T&gt;</code> <br> <code>ToIntFunction&lt;T&gt;</code> <br> <code>ToLongFunction&lt;T&gt;</code> </td>
<td>1 (
<code>T</code>)</td>
<td>
<code>double</code> <br> <code>int</code> <br> <code>long</code> </td>
<td>
<code>applyAsDouble</code> <br> <code>applyAsInt</code> <br> <code>applyAsLong</code> </td> </tr>
<tr>
<td>
<code>ToDoubleBiFunction&lt;T, U&gt;</code> <br> <code>ToIntBiFunction&lt;T, U&gt;</code> <br> <code>ToLongBiFunction&lt;T, U&gt;</code> </td>
<td>2 (
<code>T</code>, <code>U</code>)</td>
<td>
<code>double</code> <br> <code>int</code> <br> <code>long</code> </td>
<td>
<code>applyAsDouble</code> <br> <code>applyAsInt</code> <br> <code>applyAsLong</code> </td> </tr>
<tr>
<td>
<code>DoubleToIntFunction</code> <br> <code>DoubleToLongFunction</code> <br> <code>IntToDoubleFunction</code> <br> <code>IntToLongFunction</code> <br> <code>LongToDoubleFunction</code> <br> <code>LongToIntFunction</code> </td>
<td>1 (
<code>double</code>)<br> 1 (
<code>double</code>)<br> 1 (
<code>int</code>)<br> 1 (
<code>int</code>)<br> 1 (
<code>long</code>)<br> 1 (
<code>long</code>) </td>
<td>
<code>int</code> <br> <code>long</code> <br> <code>double</code> <br> <code>long</code> <br> <code>double</code> <br> <code>int</code> </td>
<td>
<code>applyAsInt</code> <br> <code>applyAsLong</code> <br> <code>applyAsDouble</code> <br> <code>applyAsLong</code> <br> <code>applyAsDouble</code> <br> <code>applyAsInt</code> </td> </tr>
<tr>
<td>
<code>ObjDoubleConsumer&lt;T&gt;</code> <br> <code>ObjIntConsumer&lt;T&gt;</code> <br> <code>ObjLongConsumer&lt;T&gt;</code> </td>
<td>2 <code>(T</code>, <code>double)</code><br> 2 <code>(T</code>, <code>int)</code><br> 2 <code>(T</code>, <code>long)</code> </td>
<td>
<code>void</code> </td>
<td>
<code>accept</code> </td> </tr> </tbody> </table>



<br/> 
<br/> 

## **Working with Advanced Stream Pipeline Concepts**


**Example :**
```java
var cats = new ArrayList<String>();
cats.add("Annie");
cats.add("Ripley");
var stream = cats.stream();
cats.add("KC");
System.out.println(stream.count()); // 3
``` 

<br/> 

### **COLLECTING RESULTS**

<br/> 


 📺 Examples of grouping/partitioning collectors
<table border="4">
<thead>
<tr>
<td>Collector</td>
<td>Description</td>
<td>Return value when passed to <code>collect</code></td> </tr> </thead>
<tbody>
<tr>
<td>
<code>averagingDouble(ToDoubleFunction f)</code> <br> <code>averagingInt(ToIntFunction f)</code> <br> <code>averagingLong(ToLongFunction f)</code> </td>
<td>Calculates the average for our three core primitive types</td>
<td>
<code>Double</code> </td> </tr>
<tr>
<td>
<code>counting()</code> </td>
<td>Counts the number of elements</td>
<td>
<code>Long</code> </td> </tr>
<tr>
<td>
<code>groupingBy(Function f)</code> <br> <code>groupingBy(Function f, Collector dc)</code> <br> <code>groupingBy(Function f, Supplier s, Collector dc)</code> </td>
<td>Creates a map grouping by the specified function with the optional map type supplier and optional downstream collector</td>
<td>
<code>Map&lt;K, List&lt;T&gt;&gt;</code> </td> </tr>
<tr>
<td>
<code>joining(CharSequence cs)</code> </td>
<td>Creates a single <code>String</code> using <code>cs</code> as a delimiter between elements if one is specified</td>
<td>
<code>String</code> </td> </tr>
<tr>
<td>
<code>maxBy(Comparator c)</code> <br> <code>minBy(Comparator c)</code> </td>
<td>Finds the largest/smallest elements</td>
<td>
<code>Optional&lt;T&gt;</code> </td> </tr>
<tr>
<td>
<code>mapping(Function f, Collector dc)</code> </td>
<td>Adds another level of collectors</td>
<td>
<code>Collector</code> </td> </tr>
<tr>
<td>
<code>partitioningBy(Predicate p)</code> <br> <code>partitioningBy(Predicate p, Collector dc)</code> </td>
<td>Creates a map grouping by the specified predicate with the optional further downstream collector</td>
<td>
<code>Map&lt;Boolean, List&lt;T&gt;&gt;</code> </td> </tr>
<tr>
<td>
<code>summarizingDouble(ToDoubleFunction f)</code> <br> <code>summarizingInt(ToIntFunction f)</code> <br> <code>summarizingLong(ToLongFunction f)</code> </td>
<td>Calculates average, min, max, and so on</td>
<td>
<code>DoubleSummaryStatistics IntSummaryStatistics LongSummaryStatistics</code> </td> </tr>
<tr>
<td>
<code>summingDouble(ToDoubleFunction f)</code> <br> <code>summingInt(ToIntFunction f)</code> <br> <code>summingLong(ToLongFunction f)</code> </td>
<td>Calculates the sum for our three core primitive types</td>
<td>
<code>Double</code> <br> <code>Integer</code> <br> <code>Long</code> </td> </tr>
<tr>
<td>
<code>toList()</code> <br> <code>toSet()</code> </td>
<td>Creates an arbitrary type of list or set</td>
<td>
<code>List</code> <br> <code>Set</code> </td> </tr>
<tr>
<td>
<code>toCollection(Supplier s)</code> </td>
<td>Creates a <code>Collection</code> of the specified type</td>
<td>
<code>Collection</code> </td> </tr>
<tr>
<td>
<code>toMap(Function k, Function v)</code> <br> <code>toMap(Function k, Function v, BinaryOperator m)</code> <br> <code>toMap(Function k, Function v, BinaryOperator m, Supplier s)</code> </td>
<td>Creates a map using functions to map the keys, values, an optional merge function, and an optional map type supplier</td>
<td>
<code>Map</code> <span epub:type="pagebreak" id="Page_722" role="doc-pagebreak"></span> <span epub:type="pagebreak" id="Page_723" role="doc-pagebreak"></span> </td> </tr> </tbody> </table>





<br/> 
<br/> 

### **Collecting Using Basic Collectors**


<br/> 

**Example 1:**
```java
var ohMy = Stream.of("lions", "tigers", "bears");
String result = ohMy.collect(Collectors.joining("_"));
System.out.println(result); // lions_tigers_bears
```

<br/> 

**Example 2:**
```java
var ohMy2 = Stream.of("lions", "tigers", "bears", "tamtam");
TreeSet<String> result = ohMy2
        .filter(s -> s.startsWith("t"))
        .collect(Collectors.toCollection(TreeSet::new));
System.out.println(result);     // [tamtam, tigers]
```

<br/> 


### **Collecting into Maps**

<br/> 

**Example 2:**
```java
var ohMy3 = Stream.of("lions", "tigers", "bears", "tamtam");
Map<String, Integer> map = ohMy3.collect(
        Collectors.toMap(s -> s, String::length));
System.out.println(map); // {lions=5, bears=5, tamtam=6, tigers=6}
```

 
<br/> 

**Example :** Collect and grouping by length
```java
var ohMy4 = Stream.of("lions", "tigers", "bears", "monkey","cat","dog","horse");
TreeMap<Integer, String> map2 = ohMy4.collect(Collectors.toMap(
        String::length,
        k -> k,
        (s1, s2) -> s1 + "__" + s2,
        TreeMap::new));
System.out.println(map2); 

// Prints :
// {3=cat__dog, 5=lions__bears__horse, 6=tigers__monkey}
```

 
<br/> 

### **Collecting Using Grouping, Partitioning, and Mapping**


<br/> 

**Example :**
```java
var myStream = Stream.of("lions", "tigers", "bears", "monkey","cat","dog","horse");
Map<Integer, List<String>> map3 = myStream.collect(
        Collectors.groupingBy(String::length));
System.out.println(map3);

// Prints :
// {3=[cat, dog], 5=[lions, bears, horse], 6=[tigers, monkey]}
```

<br/> 



**Example :** Suppose that we don't want a *List* as the value in the map and prefer a *Set* instead.

```java
Map<Integer, Set<String>> map4 = myStream.collect(
            Collectors.groupingBy(
                    String::length,
                    Collectors.toSet()));
System.out.println(map4);

// Prints :
// {3=[cat, dog], 5=[lions, bears, horse], 6=[tigers, monkey]}
```

<br/> 

**Example :** We can even change the type of Map
```java
TreeMap<Integer, Set<String>> map5 = myStream.collect(
    Collectors.groupingBy(
        String::length, 
        TreeMap::new, 
        Collectors.toSet()));

System.out.println(map5);

// Prints :
// {3=[cat, dog], 5=[lions, bears, horse], 6=[tigers, monkey]}
```
<br/> 

**Example :** Keep as List
```java
TreeMap<Integer, List<String>> map4 = myStream.collect(
                Collectors.groupingBy(
                        String::length,
                        TreeMap::new,
                        Collectors.toList()));
System.out.println(map4);

// Prints :
// {3=[cat, dog], 5=[lions, bears, horse], 6=[tigers, monkey]}
```


🔴 **Note :** The function you call in *groupingBy()* cannot return null. It does not allow null keys.


#### **Partitioning**

*Partitioning* is a special case of grouping. With partitioning, there are only two possible groups—true and false. *Partitioning* is like splitting a list into two parts.



 
<br/> 

**Example :**  
```java
var ohMy = Stream.of("lions", "tigers", "bears");
Map<Boolean, List<String>> map = ohMy.collect(
   Collectors.partitioningBy(s -> s.length() <= 5));
System.out.println(map);    

// Prints :
// {false=[tigers], true=[lions, bears]}
```

 
<br/> 
<br/> 

## SUMMARY

There are three optional types for primitives: OptionalDouble, OptionalInt, and OptionalLong. These have the methods getAsDouble(), getAsInt(), and getAsLong(), respectively.

<br/> 

**Intermediate operations :**
- filter() 
- map()
- flatMap() 
- sorted()

<br/> 


**Terminal operations :**
- allMatch() 
- count() 
- forEach()

<br/> 
<br/> 
<br/> 
<br/> 



