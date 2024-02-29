# [Method References]()
A **method reference** is a shorthand syntax that allows you to refer to methods or constructors without invoking them. It provides a way to treat methods as first-class citizens, similar to [lambda expressions](./lambda_expressions.md), and can be used in functional interfaces, such as ``java.util.function`` interfaces, where a single abstract method (SAM) is expected.

There are several types of method references:

### Reference to a Static Method
It refers to a static method of a class. This is denoted by ``ClassName::methodName``.

```java
// Example: Reference to the static method Integer.parseInt
Function<String, Integer> parser = Integer::parseInt;
```

### Reference to an Instance Method of a Specific Object
It refers to an instance method of a particular object. This is denoted by ``objectName::methodName``.

```java
// Example: Reference to the instance method toUpperCase of a String object
String str = "hello";
Function<String, String> converter = str::toUpperCase;
```

### Reference to an Instance Method of an Arbitrary Object of a Particular Type
: It refers to an instance method of an object of a particular type. This is denoted by ``ClassName::methodName``. This looks identical to the Reference to a Static Method but note that in this case the method is not ``static``.

```java
// Example: Reference to the instance method length of a String object
Function<String, Integer> lengthFinder = String::length;
```

### Reference to a Constructor
It refers to a constructor. This is denoted by ``ClassName::new``.

```java
// Example: Reference to the constructor of ArrayList
Supplier<ArrayList<String>> supplier = ArrayList::new;
```

Method references are often used in functional programming contexts, particularly when working with streams, where they provide a concise and readable way to specify behavior. They promote code reuse and can make your code more expressive and easier to understand by avoiding verbose lambda expressions. 