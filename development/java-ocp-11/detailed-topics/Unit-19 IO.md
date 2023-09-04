# Unit 19 -  I/O  
##  Files and Directories
<br/>

### **FILE CLASS**

The File class cannot read or write data within a file, although it can be passed as a reference to many stream classes to read or write data.

```java
public File(String pathname)
public File(File parent, String child)
public File(String parent, String child)
```  
```java
File zooFile2 = new File("/home/tiger", "data/stripes.txt");
 
File parent = new File("/home/tiger");
File zooFile3 = new File(parent, "data/stripes.txt");
```   

<br/> 

📺 *Commonly used java.io.File methods*  

|Method Name|Description|
|--- |--- |
|boolean delete()|Deletes the file or directory and returns true only if successful. If this instance denotes a directory, then the directory must be empty in order to be deleted.|
|boolean exists()|Checks if a file exists|
|String getAbsolutePath()|Retrieves the absolute name of the file or directory within the file system|
|String getName()|Retrieves the name of the file or directory.|
|String getParent()|Retrieves the parent directory that the path is contained in or null if there is none|
|boolean isDirectory()|Checks if a File reference is a directory within the file system|
|boolean isFile()|Checks if a File reference is a file within the file system|
|long lastModified()|Returns the number of milliseconds since the epoch (number of milliseconds since 12 a.m. UTC on January 1, 1970) when the file was last modified|
|long length()|Retrieves the number of bytes in the file|
|File[] listFiles()|Retrieves a list of files within a directory|
|boolean mkdir()|Creates the directory named by this path|
|boolean mkdirs()|Creates the directory named by this path including any nonexistent parent directories|
|boolean renameTo(File dest)|Renames the file or directory denoted by this path to dest and returns true only if successful|


<br/>
<br/>  

**Example :**
```java
var file = new File("c:\\data\\zoo.txt");
System.out.println("File Exists: " + file.exists());
if (file.exists()) {
   System.out.println("Absolute Path: " + file.getAbsolutePath());
   System.out.println("Is Directory: " + file.isDirectory());
   System.out.println("Parent Path: " + file.getParent());
   if (file.isFile()) {
      System.out.println("Size: " + file.length());
      System.out.println("Last Modified: " + file.lastModified());
   } else {
      for (File subfile : file.listFiles()) {
         System.out.println("   " + subfile.getName());
      }
   }
}
```  
<br/> 

*If the path provided did not point to a file :*   
```java
File Exists: false
```   
<br/>

*If the path provided pointed to a valid file:*  
```java
File Exists: true
Absolute Path: c:\data\zoo.txt
Is Directory: false
Parent Path: c:\data
Size: 12382
Last Modified: 1606860000000
```  
<br/>

*If the path provided pointed to a valid directory, such as c:\data  :*

```java
File Exists: true
Absolute Path: c:\data
Is Directory: true
Parent Path: c:\
   employees.txt
   zoo.txt
   zoo-backup.txt
```    
<br/>

🔴 **NOTE :** */data/zoo.txt* could be a file or a directory, even though it has a file extension.


<br/>
<br/>

## I/O Streams

<br/>

### **Byte Streams vs. Character Streams**

<br/>

Differences between Byte and Character Streams:

- *Byte streams* read/write binary data ( 0s and 1s) and have class names that end in **InputStream** or **OutputStream**.  
- *Character streams* read/write text data and have class names that end in **Reader** or **Writer**.

Using a character stream is better for working with text data than a byte stream

The API frequently includes similar classes for both byte and character streams, such as **FileInputStream** and **FileReader**.  

🔴 It is important to remember that even though character streams do not contain the word Stream in their class name, they are still I/O streams. The use of *Reader*/ *Writer* in the name is just to distinguish them from byte streams.

<br/>

*Character Encoding*

```java
Charset usAsciiCharset = Charset.forName("US-ASCII");
Charset utf8Charset = Charset.forName("UTF-8");
Charset utf16Charset = Charset.forName("UTF-16");
```  

### **Input vs. Output Streams**

<br/>

📺 Corresponding 

|Input | Output|  
|-|--|  
|InputStream |OutputStream |  
|FileInputStream|FileOutputStream |  
|FileReader|FileWriter |  
 
<br/>

🔴 **PrintWriter** has no accompanying **PrintReader** class  .
**PrintStream** is an **OutputStream** that has no corresponding **InputStream** class.

<br/>

### **Low‐Level vs. High‐Level Streams**  

A low‐level stream connects directly with the source of the data, such as a file, an array, or a String.   
For example, a *FileInputStream* is a class that reads file data one byte at a time.  

A high‐level stream is built on top of another stream using wrapping.  
For example, take a look at the *FileReader* and *BufferedReader* objects 

```java
try (var br = new BufferedReader(new FileReader("zoo-data.txt"))) {
   System.out.println(br.readLine());
}
```    
*In this example, FileReader is the low‐level stream reader, whereas BufferedReader is the high‐level stream that takes a FileReader as input.*


High‐level streams can take other high‐level streams as input.

```java
try (var ois = new ObjectInputStream(
      new BufferedInputStream(
         new FileInputStream("zoo-data.txt")))) {
   System.out.print(ois.readObject());
}
```  
<br/> 

🔴 Use **Buffered** streams when working with files. Buffered classes read or write data in groups, rather than a single byte or character at a time. It gain performance.   
For example, accessing 1,600 sequential bytes is a lot faster than accessing 1,600 bytes spread across the hard drive.

<br/>

### **Stream Base Classes**

The java.io library defines four abstract classes that are the parents of all stream classes defined within the API: **InputStream**, **OutputStream**, **Reader**, and **Writer**.

The constructors of high‐level streams often take a reference to the abstract class.   
For example, *BufferedWriter* takes a *Writer* object as input, which allows it to take any subclass of *Writer*.

```java
new BufferedInputStream(new FileReader("z.txt"));       // DOES NOT COMPILE
new BufferedWriter(new FileOutputStream("z.txt"));      // DOES NOT COMPILE
new ObjectInputStream(new FileOutputStream("z.txt"));   // DOES NOT COMPILE
new BufferedInputStream(new InputStream());             // DOES NOT COMPILE
```    
The first two examples do not compile because they mix *Reader/ Writer* classes with *InputStream/ OutputStream* classes, respectively.   
The third example does not compile because we are mixing an *OutputStream* with an *InputStream*. Although it is possible to read data from an *InputStream* and write it to an *OutputStream*, wrapping the stream is not the way to do so. The data must be copied over, often iteratively.   
Finally, the last example does not compile because *InputStream* is an *abstract* class, and therefore you cannot create an instance of it.

<br/>
<br/>

### **Decoding I/O Class Names**  
<br/>

#### **Review of java.io Class Name Properties :**   

* A class with the word *InputStream* or *OutputStream* in its name is used for reading or writing binary or byte data, respectively.
* A class with the word *Reader* or *Writer* in its name is used for reading or writing character or string data, respectively.
* Most, but not all, input classes have a corresponding output class.
* A low‐level stream connects directly with the source of the data.
* A high‐level stream is built on top of another stream using wrapping.
* A class with *Buffered* in its name reads or writes data in groups of bytes or characters and often improves performance in sequential file systems.
* With a few exceptions, you only wrap a stream with another stream if they share the same abstract parent.

For the last rule, we'll cover some of those exceptions (like wrapping an *OutputStream* with a *PrintWriter*).  

<br/>

📺 *The java.io abstract stream base classes*

|Class Name|Description|
|--- |--- |
|InputStream|Abstract class for all input byte streams|
|OutputStream|Abstract class for all output byte streams|
|Reader|Abstract class for all input character streams|
|Writer|Abstract class for all output character streams|

<br/>

📺  *The java.io concrete stream classes*  

|Class Name|Low/High Level|Description|
|--- |--- |--- |
|FileInputStream|Low|Reads file data as bytes|
|FileOutputStream|Low|Writes file data as bytes|
|FileReader|Low|Reads file data as characters|
|FileWriter|Low|Writes file data as characters|
|BufferedInputStream|High|Reads byte data from an existing InputStream in a buffered manner, which improves efficiency and performance|
|BufferedOutputStream|High|Writes byte data to an existing OutputStream in a buffered manner, which improves efficiency and performance|
|BufferedReader|High|Reads character data from an existing Reader in a buffered manner, which improves efficiency and performance|
|BufferedWriter|High|Writes character data to an existing Writer in a buffered manner, which improves efficiency and performance|
|ObjectInputStream|High|Deserializes primitive Java data types and graphs of Java objects from an existing InputStream|
|ObjectOutputStream|High|Serializes primitive Java data types and graphs of Java objects to an existing OutputStream|
|PrintStream|High|Writes formatted representations of Java objects to a binary stream|
|PrintWriter|High|Writes formatted representations of Java objects to a character stream|

<br/>
<br/>

## Common I/O Stream Operations

### **READING AND WRITING DATA**

```java
// InputStream and Reader
public int read() throws IOException
```  
```java
// OutputStream and Writer
public void write(int b) throws IOException
```    

We said we are reading and writing bytes, so why do the methods use int instead of byte?   

The byte data type has a range of 256 characters. They needed an extra value to indicate the end of a stream.

End of the stream value is -1.  


```java
void copyStream(InputStream in, OutputStream out) throws IOException {
   int b;
   while ((b = in.read()) != -1) {
      out.write(b);
   }
}
 
void copyStream(Reader in, Writer out) throws IOException {
   int b;
   while ((b = in.read()) != -1) {
      out.write(b);
   }
}
```  
<br/> 
<br/> 

```java
// InputStream
public int read(byte[] b) throws IOException
public int read(byte[] b, int offset, int length) throws IOException
 
// OutputStream
public void write(byte[] b) throws IOException
public void write(byte[] b, int offset, int length) throws IOException
```    
```java
// Reader
public int read(char[] c) throws IOException
public int read(char[] c, int offset, int length) throws IOException
 
// Writer
public void write(char[] c) throws IOException
public void write(char[] c, int offset, int length) throws IOException
```  
<br/> 

### **CLOSING THE STREAM**

🔴 All I/O streams implement *Closeable*.

```java
// All I/O stream classes
public void close() throws IOException
```    
```java
try (var fis = new FileInputStream("zoo-data.txt")) {
   System.out.print(fis.read());
}
```  

<br/> 

**Example :**
```java
try (var ois = new ObjectOutputStream(
        new BufferedOutputStream(
            new FileOutputStream("zoo-banner.txt")))) {
    ois.writeObject("Hello");
}
```    
<br/> 

### **MANIPULATING INPUT STREAMS**

```java
// InputStream and Reader
public boolean <b>markSupported()</b>
public void void mark(int readLimit)
public void reset() throws IOException
public long skip(long n) throws IOException
```  

#### **mark() and reset()**  

```java
public void readData(InputStream is) throws IOException {
   System.out.print((char) is.read());     // L
   if (is.markSupported()) {
      is.mark(100);  // Marks up to 100 bytes
      System.out.print((char) is.read());  // I
      System.out.print((char) is.read());  // O
      is.reset();    // Resets stream to position before I
   }
   System.out.print((char) is.read());    // I
   System.out.print((char) is.read());    // O
   System.out.print((char) is.read());    // N
}
```    
<br/> 

#### **skip()** 


```java
System.out.print ((char)is.read()); // T
is.skip(2);  // Skips I and G
is.read();   // Reads E but doesn't output it
System.out.print((char)is.read());  // R
System.out.print((char)is.read());  // S
```  

### **FLUSHING OUTPUT STREAMS**

```java
// OutputStream and Writer
public void flush() throws IOException
```    



📺 *Common I/O stream methods*  

<table border="2">
<thead>
<tr>
<td >Stream Class</td>
<td >Method Name</td>
<td >Description</td> </tr> </thead>
<tbody>
<tr>
<td >All streams</td>
<td >
<code>void close()</code> </td>
<td >Closes stream and releases resources</td> </tr>
<tr>
<td >All input streams</td>
<td >
<code>int read()</code> </td>
<td >Reads a single byte or returns <code>‐1</code> if no bytes were available</td> </tr>
<tr>
<td >
<code>InputStream</code> </td>
<td >
<code>int read(byte[] b)</code> </td>
<td  rowspan="2">Reads values into a buffer. Returns number of bytes read</td> </tr>
<tr>
<td >
<code>Reader</code> </td>
<td >
<code>int read(char[] c)</code> </td> </tr>
<tr>
<td >
<code>InputStream</code> </td>
<td >
<code>int read(byte[] b, int offset, int length)</code> </td>
<td  rowspan="2">Reads up to <code>length</code> values into a buffer starting from position <code>offset</code>. Returns number of bytes read</td> </tr>
<tr>
<td >
<code>Reader</code> </td>
<td >
<code>int read(char[] c, int offset, int length)</code> </td> </tr>
<tr>
<td >All output streams</td>
<td >
<code>void write(int)</code> </td>
<td >Writes a single byte</td> </tr>
<tr>
<td >
<code>OutputStream</code> </td>
<td >
<code>void write(byte[] b)</code> </td>
<td  rowspan="2">Writes an array of values into the stream</td> </tr>
<tr>
<td >
<code>Writer</code> </td>
<td >
<code>void write(char[] c)</code> </td> </tr>
<tr>
<td >
<code>OutputStream</code> </td>
<td >
<code>void write(byte[] b, int offset, int length)</code> </td>
<td  rowspan="2">Writes <code>length</code> values from an array into the stream, starting with an <code>offset</code> index</td> </tr>
<tr>
<td >
<code>Writer</code> </td>
<td >
<code>void write(char[] c, int offset, int length)</code> </td> </tr>
<tr>
<td >All input streams</td>
<td >
<code>boolean markSupported()</code> </td>
<td >Returns <code>true</code> if the stream class supports <code>mark()</code></td> </tr>
<tr>
<td >All input streams</td>
<td >
<code>mark(int readLimit)</code> </td>
<td >Marks the current position in the stream</td> </tr>
<tr>
<td >All input streams</td>
<td >
<code>void reset()</code> </td>
<td >Attempts to reset the stream to the <code>mark()</code> position</td> </tr>
<tr>
<td >All input streams</td>
<td >
<code>long skip(long n)</code> </td>
<td >Reads and discards a specified number of characters</td> </tr>
<tr>
<td >All output streams</td>
<td >
<code>void flush()</code> </td>
<td >Flushes buffered data through the stream</td> </tr> </tbody> </table> 

<br/> 
<br/> 

## Working with I/O Stream Classes

### **READING AND WRITING BINARY DATA**

```java
public FileInputStream(File file) throws FileNotFoundException
public FileInputStream(String name) throws FileNotFoundException
 
public FileOutputStream(File file) throws FileNotFoundException
public FileOutputStream(String name) throws FileNotFoundException
```  
<br/> 

**Example 1:**
```java
void copyFile(File src, File dest) throws IOException {
   try (var in = new FileInputStream(src);
        var out = new FileOutputStream(dest)) {
      int b;
      while ((b = in.read()) != -1) {
         out.write(b);
      }
   }
}
```

### **BUFFERING BINARY DATA**



```java
public BufferedInputStream(InputStream in)

public BufferedOutputStream(OutputStream out)
```  
<br/> 

**Example 1:**
```java
void copyFileWithBuffer(File src, File dest) throws IOException {
   try (var in = new BufferedInputStream(
           new FileInputStream(src));
        var out = new BufferedOutputStream(
           new FileOutputStream(dest))) {
      var buffer = new byte[1024];
      int lengthRead;
      while ((lengthRead = in.read(buffer))> 0) {
         out.write(buffer, 0, lengthRead);
         out.flush();
      }
   }
}
```    

### **READING AND WRITING CHARACTER DATA**  


```java
public FileReader(File file) throws FileNotFoundException
public FileReader(String name) throws FileNotFoundException
 
public FileWriter(File file) throws FileNotFoundException
public FileWriter(String name) throws FileNotFoundException
```  
```java
void copyTextFile(File src, File dest) throws IOException {
   try (var reader = new FileReader(src);
        var writer = new FileWriter(dest)) {
      int b;
      while ((b = reader.read()) != -1) {
         writer.write(b);
      }
   }
}
```    
<br/>  

The *FileWriter* inherits a method from the *Writer* class that allows it to write String values.
```java
// Writer
public void write(String str) throws IOException
```  
<br/> 

The following is supported in *FileWriter* but not *FileOutputStream*:
```java
writer.write("Hello World");
```
<br/>      

### **BUFFERING CHARACTER DATA**  

```java
public BufferedReader(Reader in)
 
public BufferedWriter(Writer out)
```  
```java
// BufferedReader
public String readLine() throws IOException
 
// BufferedWriter
public void newLine() throws IOException
```    

**Example :**
```java
void copyTextFileWithBuffer(File src, File dest) throws IOException {
   try (var reader = new BufferedReader(new FileReader(src));
        var writer = new BufferedWriter(new FileWriter(dest))) {
      String s;
      while ((s = reader.readLine()) != null) {
         writer.write(s);
         writer.newLine();
      }
   }
}
``` 




<br/> 
<br/> 

### **SERIALIZING**

 *Serialization* is the process of converting an in‐memory object to a byte stream. Likewise, *deserialization*  is the process of converting from a byte stream into an object.


To serialize an object using the I/O API, the object must implement the j*ava.io.Serializable* interface. The *Serializable* interface is a marker interface.  
By marker interface, it means the interface does not have any methods.

Any field that is marked **transient** will not be saved to a stream when the class is serialized.

 The transient modifier is used for sensitive data of the class, like a password.

<br/>

 **How to Make a Class Serializable**

- The class must be marked Serializable.
- Every instance member of the class is serializable, marked transient, or has a null value at the time of serialization.

**Example :** Why Cat class is not serializable ?
```java
public class Cat implements Serializable {
   private Tail tail = new Tail();
}
 
 
public class Tail implements Serializable {
   private Fur fur = new Fur();
}
 
public class Fur {}
```
Fur is not marked Serializable.

<br/>

**Example :** It can serializable.
```java
public class Tail implements Serializable {
   private transient Fur fur = new Fur();
}
 
public class Fur implements Serializable {}
```

<br/>

### **Storing Data with ObjectOutputStream and ObjectInputStream**

The *ObjectInputStream* class is used to deserialize an object from a stream, while the *ObjectOutputStream* is used to serialize an object to a stream

```java
public ObjectInputStream(InputStream in) throws IOException
 
public ObjectOutputStream(OutputStream out) throws IOException
```

```java
// ObjectInputStream
public Object readObject() throws IOException, ClassNotFoundException
 
// ObjectOutputStream
public void writeObject(Object obj) throws IOException
```

**Example :**
```java
void saveToFile(List<Gorilla> gorillas, File dataFile)
      throws IOException {
   try (var out = new ObjectOutputStream(
           new BufferedOutputStream(
              new FileOutputStream(dataFile)))) {
      for (Gorilla gorilla : gorillas)
         out.writeObject(gorilla);
   }
}
```

### **Deserialization Creation Process**

Any *static* or *transient* fields are ignored. Values that are not provided will be given their default Java value, such as *null* for String, or *0* for int values.

**Example :**
```java
import java.io.Serializable;
public class Chimpanzee implements Serializable {
   private static final long serialVersionUID = 2L;
   private transient String name;
   private transient int age = 10;
   private static char type = 'C';
   { this.age = 14; }
 
   public Chimpanzee() {
      this.name = "Unknown";
      this.age = 12;
      this.type = 'Q';
   }
 
   public Chimpanzee(String name, int age, char type) {
      this.name = name;
      this.age = age;
      this.type = type;
   }
 
   // Getters/Setters/toString() omitted
}
```

```java
var chimpanzees = new ArrayList<Chimpanzee>();
chimpanzees.add(new Chimpanzee("Ham", 2, 'A'));
chimpanzees.add(new Chimpanzee("Enos", 4, 'B'));
File dataFile = new File("chimpanzee.data");
 
saveToFile(chimpanzees, dataFile);
var chimpanzeesFromDisk = readFromFile(dataFile);
System.out.println(chimpanzeesFromDisk);
```

None of the instance members would be serialized to a file. The name and age variables are both marked transient, while the type variable is static. We purposely accessed the type variable using this to see whether you were paying attention

Upon deserialization, none of the constructors in Chimpanzee is called. Even the no‐arg constructor that sets the values [ name=Unknown,age=12,type=Q] is ignored. The instance initializer that sets age to 14 is also not executed.

 Since it's static, it will actually display whatever value was set last.

<br/> 

**Print :**
```java
[[name=null,age=0,type=B],
 [name=null,age=0,type=B]]
```




### **PRINTING DATA**

*PrintStream* and *PrintWriter* are high‐level output print streams classes that are useful for writing text data to a stream.

 
<br/>

**Example :** Constructors
```java
public PrintStream(OutputStream out)
public PrintStream(File file) throws FileNotFoundException
public PrintStream(String fileName) throws FileNotFoundException
 
public PrintWriter(Writer out)
public PrintWriter(File file) throws FileNotFoundException
public PrintWriter(String fileName) throws FileNotFoundException
```
<br/>


<center> 

### **Review Of Stream Classes** 
</center>

![images.info](.././images/stream-classes.png)

 
 
<br/>

## SYSTEM STREAMS  

🔴 You shouldn't close to System.out, System.err, and System.in.  
Because these are static objects, the System streams are shared by the entire application. The JVM creates and opens them for us. They can be used in a try‐with‐resources statement or by calling close(), although closing them is not recommended. Closing the System streams makes them permanently unavailable for all threads in the remainder of the program.


**Example :**
```java
try (var out = System.out) {}
System.out.println("Hello");  
```
It prints nothing. Because System.out is closed after try.

*PrintStream* do not throw any checked exceptions and rely on the *checkError()* to report errors.

<br/>

## CONSOLE

*The java.io.Console* class is specifically designed to handle user interactions. After all, *System.in* and *System.out* are just raw streams, whereas *Console* is a class with numerous methods centered around user input.

**The Console class is a singleton**
```java
Console console = System.console();
Console console2 = new Console();  // DOES NOT COMPILE
```
<br/>


**Methods :**
```java
public Reader reader()
public PrintWriter writer()

public Console format(String format, Object… args)

public String readLine()
public String readLine(String fmt, Object… args)
 
public char[] readPassword()
public char[] readPassword(String fmt, Object… args) 
```

<br/>

**Example 1 :**
```java
Console console = System.console();
if (console == null) {
   throw new RuntimeException("Console not available");
} else {
   console.writer().println("Welcome to Our Zoo!");
   console.format("It has %d animals and employs %d people", 391, 25);
   console.writer().println();
   console.printf("The zoo spans %5.1f acres", 128.91);
}
```
<br/>

**Print :**
```
Welcome to Our Zoo!
It has 391 animals and employs 25 people
The zoo spans 128.9 acres.
```
<br/>


**Example 2 :**
```
Console console = System.console();
if (console == null) {
   throw new RuntimeException("Console not available");
} else {
   String name = console.readLine("Please enter your name: ");
   console.writer().format("Hi %s", name);
   console.writer().println();
 
   console.format("What is your address? ");
   String address = console.readLine();
 
   char[] password = console.readPassword("Enter a password " 
      + "between %d and %d characters: ", 5, 10);
   char[] verify = console.readPassword("Enter the password again: ");
   console.printf("Passwords "
      + (Arrays.equals(password, verify) ? "match" : "do not match"));
}
```
<br/>

**Print :**
```
Please enter your name: Max
Hi Max
What is your address? Spoonerville
Enter a password between 5 and 10 digits:
Enter the password again:
Passwords match
```
<br/>
 


<br/> 

📺
☝ 
🟡

🔴
〰️ 
<br/> 
<br/> 
