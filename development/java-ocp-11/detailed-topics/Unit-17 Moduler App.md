# Modular Applications

📺 **Common module directives**

<table border="1">
<thead>
<tr>
<td>Derivative</td>
<td>Description</td> </tr> </thead>
<tbody>
<tr>
<td>
<code>exports</code> <i>
<code>&lt;package&gt;</code></i></td>
<td>Allows all modules to access the package</td> </tr>
<tr>
<td>
<code>exports</code> <i>
<code>&lt;package&gt;</code></i> <code>to</code> <i>
<code>&lt;module&gt;</code></i></td>
<td>Allows a specific module to access the package</td> </tr>
<tr>
<td>
<code>requires</code> <i>
<code>&lt;module&gt;</code></i></td>
<td>Indicates module is dependent on another module</td> </tr>
<tr>
<td>
<code>requires transitive</code> <i>
<code>&lt;module&gt;</code></i></td>
<td>Indicates the module and that all modules that use this module are dependent on another module</td> </tr>
<tr>
<td>
<code>uses</code> <i>
<code>&lt;interface&gt;</code></i></td>
<td>Indicates that a module uses a service</td> </tr>
<tr>
<td>
<code>provides</code> <i>
<code>&lt;interface&gt;</code></i> <code>with</code> <i>
<code>&lt;class&gt;</code></i></td>
<td>Indicates that a module provides an implementation of a service</td> </tr> </tbody> </table>

<br/>

<br/>

# Types of Modules
## NAMED MODULES


A **named module** is one containing a module‐info file.

<br/>

## AUTOMATIC MODULES

An **automatic module** appears on the module path but does not contain a module‐info file

<br/>

## UNNAMED MODULES
An **unnamed module** appears on the classpath. Like an automatic module, it is a regular JAR.

<br/>

📺 **Properties of modules types**

|Property|Named|Automatic|Unnamed|
|--- |--- |--- |--- |
|A ______ module contains a module‐info file?|Yes|No|Ignored if present|
|A ______ module exports which packages to other modules?|Those in the module‐info file|All packages|No packages|
|A ______ module is readable by other modules on the module path?|Yes|Yes|No|
|A ______ module is readable by other JARs on the classpath?|Yes|Yes|Yes|


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