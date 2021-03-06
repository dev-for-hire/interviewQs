# Questions that don't really fit into the main categories

### Q. Can you explain the paradigm "convention over configuration" and give a few examples were you may have used it?<br>
<br><br>

### Q. True or False. All classes have at least one constructor?<br>
True! If you do not explicitly declare a constructor in your class then the compiler will *automagically* generate a no-argument constructor, it's called the default constructor.<br>Be warned though, if you declare any constructor, with no-args or args, then the compiler will not autogenerate a default constructor.
<br><br>

### Q. What is an inner class?<br>
An inner class is a non-static nested class.<br> Take a look at [Java Tutorial](https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html) where it states that "Nested classes are divided into two categories: static and non-static. Nested classes that are declared static are called static nested classes. Non-static nested classes are called inner classes."
<br><br>

### Q. What's the syntax for creating an object for the static nested class *StaticNestedClass* that's nested inside *OuterClass*?<br>

```java

OuterClass.StaticNestedClass nestedObject = new OuterClass.StaticNestedClass();
     
```
<br><br>

### Q. What's the syntax for creating an object for the inner class *InnerClass* that's nested inside *OuterClass*?<br>
This is a bit more clunky. Remember, to instantiate the inner class you need an instance of the *OuterClass* first.<br>

```java
OuterClass outerObject = instantiateOuterClassSomehowFromSomeWhere();
...
OuterClass.InnerClass innerObject = outerObject.new InnerClass();
     
```
<br><br>

### Q. What happens when you return a value from a method that is declared void?<br>
You'll get a compiler error as you would if you not return a value from a method expecting a return type, albeit a different error message.<br><br>


### Q. How are primitive and reference types passed as method parameters?<br>
They are both passed by *value*. In particular for references, this means that when the method returns, the passed-in reference still references the same object as before. However, the reference's state can be changed inside the method and the affect can be seen after the method call - assuming the reference has mutator methods etc. (or perhaps because of unforeseen side-effects :-)<br><br>


### Q. What's the *implicit* parameter in a non-static method?<br>
*this* - the reference to the object the method was called on.<br><br>

### Q. Can you explain the *transient* keyword and when you might make use of it?(a PCQ)<br>
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
<br><br>

### Q. Give 3 ways to initialise a *non-static, final* instance field, x. (a PCQ)<br>
Inside the class declaration like:<br>

```java
public class SomeUser {
  final int x = 11;  
}
```
<br>
Inside a constructor as in:<br>

```java
public class SomeUser {
  final int x;  
  public SomeUser() {
    x = 11;
  }
}
```
<br>
Inside one instance initializer like:<br>

```java
public class SomeUser {
  final int x;
  {
    x = 11;
  }
}
```
<br>
There's actually a fourth way, well kinda. You could create a final method that initialises the field like this:<br>

```java
public class SomeUser {
  final int x = initializeX();
  
  protected final int initializeX() {
    x = 11;
  }
}
```

<br>But this is still kinda doing the init inside the class declaration.<br><br>

### Q. Give 2 ways to initialise a *static, final* class field, x. (a PCQ)<br>
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
<br><br>

### Q. A *final* variable can't be reassigned once initialised right? However, there is one exception to this rule? (a PCQ)
<br>

```java
System.in
System.out
System.err
```
<br>
System uses the *setOut()* method which is a native method, not implemented in the Java programming language. These native methods can bypass the normal access control mechanisms.
<br><br>

### Q. What's wrong with this class definition? (a PCQ)<br>

```java
public class final abstract SomeUser {
  ...
  blah blah blah
  ...
}
```

<br>
The Java Language [Spec](http://docs.oracle.com/javase/specs/jls/se8/html/jls-8.html#jls-8.1.1.2) states that a class can be declared final if its definition is complete and no subclasses are desired or required. However, marking a class as abstract implies that the class is incomplete and will results in a compile-time error. 

<br><br>

### Q. Why is it a compile-time error for a generic class to be a direct or indirect subclass of Throwable? (a PCQ)
This restriction is needed since the *catch mechanism* of the JVM works only with non-generic classes.

<br><br>

### Q. What you reckon will be printed out when this program is executed?<br>

```java
public class App {

    public static void main(String[] args) {

        Counter counter = new Counter();

        for (int i = 0; i < 100; i++) {
            counter.increment();
        }

        increment(counter);

        System.out.print(String.format("Counter is now = %s", counter.getCount()));
    }

    public static Counter increment(Counter counter) {

        counter = new Counter();

        for (int i = 0; i < 100; i++) {
            counter.decrement();
        }

        return counter;
    }
}

class Counter {
    private int count = 0;

    public int getCount() { return count;}

    public void increment() { count += 1; }

    public void decrement() { count -= 1; }

}

```
<br>
Here's what's printer out:<br>

```java
Counter is now = 100
```
<br>
Why? <br>Remember that Java does not have pass by reference semantics. Sure you can pass an object into a method and change the state of the object and even see the changes when the scope of the method is gone. However, you can't change the memory location that the object points to - it's stuck there. So even though the static increment() method attempts to change the location of the Counter object passed in as parameter, once the method is finish the original Counter object will still point to the old object and it's state will not have been changed.


