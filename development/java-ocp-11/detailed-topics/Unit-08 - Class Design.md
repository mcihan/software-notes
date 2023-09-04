

# Unit 8 - Class Design

*Example: define and extend many classes within the same Java file*   
Groundhog.java
```java 
class Rodent {}

public class Groundhog extends Rodent {} // compile
```

## Missing a Default No-Argument Constructor
```java 

public class Mammal {
    public Mammal(int age) {}
}
public class Elephant extends Mammal { // DOES NOT COMPILE }
```

## Constructors and final Fields
*final field must be initialize*

```java 
public class MouseHouse {
	private final String type;

	public MouseHouse() { // DOES NOT COMPILE

	}
}
```

## Overloaded Constructors

*Example 1:* 
```java
public Hamster(int weight) {
    Hamster(weight, "brown");       // DOES NOT COMPILE
}

public Hamster(int weight) {
    new Hamster(weight, "brown");   // Compiles, but incorrect
}
```  

*Example 2:  
When this() is used with parentheses, Java calls another constructor on the same instance of the class.*

```java
public Hamster(int weight) { 
    this(weight, "brown");
}
```  
*Example 3:  
the this() call must be the first statement in the constructor*

```java
public Hamster(int weight) {
    System.out.println("in constructor");
    // Set weight and default color
    this(weight, "brown"); // DOES NOT COMPILE
```  

*Example 4 : **Recursive Constructor Error*** 
```java
public class Human {
    int age;

    public Human(int age){ // DOESN'T COMPILE !!!
       this(1); 
    }
}
```  

*Example 5 : **Recursive Constructor Error*** 

```java
public class Human {
    int age;

    public Human(){
        this(1);        // DOESN'T COMPILE !!!
    }
    public Human(int age){
        this();         // DOESN'T COMPILE !!!
    }
}
```  
 ## Order of Initialization

 ### Class Initialization

 
#### Initialize Class X  
 
-  If there is a superclass Y of X, then initialize class Y first.
- Process all static variable declarations in the order they appear in the class.  
- Process all static initializers in the order they appear in the class.

```java
public class Animal {
	static {
		System.out.print("A");
	}
}

public class Hippo extends Animal {
	static {
		System.out.print("B");
	}

	public static void main(String[] grass) {
		System.out.print("C");
		new Hippo();
		new Hippo();
		new Hippo();
	}
}
```  
It prints ABC exactly once. Since the main() method is inside the Hippo class, the class will be initialized first, starting with the superclass and printing AB. Afterward, the main() method is executed, printing C. Even though the main() method creates three instances, the class is loaded only once.  


```java
public class HippoFriend {
	public static void main(String[] grass) {
		System.out.print("C");
		new Hippo();
	}
}
```  
*print **CAB***

#### Instance Initialization


```java
public class Cuttlefish {
	private String name = "swimmy";
	
	{ System.out.println(name); }
	
	private static int COUNT = 0;
	
	static { System.out.println(COUNT);}
	
	{	
		COUNT++;
		System.out.println(COUNT);
	}

	public Cuttlefish() {
		System.out.println("Constructor");
	}

	public static void main(String[] args) {
		System.out.println("Ready");
		new Cuttlefish();
	}
}
```  
*print:*
```java
0  
Ready   
swimmy  
1   
Constructor
```


<br/> 


*Example2:*
  
**Animal.java** 
```java
class Animal {
	static {
		System.out.println("Animal Static Block");
	}
	{
		System.out.println("Animal Normal Block");
	}
	
	public Animal() {
		System.out.println("Animal Default Cons");
	}

	public Animal(String name) {
		this(1);
		System.out.println("Animal String Cons");
	}

	public Animal(int stripes) {
		System.out.println("Animal int Cons");
	}
}
```  
**Monkey.java** 
```java
public class Monkey extends Animal {
	static {
		System.out.println("Monkey Static Block");
	}
	
	{
		System.out.println("Monkey Normal Block");
	}

	public Monkey() {
		System.out.println("Monkey Default Cons");
	}

	public Monkey(int stripes) {
		super("stripes");
		System.out.println("Monkey Int Cons");
	}
	
	public Monkey(String name) {
		super("Roar");
		System.out.println("Monkey String Cons");
	}

	public static void main(String[] grass) {
		new Monkey();
		System.out.println("--------------------");
		new Monkey(1);
		System.out.println("--------------------");
		new Monkey("TestName");
	}
}
```  
**Print:**  
```java
Animal Static Block
Monkey Static Block
Animal Normal Block
Animal Default Cons
Monkey Normal Block
Monkey Default Cons
--------------------
Animal Normal Block
Animal int Cons
Animal String Cons
Monkey Normal Block
Monkey Int Cons
--------------------
Animal Normal Block
Animal int Cons
Animal String Cons
Monkey Normal Block
Monkey String Cons
```  


**Reviewing Constructor Rules**

 
* The first statement of every constructor is a call to an overloaded constructor via this(), or a direct parent constructor via super().
* If the first statement of a constructor is not a call to this() or super(), then the compiler will insert a no-argument super() as the first statement of the constructor.
* Calling this() and super() after the first statement of a constructor results in a compiler error.
* If the parent class doesn’t have a no-argument constructor, then every constructor in the child class must start with an explicit this() or super() constructor call.
* If the parent class doesn’t have a no-argument constructor and the child doesn’t define any constructors, then the child class will not compile.
* If a class only defines private constructors, then it cannot be extended by a top-level class.
* All final instance variables must be assigned a value exactly once by the end of the constructor. Any final instance variables not assigned a value will be reported as a compiler error on the line the constructor is declared.

## Inheriting Members

### Inheriting Methods

#### Overriding a Method

To override a method, you must follow a number of rules. The compiler performs the following checks when you override a method:
1. The method in the child class must have the same signature as the method in the parent class.
2. The method in the child class must be at least as accessible as the method in the parent class.
3. The method in the child class may not declare a checked exception that is new or broader than the class of any exception declared in the parent class method.
4. If the method returns a value, it must be the same or a subtype of the method in the parent class, known as covariant return types.

    
<br/> 
 

**Overloading vs. Overriding**

```java
public class Bird {
    public void fly() {
        System.out.println("Bird is flying");
    }

    public void eat(int food) {
        System.out.println("Bird is eating " + food + " units of food");
    }
}

public class Eagle extends Bird {
    public int fly(int height) {
        System.out.println("Bird is flying at " + height + " meters");
        return height;
    }

    public int eat(int food) { // DOES NOT COMPILE 
        System.out.println("Bird is eating "+food+" units of food");
        return food;
    }
}
``` 

*The eat() method is overridden in the subclass Eagle, since the signature is the same as
it is in the parent class Bird*


<br/> 
 


**access modifiers**  

```java
public class Camel {
    public int getNumberOfHumps() {
        return 1;
    }
}

public class BactrianCamel extends Camel {
    private int getNumberOfHumps() { // DOES NOT COMPILE
        return 2;
    }
}
```  
*access modifier private is more restrictive than the one defined in the parent version of the method*



<br/> 
 
**Exceptions**
```java
public class Reptile {
    protected void sleepInShell() throws IOException {
    }

    protected void hideInShell() throws NumberFormatException {
    }

    protected void exitShell() throws FileNotFoundException {
    }
}

public class GalapagosTortoise extends Reptile {
    public void sleepInShell() throws FileNotFoundException {
    }

    public void hideInShell() throws IllegalArgumentException {
    }

    public void exitShell() throws IOException {  // DOES NOT COMPILE
    }
}
```  
**exitShell()* does not compile because child class exception (**IOException**) is broader than parent class exception (**FileNotFoundException**)*

<br/> 


**covariant return types**

```java
public class Rhino {
    protected CharSequence getName() {
        return "rhino";
    }

    protected String getColor() {
        return "grey, black, or white";
    }
}

class JavanRhino extends Rhino {
    public String getName() {
        return "javan rhino";
    }

    public CharSequence getColor() { // DOES NOT COMPILE 
        return "grey";
    }
}
``` 
*String implements the CharSequence interface, making String a subtype of CharSequence*

*The overridden getColor() method does not compile because CharSequence is not a subtype of String*


## Overriding a Generic Method

You cannot overload methods by changing the generic type due to type erasure.

```java
public class LongTailAnimal {
    protected void chew(List<Object> input) {}

    protected void chew(List<Double> input) {} // DOES NOT COMPILE
}
```  

*For the same reason, you also can’t overload a generic method in a parent class.*
```java
public class LongTailAnimal {
    protected void chew(List<Object> input) {}
}

public class Anteater extends LongTailAnimal {
    protected void chew(List<Double> input) {} // DOES NOT COMPILE
}
```  

```java
public class LongTailAnimal {
    protected void chew(List<Object> input) {}
}

public class Anteater extends LongTailAnimal {
    protected void chew(ArrayList<Double> input) {} // COMPILE
}
``` 

### Generic Return Types


```java
public class Mammal {
    public List<CharSequence> play() { ... }
    public CharSequence sleep() { ... }
}

public class Monkey extends Mammal {
    public ArrayList<CharSequence> play() { ... }
}

public class Goat extends Mammal {
    public List<String> play() { ... } // DOES NOT COMPILE
    public String sleep() { ... }
}
```  
*The Monkey class compiles because ArrayList is a subtype of List. The play() method in the Goat class does not compile, though. For the return types to be covariant, the generic type parameter must match. Even though String is a subtype of CharSequence, it does not exactly match the generic type defined in the Mammal class. Therefore, this is considered an invalid override.*

### Redeclaring private Methods

```java
public class Camel {
    private String getNumberOfHumps() {
        return "Undefined";
    }
}

public class DromedaryCamel extends Camel {
    private int getNumberOfHumps() {
        return 1;
    }
}
```  

*Child class method is not override of parent class method, it redeclared so it compile without issue. If parent class method were public or protected child class would not compile.*

## Hiding Static Methods

**The method defined in the child class must be marked as static if it is marked as static in a parent class.**
 

```java
public class Bear {
    public static void sneeze() {
        System.out.println("Bear is sneezing");
    }

    public void hibernate() {
        System.out.println("Bear is hibernating");
    }

    public static void laugh() {
        System.out.println("Bear is laughing");
    }
    public static void eat() {
        System.out.println("Bear is eating");
    }
}

public class Panda extends Bear {
    public void sneeze() { // DOES NOT COMPILE
        System.out.println("Panda sneezes quietly");
    }

    public static void hibernate() { // DOES NOT COMPILE 
        System.out.println("Panda is going to sleep");
    }

    protected static void laugh() { // DOES NOT COMPILE
        System.out.println("Panda is laughing");
    }

    public static void eat() {  // COMPILE !!
        System.out.println("Panda is chewing");
    }
}
```  

 *Child souldn't has a more restrictive access modifier than the one it inherits.*

## Creating final Methods

```java```  

*You cannot hide a static method in a child class if it is marked final in the parent class.*






## Understanding Polymorphism

**Interface Primer**

- An interface can define abstract methods.
- A class can implement any number of interfaces.
- A class implements an interface by overriding the inherited abstract methods.
- An object that implements an interface can be assigned to a reference for that
interface.
- Understand the rules for method overriding.
- Understand the rules for hiding methods and variables.
- Recognize the difference between method overriding and method overloading
- Understand polymorphism.


