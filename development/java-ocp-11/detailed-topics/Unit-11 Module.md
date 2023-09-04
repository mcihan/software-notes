# Module

## COMPILING MODULE
<br/>

🔴 **Note :** order matters when compiling a module. Suppose we list the module-info file first when trying to compile:

```java
 module zoo.animal.care {
    exports zoo.animal.care.medical;
    requires zoo.animal.feeding;
}
```
```java
javac -p mods
   -d care
   care/module-info.java
   care/zoo/animal/care/details/*.java
   care/zoo/animal/care/medical/*.java
```

The compiler complains that it doesn’t know anything about the package zoo.animal.care.medical.


<br/>

## EXPORT

<br/>

🔴 You are free to name your variable *exports*. 


```java
module zoo.animal.talks {
   exports zoo.animal.talks.content to zoo.staff;
   exports zoo.animal.talks.media;
   exports zoo.animal.talks.schedule;
 
   requires zoo.animal.feeding;
   requires zoo.animal.care;
}

```

<br/>

## REQUIRES TRANSITIVE


 *requires transitive moduleName*, means that any module that requires this module will also depend on *moduleName*.


<br/>

### Duplicate requires Statements


<br/>

**Example :** Does not compile !
```java
module bad.module {
   requires zoo.animal.talks;
   requires transitive zoo.animal.talks;
}
```

<br/>

## PROVIDES, USES, AND OPENS

<br/>

### Provide 

The **provides** keyword specifies that a class provides an implementation of a service.

```java
provides zoo.staff.ZooApi with zoo.staff.ZooImpl
```

<br/>

### Uses 

The *uses* keyword specifies that a module is relying on a service. 

```java
uses zoo.staff.ZooApi
```

<br/>

### Opens

Opens keyword allows to reflections.

```java
opens zoo.animal.talks.schedule;
opens zoo.animal.talks.media to zoo.staff;
```

<br/>

<br/>

## THE JAVA COMMAND

### Describing a Module

```java
module zoo.animal.feeding {
   exports zoo.animal.feeding;
}
```

```
java -p mods -d zoo.animal.feeding
 
java -p mods --describe-module zoo.animal.feeding
```

**Print**
```console
zoo.animal.feeding file:///absolutePath/mods/zoo.animal.feeding.jar
exports zoo.animal.feeding
requires java.base mandated
```

<br/>

<br/>

**Example 2:**
```java
module zoo.animal.care {
   exports zoo.animal.care.medical to zoo.staff;
   requires transitive zoo.animal.feeding;
}
```


```
java -p mods -d zoo.animal.care
```
**Print :**
```  
zoo.animal.care file:///absolutePath/mods/zoo.animal.care.jar
requires zoo.animal.feeding transitive
requires java.base mandated
qualified exports zoo.animal.care.medical to zoo.staff
contains zoo.animal.care.details
```

<br/>

### Listing Available Modules

<br/>

```
 java -p mods --list-modules
```
**Print :**
```
zoo.animal.care file:///absolutePath/mods/zoo.animal.care.jar
zoo.animal.feeding file:///absolutePath/mods/zoo.animal.feeding.jar
zoo.animal.talks file:///absolutePath/mods/zoo.animal.talks.jar
zoo.staff file:///absolutePath/mods/zoo.staff.jar
```

<br/>

### Showing Module Resolution

```java
 java --show-module-resolution
   -p feeding
   -m zoo.animal.feeding/zoo.animal.feeding.Task
```
**Print :**
```
root zoo.animal.feeding file:///absolutePath/feeding/
java.base binds java.desktop jrt:/java.desktop
java.base binds jdk.jartool jrt:/jdk.jartool
...
jdk.security.auth requires java.naming jrt:/java.naming
jdk.security.auth requires java.security.jgss jrt:/java.security.jgss
...
All fed!
```


## THE JAR COMMAND

Like the java command, the jar command can describe a module. Both of these commands are equivalent:

```
jar -f mods/zoo.animal.feeding.jar -d
jar --file mods/zoo.animal.feeding.jar --describe-module
```
**Print :**
```
zoo.animal.feeding jar:file:///absolutePath/mods/zoo.animal.feeding.jar /!module-info.class
exports zoo.animal.feeding
requires java.base mandated
```

## THE JDEPS COMMAND

### summary 
<br/>

**For Jar**
``` 
jdeps -s mods/zoo.animal.feeding.jar

jdeps -summary mods/zoo.animal.feeding.jar
``` 
**Print :**
``` 
zoo.animal.feeding -> java.base
``` 
<br/>

**For Module**
``` 
jdeps -s --module-path mods  mods/zoo.animal.care.jar

jdeps -summary --module-path mods  mods/zoo.animal.care.jar
``` 

<br/>


## THE JMOD COMMAND

 JMOD files are recommended only when you have native libraries or something that can’t go inside a JAR file

<br/>

|Operation|Description|
|--- |--- |
|create|Creates a JMOD file.|
|extract|Extracts all files from the JMOD. Works like unzipping.|
|describe|Prints the module details such as requires.|
|list|Lists all files in the JMOD file.|
|hash|Shows a long string that goes with the file|


<br/>

## Command-Line Options

<br/>

<table border="1">
<thead>
<tr>
<td><b>Description</b></td>
<td><b>Syntax</b></td>
</tr>
</thead>
<tbody>
<tr>
<td><span>Compile nonmodular code</span></td>
<td><p><b>javac -cp</b> <i>classpath</i> -d <i>directory classesToCompile</i></p> 
<p><b>javac --class-path</b> <i>classpath</i> -d <i>directory classesToCompile</i></p> 
<p><b>javac -classpath</b> <i>classpath</i> -d <i>directory classesToCompile</i></p></td>
</tr>
<tr>
<td><span>Run nonmodular code</span></td>
<td><p><b>java -cp</b> <i>classpath package</i>.<i>className</i></p> 
<p><b>java -classpath</b> <i>classpath package</i>.<i>className</i></p> 
<p><b>java --class-path</b> <i>classpath package</i>.<i>className</i></p></td>
</tr>
<tr>
<td><span>Compile a module</span></td>
<td><p><b>javac -p</b> <i>moduleFolderName</i> -d <i>directory classesToCompileIncludingModuleInfo</i></p> 
<p><b>javac --module-path</b> <i>moduleFolderName</i> -d directory <i>classesToCompileIncludingModuleInfo</i></p></td>
</tr>
<tr>
<td><span>Run a module</span></td>
<td><p><b>java -p</b> <i>moduleFolderName</i> <b>-m</b> <i>moduleName/package.className</i></p> 
<p><b>java --module-path</b> <i>moduleFolderName</i> <b>--module</b> <i> moduleName/package.className</i></p></td>
</tr>
<tr>
<td><span>Describe a module</span></td>
<td><p><b>java -p</b> <i>moduleFolderName</i> -<b>d</b> <i>moduleName</i></p> 
<p><b>java --module-path</b> <i>moduleFolderName</i> <b>--describe-module</b> <i>moduleName</i></p> 
<p><b>jar</b> --file <i>jarName</i> <b>--describe-module</b></p> 
<p><b>jar</b> -f <i>jarName</i> <b>-d</b></p></td>
</tr>
<tr>
<td><span>List available modules</span></td>
<td><p><b>java --module-path</b> <i>moduleFolderName</i> <b>--list-modules</b></p> 
<p><b>java -p</b> <i>moduleFolderName</i> <b>--list-modules</b></p> 
<p><b>java --list-modules</b></p></td>
</tr>
<tr>
<td><span>View dependencies</span></td>
<td><p><b>jdeps -summary --module-path</b> <i>moduleFolderName jarName</i></p> 
<p><b>jdeps -s --module-path</b> <i>moduleFolderName jarName</i></p></td>
</tr>
<tr>
<td><span>Show module resolution</span></td>
<td><p><b>java --show-module-resolution -p</b> <i>moduleFolderName</i> -<b>m</b> <i>moduleName</i></p> 
<p><b>java --show-module-resolution --module-path</b> <i>moduleFolderName</i> <b>--module</b> <i>moduleName</i></p></td>
</tr>
</tbody>
</table>


<br/>
<br/>

### **javac options**

<table border="1">
<thead>
<tr>
<td><span >Option</span></td>
<td><span >Description</span></td>
</tr>
</thead>
<tbody>
<tr>
<td><p><span class="codeLabel">-cp &lt;classpath&gt;</span></p> 
<p><span class="codeLabel">-classpath &lt;classpath&gt;</span></p> 
<p><span class="codeLabel">--class-path &lt;classpath&gt;</span></p></td>
<td><span >Location of JARs in a nonmodular program</span></td>
</tr>
<tr>
<td><span ><span class="codeLabel">-d &lt;dir&gt;</span></span></td>
<td><span >Directory to place generated class files</span></td>
</tr>
<tr>
<td><p><span class="codeLabel">-p &lt;path&gt;</span></p> 
<p><span class="codeLabel">--module-path &lt;path&gt;</span></p></td>
<td><span >Location of JARs in a modular program</span></td>
</tr>
</tbody>
</table>


<br/>
<br/>

### **java options**

<table border="1">
<thead>
<tr>
<td><span >Option</span></td>
<td><span >Description</span></td>
</tr>
</thead>
<tbody>
<tr>
<td><p><span class="codeLabel">-c</span></p> 
<p><span class="codeLabel">--create</span></p></td>
<td><span >Create a new JAR file</span></td>
</tr>
<tr>
<td><p><span class="codeLabel">-v</span></p> 
<p><span class="codeLabel">--verbose</span></p>
<p></p></td>
<td><span >Prints details when working with JAR files</span></td>
</tr>
<tr>
<td><p><span class="codeLabel">-f</span></p> 
<p><span class="codeLabel">--file</span></p></td>
<td><span >JAR filename</span></td>
</tr>
<tr>
<td><span ><span class="codeLabel">-C</span></span></td>
<td><span >Directory containing files to be used to create the JAR</span></td>
</tr>
<tr>
<td><p><span class="codeLabel">-d</span></p> 
<p><span class="codeLabel">--describe-module</span></p></td>
<td><span >Describes the details of a module</span></td>
</tr>
</tbody>
</table>
<br/>
<br/>

### **jdeps options**

<table border="1">
<thead>
<tr>
<td><span >Option</span></td>
<td><span >Description</span></td>
</tr>
</thead>
<tbody>
<tr>
<td><span ><span class="codeLabel">--module-path &lt;path&gt;</span></span></td>
<td><span >Location of JARs in a modular program</span></td>
</tr>
<tr>
<td><p><span class="codeLabel">-s</span></p> 
<p><span class="codeLabel">-summary</span></p></td>
<td><span >Summarizes output</span></td>
</tr>
</tbody>
</table>

<br/>

## Examples
Run Java Module
```css
java -p out\production -m FirstModuleInfo/com.cihan.HelloWorld
```
or 
```css
java -p . -m FirstModuleInfo
```


Create Runnable Jar From Module

```css
jar --create --file MyNewApp.jar --main-class com.cihan.HelloWorld
-C out\production\FirstModule\ .
```

List Jar

```css
jar -f MyNewApp.jar --list
```

```
META-INF/
META-INF/MANIFEST.MF
module-info.class
com/
com/cihan/
com/cihan/HelloWorld.class

```
## Descripe Module

### Descripe Module Alternative 1
```
jar -f MyNewApp.jar -d
```

```css
FirstModuleInfo jar:file:///C:/Users/z0049ymw/Documents/ModuleTest/MyNewApp.jar/!module-info.class
requires java.base mandated
contains com.cihan
main-class com.cihan.HelloWorld
```

### Descripe Module-Alternative 2

```css
java --module-path . --describe-module FirstModuleInfo
```
Or
```css
java -p . -d FirstModuleInfo
```
```java
FirstModuleInfo file:///C:/Users/z0049ymw/Documents/ModuleTest/./MyNewApp.jar
requires java.base mandated
contains com.cihan

```

## jdeps

```css
jdeps MyNewApp.jar
```

```java
FirstModuleInfo
 [file:///C:/Users/z0049ymw/Documents/ModuleTest/MyNewApp.jar]
   requires mandated java.base (@11.0.10)
FirstModuleInfo -> java.base
   com.cihan                                          -> java.io
  java.base
   com.cihan                                          -> java.lang
  java.base
```

<br/>
<br/>
