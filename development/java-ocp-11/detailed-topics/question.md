**Question 1: Which of the following four constructs are valid?**

```java
// 1.
   switch(5)
  {
      default :
   }

// 2.
   switch(5)
   {
      default : break;
   }

// 3.
  switch(8);

// 4.
var x = 0;
switch(x){
}

```
**Answer : 1, 2, 4**

<br/>
<br/>

Standard Tests - Test 10 :  Q34

 **Question 2: Which of the given statements are correct about the following code fragment:**

``` java
ConcurrentMap<String, Object> cache = new ConcurrentHashMap<>();

if(!cache.containsKey(key)) cache.put(key, value);
```
 
**Answer : To ensure that an entry in cache must never be overwritten, this statement should be replaced with:**

``` java
cache.putIfAbsent(key, value);
```


<br/>
<br/>


🟡 NIO - Standard Tests - Test 10 / Q16
🟡 Security  Standard Tests - Test 10 / Q28
🟡 Security  Standard Tests - Test 10 / Q40
🟡 Security  Standard Tests - Test 10 / Q53
🟡 IO - Standard Tests - Test 10 / Q31
🟡 Concurrency - Standard Tests - Test 10 / Q48 (deadlock)
🟡 Concurrency - Standard Tests - Test 10 / Q49
🟡 Collections  - Standard Tests - Test 10 / Q50 (NavigableMap)
 
 

<hr>


## Arrays
### **compare(a,b)**
* a.length == b.length -> -1
* a==b  -> 0
* else -> a-b
* a == null -> -1
* b == null -> 1

### **mismatch(a,b)**
* a==b -> -1
* else first mismatches index return 
* if there are not matches element, return 0


```java
int[] a = { 'h', 'e', 'a'};
int[] b = { 'h', 'e', 'l'};

int x = Arrays.compare(a, b);
int y = Arrays.mismatch(a, b);
System.out.println(x+" "+y);   // -1 2
```

```java
int[] a = { 'a', 'a', 'a'};
int[] b = { 'h', 'e', 'l'};

int x = Arrays.compare(a, b);
int y = Arrays.mismatch(a, b);
System.out.println(x+" "+y);    // -1 0

```







🟡

🔴