# [Keywords](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/_keywords.html)

## final
The ``final`` keyword is used to apply restrictions on classes, methods and variables, and will function differently depending on which one it is used with.

### Final Classes
Final classes e.g. ``final class MyClass`` cannot be inherited from, i.e. you cannot create a subclass of a final class. 

```java
final class FinalClass {
    // Class implementation
}

// Attempting to subclass a final class will 
// result in a compilation error
// class SubClass extends FinalClass {
//     // Class implementation
// }

```

### Final Methods
When ``final`` is used for a method it indicates that the method cannot be overriden by subclasses. In essence, it locks the behaviour of the method, ensuring no subclass can change its behaviour.

```java
class Parent {
    final void display() {
        System.out.println("Parent's display method");
    }
}

class Child extends Parent {
    // This will result in a compilation error
    // Attempting to override a final method
    // void display() {
    //     System.out.println("Child's display method");
    // }
}
```

### Final Variables
When applied to variables, the final keyword ensures that the value of the variable cannot be changed after it has been initialized. Once assigned, the value remains constant throughout the program execution. This is useful when you want to define constants or ensure immutability.

```java
final int MAX_VALUE = 100;
final double PI = 3.14;
final String CONST_TITLE = "My Title";

```

{#final-note}**Note:** There is a misconception that because a variable is marked ``final`` that it cannot be mutated/changed at all. This is incorrect, as ``final`` indicates that the reference cannot be modified, i.e. we cannot reassign a ``final`` variable. For example:

```java
final double PI = 3.14;
public static void main(final String[] args) {
	// Below will produce a compilation error
	PI = 3; // Cannot assign a value to final variable 'PI';
}
```

We can however modify the content if the final variable is the some non-primitive(int, float, double, char, ...) object. 

```
final List<Double> list = new ArrayList<>();

public static void main(final String[] args) {
	// we can add to the list
	list.add(2.5);
	list.add(3.14);
	
	// But we change the reference, i.e. reassign
	// Below will produce a compilation error
	list = new ArrayList<>(); // Cannot assign a value to final variable 'list';
}
```

### Final Parameters
When applied to method parameters, the final keyword ensures that the parameter cannot be modified within the method body. However, the same [note](#final-note) still applies. 

```java
void process(final int x) {
    // This will result in a compilation error
    // Attempting to modify a final parameter
    // x = 10;
    System.out.println("Value of x: " + x);
}
```
