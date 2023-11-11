
# JAVA 17 Features  <!-- omit in toc -->

- [1. Text Block](#1-text-block)
- [2. Pattern Matching for Switch](#2-pattern-matching-for-switch)
  - [2.1. Arrow Operator **-\>**](#21-arrow-operator--)
  - [2.2. If else improved](#22-if-else-improved)
  - [2.3. Pattern matching and null](#23-pattern-matching-and-null)
- [3. Record](#3-record)
- [4. **sealed** class](#4-sealed-class)



## 1. Text Block

**Example 1:**  
 
```java
String json="""
        {
            "name": "cihan"
            "surname: "dogan"
        }
        """;

System.out.println(json);
```

print:  
```json
{
    "name": "cihan"
    "surname": "dogan"
}
```

**Example 2:** 

```java
String sql = """
        SELECT *
        FROM
        CURRENCY;
        """;

entityManage.createNativeQuery(sql);
```


## 2. Pattern Matching for Switch 
### 2.1. Arrow Operator **->**
**Before :** 
```java
public String getMonth(int index) {
    String month = "";

    switch (index) {
        case 1:
            month = "January";
            break;
        case 2:
            month = "February";
            break;
        case 3:
            month = "March";
            break;
         // ...
         // ...
        default:
            month = "Invalid";
    }

    return month;
}
```

**After :** 
```java
public String getMonth2(int index) {
   return switch (index) {
        case 1 -> "January";
        case 2 -> "February";
        case 3 -> "March";
        // ...
        // ...
        default-> "Invalid";
    };
}
```

### 2.2. If else improved 
**Before :**

```java
 public String formatter(Object o) {
    String formatted = "unknown";
    if (o instanceof Integer i) {
        formatted = String.format("int %d", i);
    } else if (o instanceof Long l) {
        formatted = String.format("long %d", l);
    } else if (o instanceof Double d) {
        formatted = String.format("double %f", d);
    } else if (o instanceof String s) {
        formatted = String.format("String %s", s);
    }
    return formatted;
}
```

**After :**
```java

public String formatter(Object o) {
    return switch (o) {
        case Integer i -> String.format("int %d", i);
        case Long l    -> String.format("long %d", l);
        case Double d  -> String.format("double %f", d);
        case String s  -> String.format("String %s", s);
        default        -> o.toString();
    };
}
```

###  2.3. Pattern matching and null

```java
public String getTypes(Object obj) {
  return  switch (obj) {
        case String s -> "It's a string";
        case Liskov l && l.getSubstitution() == 1 -> "Conditional Statement";
        case int[] numbers -> "Array Type";
        default -> "Invalid";
  };
}
```

<br/>



## 3. Record

There are a lot of code mess because of dto and pojo classes. Record makes it simple.  
**Before Record :**

```java
public class UserDto {
    private int id;
    private String name;

    public UserDto(int id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        UserDto userDto = (UserDto) o;
        return id == userDto.id && Objects.equals(name, userDto.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id, name);
    }
    
    public String toString() {
        return "(id=" + this.id + ", name=" + this.name + ")";
    }
}
```
**Tests :** 
```java
UserDto user1 = new UserDto(1, "cihan");
UserDto user2 = new UserDto(1, "cihan");
System.out.println(user1);
System.out.println("equals= " + user1.equals(user2));
```
**Print:** 
```
(id=1, name=cihan)
equals= true
```

<br/>

**After Record :**

```java
record UserRecord(int id, String name) { }
```

```java
UserRecord user1 = new UserRecord(1, "cihan");
UserRecord user2 = new UserRecord(1, "cihan");
System.out.println(user1);
System.out.println("equals= " + user1.equals(user2));
```

**Print:**
```
(id=1, name=cihan)
equals= true
```

- Records already have constructor with all fields and implements **toString()**, **hashCode()** and **equals()** methods.
- Records can extend or implements other classes/interfaces.
- Records cannot be extended/implemented by other classes.
- Records can not be private or protected.



## 4. **sealed** class

Sealed classes help manage inheritance in Java. Before the introduction of sealed classes, we could only use "final" to
restrict a class from being extended. However, with "final," we were unable to permit one class to be extended while
restricting another.
Java17 addresses this issue using sealed classes.


**Final** restricts class A from being extended. 
```java
final class A{}

class B extends A{}  // DOES NOT COMPILE!!

class C extends A{}  // DOES NOT COMPILE!!
```

What if I want to restrict class B from extending class A but permit class C and D to be extended?

Solve it with **sealed**.

```java
sealed class A permits C, D{}

non-sealed class C extends A{}  // Compile

final class D extends A{}       // Compile

class B extends A{}  // DOES NOT COMPILE!
```


