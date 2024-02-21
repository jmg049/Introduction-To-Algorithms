# [The Comparable Interface](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html)
This is an interface which is used to indicate how objects of a class are to be compared against other objects. It is a very simple, but powerful interface.


```java
public interface Comparable<T> {
	public int compareTo(T o);
}
```

The interface contains a single method, ``compareTo()``, which compares the current object to another object. By implementing the ``Comparable`` interface objects of a class can be sorted a natural order.

The ``compareTo`` method should return a negative integer if ``this`` object is less than the specified object, zero if they are equal and a positive numbre if ``this`` object is greater than the specified object.

## Implementing Comparable

```java
public class Student implements Comparable<Student> {
    private String name;
    private int age;

    // Constructor, getters, and setters

    @Override
    public int compareTo(Student otherStudent) {
        // Compare based on age
      	// We can swap this to compared based on name
        return Integer.compare(this.age, otherStudent.age);
    }
}

```

## Using Comparable
```
import java.util.ArrayList;
import java.util.Collections;

public class Main {
    public static void main(String[] args) {
        ArrayList<Student> students = new ArrayList<>();
        students.add(new Student("Alice", 20));
        students.add(new Student("Bob", 18));
        students.add(new Student("Charlie", 22));

        // Sorting students based on their age
        Collections.sort(students);

        // Print sorted students
        for (Student student : students) {
            System.out.println(student.getName() + " : " + student.getAge());
        }
    }
}

```

The above example should print:

Bob : 18

Alice : 20

Charlie : 22