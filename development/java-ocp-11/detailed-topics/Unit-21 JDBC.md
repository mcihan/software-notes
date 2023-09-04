- **Driver:** Establishes a connection to the database
- **Connection:** Sends commands to a database
- **PreparedStatement:** Executes a SQL query
- **CallableStatement:** Executes commands stored in the database
- **ResultSet:** Reads results of a query

```java
 String url = "jdbc:derby:zoo";
try (Connection conn = DriverManager.getConnection(url);
    PreparedStatement ps = conn.prepareStatement(
    "SELECT name FROM animal");
    ResultSet rs = ps.executeQuery()) {
    while (rs.next())
    System.out.println(rs.getString(1));
}
 
```
**Url Examples :**
```java
jdbc:postgresql://localhost/zoo
jdbc:oracle:thin:@123.123.123.123:1521:zoo
jdbc:mysql://localhost:3306
jdbc:mysql://localhost:3306/zoo?profileSQL=true
```

```java
```

```java
```