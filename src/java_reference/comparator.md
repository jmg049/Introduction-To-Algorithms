# [Comparator](https://docs.oracle.com/javase/10/docs/api/java/util/Comparator.html)
The ``Comparator`` interface is used to define a comparison method that can be used to order objects. It's often used in sorting collections of objects or when defining custom sorting orders for objects. The Comparator interface is part of the ``java.util`` package and it has a single method, ``compare()``, which compares two objects and returns an integer indicating their relative order.

```java
public interface Comparator<T> {
    int compare(T o1, T o2);
}
```
The ``compare()`` method returns a negative integer, zero, or a positive integer if the first argument is less than, equal to, or greater than the second, respectively.

One way of using ``Comparators`` is to create your own custom  ``Comparator`` class  like so:

```java
import java.util.*;

public class StringComparator implements Comparator<String> {
    @Override
    public int compare(String s1, String s2) {
        return s1.compareTo(s2);
    }

    public static void main(final String[] args) {
        final List<String> strings = Arrays.asList("banana", "apple", "orange", "grape");
        Collections.sort(strings, new StringComparator());

        System.out.println("Sorted strings: " + strings);
    }
}
```

However,  the same can be achieved much more elegantly with a [method reference](./method_reference.md):

```java
import java.util.*;

public class Main  {

    public static void main(final String[] args) {
        final List<String> strings = Arrays.asList("banana", "apple", "orange", "grape");
        Collections.sort(strings, Comparator.comparing(String::valueOf));
		// alternatively strings.sort(Comparator.comparing(String::valueOf));
        System.out.println("Sorted strings: " + strings);
    }
}
```

This should output:
```
Sorted strings: [apple, banana, grape, orange]
```

Now imaging we have a custom class, ``Person``
```java
public class Person {
    private String name;
    private int age;

	public Person(final String name, final int age) {
		this.name = name;
		this.age = age;
	}

	public String getName() {
		return this.name;
	}

	public int getAge() {
		return this.age;
	}
	
	@Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

And we want to compare instances of the ``Person`` class based on their ages.

```java
public class Main {
	public static void main(final String[] args
		final List<Person> people = new ArrayList
		people.add(new Person("Alice", 30);
		people.add(new Person("Bob", 25);
		people.add(new Person("Charlie", 35);
		
		Collections.sort(people, Compator.comparing(Person::getAge));
		System.out.println("Sorted people by age: " + people);
    }
}
```

We should get an output of:
```
Sorted people by age: [Person{name='Bob', age=25}, Person{name='Alice', age=30}, Person{name='Charlie', age=35}]
```
