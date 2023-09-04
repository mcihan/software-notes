
# Unit-9 Advanced Class Design
## Abstract Classes


```java
class Bird {
    public String getName() {
        return null;
    }
    public void printName() {
        System.out.print(getName());
    }
}
public class Stork extends Bird {
    public String getName() {
        return "Stork!";
    }
    public static void main(String[] args) {
        new Stork().printName();
    }
}
```  


*This program prints **Stork!** at runtime. Notice that the getName() method is overridden in the subclass. Even though the implementation of printName() is defined in the Bird Creating Abstract Classes 365 class, the fact that getName() is overridden in the subclass means it is replaced everywhere, even in the parent class.*


🔴 **Note** : It is not possible to define an abstract method that has a body, or default implementation. You can still define a method with a body—you just can’t mark it as abstract . As long as you do not mark the method as final , the subclass has the option to override an inherited method.


### Constructor Classes

**Example 1:**

```java
abstract class Bear {
	abstract CharSequence chew();

	public Bear() {
		System.out.println(chew()); // Does this compile?
	}
}

public class Panda extends Bear {
	String chew() {
		return "yummy!";
	}

	public static void main(String[] args) {
		new Panda();
	}
}
```  

*The Bear constructor is only called when the abstract class is being initialized through a subclass; therefore, there is an implementation of   chew() at the time the constructor is called.*   
*The primary difference between a constructor in an abstract class and a nonabstract class is that a constructor in abstract class can be called only when it is being initialized by a nonabstract subclass. This makes sense, as abstract classes cannot be instantiated.*

<br/> 


**Example 2:**
```java

abstract class Bear {
	abstract CharSequence chew();

	public Bear() {
		System.out.println("non parameter constructer"); 
	}

	public Bear(int i) {
		System.out.println("int constructer");  
	}
	 
}

public class Panda extends Bear {

	public static void main(String[] args) {
		new Panda(1);   // DOES NOT COMPILE
	}
}
```  
*You can define multible constructor, but only default constructor work when you initialate subclass, other constructor never work. new Panda(1) does not compile because there isn't Panda constructor which have int parameter.*

**Example 3:**
```java

abstract class Bear {
	abstract CharSequence chew();

	public Bear() {
		System.out.println("non parameter constructer"); 
	}

	public Bear(int i) {
		System.out.println("int constructer");  
	}
	 
}

public class Panda extends Bear {

    Panda(int i){
		
	}

	public static void main(String[] args) {
		new Panda(1);   
	}
}
```  
*It complie success and print **"non parameter constructer"** !!  
-> You can define multible constructor, but only default constructor work when you initialate subclass, other constructor never work.*


### Declarations

* **Java does not permit a class or method to be marked both abstract and final .**

```java
public abstract final class Tortoise { // DOES NOT COMPILE
	public abstract final void walk(); // DOES NOT COMPILE
}
```  
* **it is not possible to declare a method abstract and private.**
```java
public abstract class Whale {
	private abstract void sing(); // DOES NOT COMPILE
}
```  
* **Static method cannot be overridden. If a static method cannot be overridden, then it follows that it also cannot be marked abstract since it can never be implemented.**
```java
abstract class Hippopotamus {
    abstract static void swim(); // DOES NOT COMPILE
}
```  

### Creating a Concrete Class



```java
public abstract class Animal {
	public abstract String getName();
}

public class Walrus extends Animal { // DOES NOT COMPILE
}
```  

*Since Walrus is the first concrete subclass, it must implement all inherited abstract methods.*

<br/> 


```java
abstract class Mammal {
	abstract void showHorn();
	abstract void eatLeaf();
}

abstract class Rhino extends Mammal {
	void showHorn() {
	}
}

public class BlackRhino extends Rhino {
	void eatLeaf() {
	}
}
```  
*Since the parent class, Rhino, provides an implementation of showHorn(), the method is inherited in the BlackRhino as a nonabstract method. For this reason, the BlackRhino class is permitted but not required to override the showHorn() method.*  
  
*Codes are correctly defined and compile*

<br/> 



  

### Abstract Class Definition Rules

1. Abstract classes cannot be instantiated.
2. All top-level types, including abstract classes, cannot be marked **protected** or **private**.
3. Abstract classes cannot be marked **final**.
4. Abstract classes may include zero or more abstract and nonabstract methods.
5. An abstract class that extends another abstract class inherits all of its abstract methods.
6. The first concrete class that extends an abstract class must provide an implementation
for all of the inherited abstract methods.
7. Abstract class constructors follow the same rules for initialization as regular constructors, except they can be called only as part of the initialization of a subclass.  
  
These rules for abstract methods apply regardless of whether the abstract method is
defined in an abstract class or interface.


### Abstract Method Definition Rules

1. Abstract methods can be defined only in abstract classes or interfaces.
2. Abstract methods cannot be declared private or final.
3. Abstract methods must not provide a method body/implementation in the abstract
class in which they are declared.
4. Implementing an abstract method in a subclass follows the same rules for overriding a
method, including covariant return types, exception declarations, etc.

<br/> 


  
  ## Interfaces

* Definition 
```java
public abstract interface WalksOnTwoLegs {}
public final interface WalksOnEightLegs {} // DOES NOT COMPILE
private interface WalksOnEightLegs {} // DOES NOT COMPILE
```  

* Like a class, an interface can extend another interface using the extends keyword. 

```java
interface Nocturnal {}
public interface HasBigEyes extends Nocturnal {}
```  

* Unlike a class, which can extend only one class, an interface can extend multiple interfaces.

```java
interface Nocturnal {}
interface CanFly {}

public interface HasBigEyes extends Nocturnal, CanFly {}
```  

### Inserting Implicit Modifiers

* Interfaces are assumed to be abstract.
* Interface variables are assumed to be public, static, and final.
* Interface methods without a body are assumed to be abstract and public.

```java
public interface Soar {
	int MAX_HEIGHT = 10;
	final static boolean UNDERWATER = true;
	void fly(int speed);
	abstract void takeoff();
	public abstract double dive();
}
```  
*We’ve marked in comment line the implicit modifiers that the compiler automatically inserts.*
 
```java
public /*abstract*/ interface Soar {
	/*public static final */ int MAX_HEIGHT = 10;
	/*public*/ final static boolean UNDERWATER = true;
	/*public abstract */ void fly(int speed);
	/*public*/ abstract void takeoff();
	/*public*/ abstract double dive();
}
```

### Conflicting Modifiers


```java
public interface Dance {
    private int count = 4; // DOES NOT COMPILE
    protected void step(); // DOES NOT COMPILE
}
```  

### Differences between Interfaces and Abstract Classes


```java
abstract class Husky {
    abstract void play();
}

interface Poodle {
    void play();
}
```  
- *Both of these method definitions are considered abstract. That said, the Husky class will not compile if the play() method is not marked abstract, whereas the method in the Poodle interface will compile with or without the abstract modifier.*

- *Even though neither has an access
modifier, they do not have the same access level. The play() method in Husky class is considered default (package-private), whereas the method in the Poodle interface is assumed to be public.*



```java
class Webby extends Husky {
    void play() {}
}
class Georgette implements Poodle {
    void play() {}
}
```  
*The Webby class compiles, but the Georgette class does not. The play() method in the Georgette class breaks the rules of method overriding. 
The play() method in interface is assummed to be **public**, but in Georgette is **default**(package-private) * 


**Correct implementation :**
```java
class Georgette implements Poodle {
    public void play() {}
}
```  


### Inheriting an Interface  

An interface can be inherited in one of three ways.
* An interface can extend another interface.
* A class can implement an interface.
* A class can extend another class whose ancestor implements an interface.  

When an interface is inherited, all of the abstract methods are inherited. 
If the type inheriting the interface is also abstract, such as an interface
or abstract class, it is not required to implement the interface methods.

```java
public interface HasTail {
	public int getTailLength();
}

public interface HasWhiskers {
	public int getNumberOfWhiskers();
}

public abstract class HarborSeal implements HasTail, HasWhiskers {
}

public class CommonSeal extends HarborSeal { // DOES NOT COMPILE
}
```  
*The HarborSeal class is not required to implement any of the abstract methods because it is marked abstract.*  
  
*CommonSeal calss does not compile because it is required to implement all inherited abstract methods*

### Mixing Class and Interface Keywords

```java
class1 extends class2
interface1 extends interface2, interface3, ...
class1 implements interface2, interface3, ...
```  


### Duplicate Interface Method Declarations
**Example 1:**
```java
public interface Herbivore {
	public void eatPlants();
}

public interface Omnivore {
	public void eatPlants();
}

public class Bear implements Herbivore, Omnivore {
	public void eatPlants() {
		System.out.println("Eating plants");
	}
}
``` 
*In this scenario, the signatures for the two interface methods eatPlants() are duplicates. As they have identical method declarations, they are also considered compatible. By compatibility, we mean that the compiler can resolve the differences between the two declarations without finding any conflicts.*

<br/> 


**Example 2:**
```java
interface Dances {
	String swingArms();
}

interface EatsFish {
	CharSequence swingArms();
}

public class Penguin implements Dances, EatsFish {
	public String swingArms() {
		return "swing!";
	}
}
```  

*It compiles successfuly. The EatsFish version of swingArms() is also
overridden as String and CharSequence are covariant return types.* 

<br/> 

**Example 3: If return types are not covariant, it doesn't compile !**

```java
interface Dances {
	int countMoves();
}

interface EatsFish {
	boolean countMoves();
}

public class Penguin implements Dances, EatsFish { // DOES NOT COMPILE
```  

## Polymorphism and Interfaces

```java
interface Canine {
}

class Dog implements Canine {
}

class Wolf implements Canine {
}

public class BadCasts {
	public static void main(String[] args) {
		Canine canine = new Wolf();
		Canine badDog = (Dog) canine;
	}
}
```  
*It allows the invalid cast and code compiles but throws a ClassCastException
at runtime.*

### Interfaces and the instanceof Operator

* The compiler will report an error if you attempt to use the instanceof operator with two unrelated classes



## Interface Definition Rules
1. Interfaces cannot be instantiated.
2. All top-level types, including interfaces, cannot be marked protected or private.
3. Interfaces are assumed to be abstract and cannot be marked final.
4. Interfaces may include zero or more abstract methods.
5. An interface can extend any number of interfaces.
6. An interface reference may be cast to any reference that inherits the interface, although this may produce an exception at runtime if the classes aren’t related.
7. The compiler will only report an unrelated type error for an instanceof operation with an interface on the right side if the reference on the left side is a final class that does not inherit the interface.
8. An interface method with a body must be marked default, private, static, or
private static.

## Abstract Interface Method Rules
1. Abstract methods can be defined only in abstract classes or interfaces.
2. Abstract methods cannot be declared private or final.
3. Abstract methods must not provide a method body/implementation in the abstract class in which is it declared.
4. Implementing an abstract method in a subclass follows the same rules for overriding a method, including covariant return types, exception declarations, etc.
5. Interface methods without a body are assumed to be abstract and public.
Notice anything? The first four rules for abstract methods, whether they be defined in abstract classes or interfaces, are exactly the same! The only new rule you need to learn for interfaces is the last one.


## Interface Variables Rules
1. Interface variables are assumed to be public, static, and final.
2. Because interface variables are marked final, they must be initialized with a value when they are declared.


## Inner Class

**four types of nested classes:**
- inner classes, 
- local classes, 
- anonymous classes,
- static nested classes

```java
public class Zoo {
    private interface Paper {}
    public class Ticket implements Paper {}
}
```  

**IMPORTANT !!!!!**

*If a inner class is abstract, it can mark as **private** or **static***

```java
abstract class Cinema {
	abstract private static class Ticket {

	}
}
```  
<br/> -
