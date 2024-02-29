# [Generics](https://docs.oracle.com/javase/tutorial/java/generics/types.html)
Generics in Java are a powerful feature that allows developers to write flexible and reusable code by creating classes, interfaces, and methods that operate on a range of data types. Essentially, generics enable the creation of parameterized types, allowing classes and methods to work with any data type specified at compile time. This enhances code flexibility, type safety, and code reusability by providing a way to write algorithms and data structures that can adapt to various data types without sacrificing type safety or resorting to casting.

There are many benefits to using generics in Java. Firstly, generics promote code reusability by enabling the creation of classes, methods, and interfaces that can operate on a variety of data types without duplicating code. This reduces the need for writing specialized implementations for each data type, leading to cleaner and more maintainable codebases. Additionally, generics enhance type safety by allowing the compiler to perform type checking at compile time, preventing runtime errors related to type mismatches. By leveraging generics, developers can write more robust and error-resistant code, leading to fewer bugs and easier debugging processes. Overall, generics play a crucial role in Java programming by facilitating code flexibility, reusability, and type safety, ultimately contributing to the development of efficient and maintainable software systems.

## Syntax
Generic syntax involves declaring classes, interfaces, and methods with one or more type parameters, denoted by angle brackets < >. These type parameters represent placeholders for actual data types that will be specified when instances of the generic class or interface are created or when methods are invoked. Let's explore the syntax with some examples:

```java
public class Box<T> {
    private T value;

    public void setValue(T value) {
        this.value = value;
    }

    public T getValue() {
        return value;
    }
}
```

In this example, ``Box`` is a generic class with a type parameter ``T``. This class can hold any type of object. The type parameter ``T`` is used to specify the type of the value stored in the box. When creating instances of ``Box``, you specify the actual type:

```java
Box<Integer> intBox = new Box<>();
intBox.setValue(10);
System.out.println("Value in the integer box: " + intBox.getValue()); // Output: 10

Box<String> stringBox = new Box<>();
stringBox.setValue("Hello");
System.out.println("Value in the string box: " + stringBox.getValue()); // Output: Hello
```
	

``` java
public interface Pair<K, V> {
    K getKey();
    V getValue();
}

```

In this example, ``Pair`` is a generic interface with two type parameters ``K`` and ``V``, representing the key-value pair. Implementing classes specify the types for ``K`` and ``V``:

```java
public class OrderedPair<K, V> implements Pair<K, V> {
    private K key;
    private V value;

    public OrderedPair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    @Override
    public K getKey() {
        return key;
    }

    @Override
    public V getValue() {
        return value;
    }
}
```
You can then create instances of ``OrderedPair`` with specific types for ``K`` and ``V``:
```java
OrderedPair<String, Integer> pair1 = new OrderedPair<>("One", 1);
System.out.println("Key: " + pair1.getKey() + ", Value: " + pair1.getValue()); // Output: Key: One, Value: 1

OrderedPair<Integer, String> pair2 = new OrderedPair<>(2, "Two");
System.out.println("Key: " + pair2.getKey() + ", Value: " + pair2.getValue()); // Output: Key: 2, Value: Two
```

### Wildcard Operator ``?``
he wildcard operator, denoted by ?, is used to represent an unknown type. It allows for greater flexibility when working with generic types, particularly in scenarios where the exact type is not important or needs to be left unspecified. The wildcard operator comes in two forms: the upper bounded wildcard ``<? extends T>`` and the lower bounded wildcard ``<? super T>``.

#### Upper Bounded Wildcard ``(<? extends T>``)
This wildcard allows any type that is a subtype of T or T itself. It is used when you want to work with objects of a certain type or its subtypes. For example:
```java
public static double sumOfList(List<? extends Number> list) {
    double sum = 0.0;
    for (Number num : list) {
        sum += num.doubleValue();
    }
    return sum;
}
```
Here,`` List<? extends Number>`` denotes a list of elements that are of type Number or its subtypes. This method can accept a ``List<Integer>``, ``List<Double>``, or any other list of a subtype of Number.

#### Lower Bounded Wildcard (``<? super T>``):
This wildcard allows any type that is a supertype of T. It is used when you want to work with objects of a certain type or its superclasses. For example:
```java
public static void addNumbers(List<? super Integer> list) {
    for (int i = 1; i <= 5; i++) {
        list.add(i);
    }
}
```


Here, ``List<? super Integer>`` denotes a list that can accept elements of type Integer or any superclass of Integer. This method can accept a ``List<Number>``, ``List<Object>``, or any other list that is a supertype of Integer.

Wildcard types are useful for writing more flexible and generic code, especially when dealing with collections or methods that can operate on various types. They provide a means to design methods and classes that can handle a wide range of types without sacrificing type safety. However, it's important to use wildcards judiciously to ensure that your code remains clear and understandable.

## Bounded Type Parameters
The above explanations of the bounded wildcard operator also apply to our genric types, e.g. ``T``. 

Consider the following upper bounded type example:
```java
public class Box<T extends Number> {
    private T value;

    public Box(T value) {
        this.value = value;
    }

    public T getValue() {
        return value;
    }

    // Other methods...
}
```

In this example,``T`` is an upper bounded type parameter constrained to be a subclass of ``Number`` or ``Number`` itself. This means you can create a ``Box`` of type ``Integer``, ``Double``, ``Float``, etc., but not of any other type.

Consider the following lower bounded type example:

```java
public static <T super Integer> void addToCollection(List<T> list, T element) {
    list.add(element);
}
```
In this example, the type parameter ``T`` is bounded by ``Integer`` or any superclass of ``Integer``. This allows you to add elements of type ``Integer``, ``Number``, or``Object`` to the list.

## Type Erasure
Type erasure is a fundamental concept in Java generics where the type parameters specified in generic types and methods are erased (removed) during compilation. This means that the compiler replaces all occurrences of the generic type parameters with their upper bound or the closest applicable type, typically Object if no bound is specified. The resulting bytecode does not contain any information about the generic types used in the code, making it compatible with the pre-generics JVM.

The main reason for type erasure in Java generics is to maintain compatibility with existing codebases while introducing generics as a feature. Since Java was designed to be backward compatible, it was crucial to ensure that code written without generics would continue to work seamlessly with code written using generics.

While type erasure enables this compatibility, it also has implications for runtime behavior and type safety:

#### Runtime Behaviour
Since type information is erased at compile time, the JVM does not have access to generic type parameters at runtime. As a result, generic types are effectively treated as their raw types at runtime. For example, a ``List<String>`` is treated as a raw List type at runtime. This means that you cannot perform runtime checks or operations that rely on the actual generic type parameters.

#### Type Safety
Type erasure can also affect type safety in certain scenarios. Because the compiler replaces generic type parameters with their upper bounds or ``Object``, it may not be possible to enforce type constraints at compile time. This can lead to unchecked warnings or potential type mismatches at runtime if the code makes incorrect assumptions about the types involved. For example, consider the following code:

```java
List<Integer> list = new ArrayList<>();
list.add("String"); // Error: Compilation error
```

In this case, the compiler detects the type mismatch and generates a compilation error because it knows that the list is supposed to contain integers. However, with type erasure, the code becomes:

```java
List list = new ArrayList();
list.add("String"); // No compilation error
```

Now, there is no compilation error because the compiler treats List as a raw type due to type erasure. This can lead to runtime errors if the code tries to retrieve elements from the list assuming they are integers.

To mitigate the impact of type erasure on type safety, it's essential to use generics judiciously and follow best practices such as avoiding raw types, using bounded wildcard types when appropriate, and performing explicit type checks and casts when necessary. Additionally, understanding how type erasure works can help developers write more robust and reliable code when working with generics in Java.

## Pitfalls and Best Practices
When working with generics in Java, there are several common pitfalls that developers may encounter. These pitfalls can lead to runtime errors, unchecked warnings, or reduced type safety if not addressed properly. Some of the most common pitfalls include the use of raw types, unchecked warnings, and casting issues. Let's explore each of these pitfalls and how to avoid them:

#### Raw Types
Raw types refer to the use of generic types without specifying the type parameter. For example:

```java
List list = new ArrayList(); // Raw type
```

Using raw types bypasses the type checking provided by generics, which can lead to runtime errors or unexpected behavior. To avoid raw types, always specify the type parameter when declaring generic types:

```java
List<String> list = new ArrayList<>();
```

#### Unchecked Warnings
Unchecked warnings occur when the compiler cannot ensure type safety due to the use of raw types or unchecked conversions. For example:

```java
List<String> list = new ArrayList();
```

This can generate an unchecked warning because ``ArrayList`` is a raw type. To address unchecked warnings, ensure that you specify the type parameter correctly or suppress the warning using ``@SuppressWarnings("unchecked")`` with caution:


```java
List<String> list = new ArrayList<>();
```

#### Casting Issues
Casting issues may arise when working with generics, especially when dealing with raw types or using wildcard types improperly. For example:

```java
List<String> strings = new ArrayList<>();
List<Object> objects = (List<Object>) strings; // Unsafe cast
```

This can lead to ``ClassCastException`` at runtime. To avoid casting issues, use bounded wildcard types or rethink your design to ensure type safety:

```java
List<? extends Object> objects = strings; // Safe
```

#### Mutability of Collections with Wildcards
When using wildcard types in collections, be cautious about mutability. For example:

```java
List<? extends Number> numbers = new ArrayList<>();
numbers.add(10); // Error: Cannot add to wildcard collection
```

To avoid this, consider using bounded wildcard types for read-only access and unbounded types for mutability, or use helper methods to modify collections safely.

#### Erasure and Type Safety
Java's type erasure means that generic type information is removed at runtime, which can lead to issues when working with generic types that rely on runtime type information. Be aware of the limitations of type erasure and design your generics with type safety in mind.









