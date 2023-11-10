# JAVA 17 Features

## Text Block

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


## Pattern Matching for Switch 
### Arrow Operator **->**
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

### If else improved 

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

###  Pattern matching and null

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

