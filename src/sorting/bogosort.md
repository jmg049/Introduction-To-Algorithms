# [Bogo Sort](https://en.wikipedia.org/wiki/Bogosort)
Bogo sort is a terrible sorting algorithm. It randomly shuffles an the input data and then checks to see if it sorted or not. While having no real practical benefit, it does have a fun time complexity. 

## Implementation

### Pseudocode
```pseudocode 
bogoSort(data)
    while not sorted(data)
        shuffle(data)
```

Nice and short!

### Java Implementation
Thankfully, the Java implementation isn't much longer and gives us an opportunity to use a useful method from the standard library. We will also need a utility method for checking if out input iis sorted and for [shuffling](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle) an array. [``Collections.shuffle``](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#shuffle-java.util.List-) only works for ``Lists``. 

```java
import java.util.List;
import java.util.Collections;
import java.util.Comparator;

public static <T extends Comparable<? super T>> void bogoSort(final T[] data, final Comparator<T> comparator) {
    while (!sorted(data, comparator)) {
        shuffle(data);
    }
}

public static <T extends Comparable<? super T>> void bogoSort(final List<T> data, final Comparator<T> comparator) {
    while (!sorted(data, comparator)) {
        Collections.shuffle(data);
    }
}

public static <T extends Comparable<? super T>> boolean sorted(final T[] data, final Comparator<T> comparator) {
    for (int i = 0; i < data.length - 1; i++) {
        if (comparator.compare(data[i], data[i + 1]) > 0) {
            return false;
        }
    }
    return true;
}

public static <T extends Comparable<? super T>> boolean sorted(final List<T> data, final Comparator<T> comparator) {
    for (int i = 0; i < data.size() - 1; i++) {
        if (comparator.compare(data.get(i), data.get(i + 1)) > 0) {
            return false;
        }
    }
    return true;
}

public static <T> void shuffle(final T[] data) {
    for (int i = data.length - 1; i >= 1; i--) {
        int j = (int) (Math.random() * data.length);
        swap(data, i, j);
    }
}

public static <T> void swap(final T[] data, final int i, final int j) {
    T tmp = data[i];
    data[i] = data[j];
    data[j] = tmp;
}
```

To test out our implementation we can run the following:
```java
import java.util.Arrays;
import java.util.List;
import java.util.Collections;
import java.util.Comparator;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public static void main(final String[] args) {
    final List<Integer> ints = IntStream.range(0, 5).boxed().collect(Collectors.toList());
    Collections.shuffle(ints);
    final Integer[] intsArr = ints.toArray(new Integer[0]);
    shuffle(intsArr);

    System.out.println("List sorted: " + sorted(intsArr, Comparator.comparing(Integer::intValue)) + " " + ints);
    System.out.println("Array sorted: " + sorted(intsArr, Comparator.comparing(Integer::intValue)) + " " + Arrays.toString(intsArr));

    bogoSort(ints, Comparator.comparing(Integer::intValue));
    System.out.println("List sorted: " + sorted(intsArr, Comparator.comparing(Integer::intValue)) + " " + ints);
    bogoSort(intsArr, Comparator.comparing(Integer::intValue));
    System.out.println("Array sorted: " + sorted(intsArr, Comparator.comparing(Integer::intValue)) + " " + Arrays.toString(intsArr));
}
```

## Complexity
There are two aspects which feed into the time complexity of Bogo Sort;

1. The shuffle
2. Checking if it is sorted. 

Given an input of length ``n`` there are ``n!`` possible permutations of the input. The liklihood of randomly shuffle to the sorted input is ``1/n!``. This means that we will likely have to shuffle the input ``n!`` times before we get the sorted version. 

Then each iteration we have to check if the input is sorted. This takes at most ``n`` iterations per permutation.

Combining the two we get a final complexity of ``O(n * n!)`` or ``O((n+1)!)``
