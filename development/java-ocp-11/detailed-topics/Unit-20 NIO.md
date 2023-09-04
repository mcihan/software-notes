
## Path Create

```java
FileSystems.getDefault().getPath("puma.txt");
new java.io.File("tiger.txt").toPath();
Paths.get("ocelot.txt")
Path.of(new URI(""));
```

## ABSOLUTE VS. RELATIVE PATHS
- If a path starts with a forward slash ( /), it is absolute, with / as the root directory.  
Examples: /bird/parrot.png and /bird/../data/./info

- If a path starts with a drive letter ( c:), it is absolute, with the drive letter as the root directory.  
Examples: c:/bird/parrot.png and d:/bird/../data/./info

- Otherwise, it is a relative path.  
Examples: bird/parrot.png and bird/../data/./info


```java
Path path = Paths.get("/land/hippo/harry.happy");
System.out.println("The Path Name is: " + path);
for(int i=0; i<path.getNameCount(); i++) {
   System.out.println("   Element " + i + " is: " + path.getName(i));
}
```
**Print :**
```
The Path Name is: /land/hippo/harry.happy
   Element 0 is: land
   Element 1 is: hippo
   Element 2 is: harry.happy
```

📺**Path methods**
<table border="1">
<tbody>
<tr>
<td class="left">
<code>Path of(String, String…)</code> </td>
<td class="left">
<code>Path getParent()</code> </td> </tr>
<tr>
<td class="left">
<code>URI toURI()</code> </td>
<td class="left">
<code>Path getRoot()</code> </td> </tr>
<tr>
<td class="left">
<code>File toFile()</code> </td>
<td class="left">
<code>boolean isAbsolute()</code> </td> </tr>
<tr>
<td class="left">
<code>String toString()</code> </td>
<td class="left">
<code>Path toAbsolutePath()</code> </td> </tr>
<tr>
<td class="left">
<code>int getNameCount()</code> </td>
<td class="left">
<code>Path relativize()</code> </td> </tr>
<tr>
<td class="left">
<code>Path getName(int)</code> </td>
<td class="left">
<code>Path resolve(Path)</code> </td> </tr>
<tr>
<td class="left">
<code>Path subpath(int, int)</code> </td>
<td class="left">
<code>Path normalize()</code> </td> </tr>
<tr>
<td class="left">
<code>Path getFileName()</code> </td>
<td class="left">
<code>Path toRealPath(LinkOption…)</code> </td> </tr> </tbody> </table>


<br/>
<br/>

  
## FILES.READALLLINES() VS. FILES.LINES()  
For the exam, you need to know the difference between readAllLines() and lines(). Both of these examples compile and run:

```java
Files.readAllLines(Paths.get("birds.txt")).forEach(System.out::println);
Files.lines(Paths.get("birds.txt")).forEach(System.out::println);
```
   
The first line reads the entire file into memory and performs a print operation on the result, while the second line lazily processes each line and prints it as it is read. The advantage of the second code snippet is that it does not require the entire file to be stored in memory at any time.

You should also be aware of when they are mixing incompatible types on the exam. Do you see why the following does not compile?
```java
Files.readAllLines(Paths.get("birds.txt"))
    .filter(s -> s.length()> 2)
    .forEach(System.out::println);
```
The readAllLines() method returns a List, not a Stream, so the filter() method is not available.




<br/>
<br/>

## **Streams**

### **walk()**

```java
public static Stream<Path> walk(Path start, FileVisitOption... options) throws IOException
```
Return a Stream that is lazily populated with Path by walking the file tree rooted at a given starting file.

```java
public static Stream<Path> walk(Path start, int maxDepth, FileVisitOption... options) throws IOException
```
Return a Stream that is lazily populated with Path by walking the file tree rooted at a given starting file.

<br/>

### **find()**

The find() method behaves in a similar manner as the walk() method, except that it takes a BiPredicate to filter the data. It also requires a depth limit be set. Like walk(), find() also supports the FOLLOW_LINK option.

```java
public static Stream<Path> find(Path start, int maxDepth, BiPredicate<Path,BasicFileAttributes> matcher, FileVisitOption... options) throws IOException
```
Return a Stream that is lazily populated with Path by searching for files in a file tree rooted at a given starting file.


```java
Path path = Paths.get("/bigcats");
long minSize = 1_000;
try (var s = Files.find(path, 10, 
      (p, a) -> a.isRegularFile()
         && p.toString().endsWith(".java")
         && a.size() > minSize)) {
   s.forEach(System.out::println);
}
```





# OTHERS
## **RandomAccessFile**

*RandomAccessFile* implements *DataInput* as well as *DataOutput* interfaces. Therefore, in this case, you can use raf as an instance of *DataOutput* and call its *writeUTF(String)* method.

- writeUTF(String)
- writeChars(String )

<br/>

## BufferWriter

- newLine()
- write(String)

BufferedWriter does not have writeUTF method but it does have newLine and write(String) methods. 

You should remember that following points:
1.BufferedWriter only adds the functionality of buffering on top of a Writer. It doesn't directly deal with encoding. Encoding is handled by the underlying Writer object.
2.FileWriter is a concrete subclass of java.io.Writer that writes data to the underlying file in default encoding. If you want to write text in a different encoding, you will need to create an OutputStreamWriter with that encoding. For example,
OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("utf8.txt"), Charset.forName("UTF-8").newEncoder()  );
You can then create a BufferedWriter over this OutputStreamWriter.



## **Exceptions**

**java.io.FileNotFoundException** may be thrown by *FileInputStream*, *FileOutputStream*, and *RandomAccessFile* constructors if the file by the given name does not exist.


## **Files**
<br/>

### **move()**

**ATOMIC_MOVE:** Move the file as an atomic file system operation.
**COPY_ATTRIBUTES:** Copy attributes to the new file.
**REPLACE_EXISTING:** Replace an existing file if it exists.


<br/>

### **copy()**

```java
public static Path copy(Path source, Path target, CopyOption... options) throws IOException
```

**The options parameter may include any of the following:**  

**REPLACE_EXISTING:**  If the target file exists, then the target file is replaced if it is not a non-empty directory. If the target file exists and is a symbolic link, then the symbolic link itself, not the target of the link, is replaced.

**COPY_ATTRIBUTES:**  Attempts to copy the file attributes associated with this file to the target file. The exact file attributes that are copied is platform and file system dependent and therefore unspecified. Minimally, the last-modified-time is copied to the target file if supported by both the source and target file stores. Copying of file timestamps may result in precision loss.

**NOFOLLOW_LINKS:**  Symbolic links are not followed. If the file is a symbolic link, then the symbolic link itself, not the target of the link, is copied. It is implementation specific if file attributes can be copied to the new link. In other words, the COPY_ATTRIBUTES option may be ignored when copying a symbolic link.
An implementation of this interface may support additional implementation specific options.

Copying a file is not an atomic operation. If an IOException is thrown, then it is possible that the target file is incomplete or some of its file attributes have not been copied from the source file. When the REPLACE_EXISTING option is specified and the target file exists, then the target file is replaced. The check for the existence of the file and the creation of the new file may not be atomic with respect to other file system activities.
 
<br/>

```java
```

📺 ☝ 🟡
🔴