# Questions that don't really fit into the main categories

### Q. Can you explain the *transient* keyword and when you might make sure of it?<br>
According to the [Java Language Spec. as of writing Java-8](http://docs.oracle.com/javase/specs/jls/se8/html/jls-8.html#jls-8.3.1.3), variables may be marked transient to indicate that they are not part of the persistent state of an object.<br>But what's that mean?<br>Well, suppose you've some serializable object with say a password field. You don't really want that field to be serialized when writing the object to a stream - say a file. If you mark the password field as transient then when the object is deserialized, that password field will be null.<br>
```java
public class SomeUser implements Serializable {
  String username;
  transient String password; // => when read back from stream, this field will be null
  ...
  blah blah blah
  ...
}
```
<br>

### Q. Give 3 ways to initialise a *non-static, final* instance field, x.<br>
Inside the class body:<br>

```java
public class SomeUser {
  final int x = 11;  
}
```
<br>
Inside a constructor:<br>

```java
public class SomeUser {
  final int x;  
  public SomeUser() {
    x = 11;
  }
}
```
<br>
Inside one instance initializer:<br>

```java
public class SomeUser implements Serializable {
  final int x;
  {
    x = 11;
  }
}
```
<br>

### Q. Give 2 ways to initialise a *static, final* instance field, x.<br>
Inside the class body:<br>

```java
public class SomeUser {
  static final int x = 11;  
}
```
<br>
Inside one static initializer:<br>

```java
public class SomeUser implements Serializable {
  static final int x;
  
  static {
    x = 11;
  }
}
```
<br>

### Q. What's wrong with this?<br>

```java
public class final abstract SomeUser {
  ...
  blah blah blah
  ...
}
```
<br>
The [Java Language Spec](https://docs.oracle.com/javase/specs/jls/se8/html/jls-8.html#jls-8.1.1.2) states that a class can be declared final if its definition is complete and no subclasses are desired or required. However, marking a class as abstract implies that the class is incomplete and will results in a compile-time error. 




