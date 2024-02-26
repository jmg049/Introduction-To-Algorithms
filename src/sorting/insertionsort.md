# [Insertion Sort](https://en.wikipedia.org/wiki/Insertion_sort)

Insertion sort is a simple sorting algorithm that builds the final sorted list one element at a time by repeatedly taking the next unsorted element and inserting it into its correct position in the already sorted part of the list. Initially, the first element of the list is considered as the sorted part, and the rest are unsorted. Then, the algorithm iterates over the unsorted elements, comparing each element to the elements in the sorted sublist and shifting larger elements one position to the right to make space for the new element. This process continues until all elements are sorted. Insertion sort has a time complexity of ``O(n^2)``, making it inefficient for large datasets, but its simplicity and efficiency for small datasets or nearly sorted lists make it a practical choice in some scenarios, such as for small input sizes or as a building block in more complex algorithms.

## Implementation

### pseudocode

```pseudocode
insertionSort(A:)
    n = length(A)
    for i from 1 to n-1
        key = A[i]
        j = i - 1
        while j >= 0 and A[j] > key
            A[j + 1] = A[j]
            j = j - 1
        A[j + 1] = key
```

### Java
We can now  translate the pseudocode to actual Java.

```java
import java.util.Comparator;
import java.util.Comparable;
import java.util.List;

public static <T extends Comparable<? super T>> void insertionSort(final List<T> list, final Comparator<T> comparator) {
    for (int i = 0; i < list.size(); i++) {
        final T item = list.get(i);
        int j = i - 1;
        while (j >= 0 && comparator.compare(list.get(j), item) > 0) {
            swap(list, j + 1, j);
            j = j - 1;
        }
    }
}

public static <T extends Comparable<? super T>> void insertionSort(final T[] array, final Comparator<T> comparator) {
    for (int i = 0; i < array.length; i++) {
        final T item = array[i];
        int j = i - 1;
        while (j >= 0 && comparator.compare(array[j], item) > 0) {
            swap(array, j + 1, j);
            j = j - 1;
        }
    }
}

public static <T> void swap(final List<T> list, final int firstIndex, final int secondIndex) {
    final T tmp = list.get(firstIndex);
    list.set(firstIndex, list.get(secondIndex));
    list.set(secondIndex, tmp);
}

public static <T> void swap(final T[] array, final int firstIndex, final int secondIndex) {
    final T tmp = array[firstIndex];
    array[firstIndex] = array[secondIndex];
    array[secondIndex] = tmp;
}

```

We can then test our code with:

```java
import java.util.Arrays;
import java.util.List;
import java.util.Collections;
import java.util.Comparator;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public static void main(final String[] args) {
    final List<Integer> ints = IntStream.range(0, 100).boxed().collect(Collectors.toList());
    Collections.shuffle(ints);
    final Integer[] intsArr = ints.toArray(new Integer[0]);
    shuffle(intsArr); 

    System.out.println("List sorted: " + sorted(intsArr, Comparator.comparing(Integer::intValue)) + " " + ints);
    System.out.println("Array sorted: " + sorted(intsArr, Comparator.comparing(Integer::intValue)) + " " + Arrays.toString(intsArr));

    selectionSort(ints, Comparator.comparing(Integer::intValue));
    System.out.println("List sorted: " + sorted(intsArr, Comparator.comparing(Integer::intValue)) + " " + ints);
    selectionSort(intsArr, Comparator.comparing(Integer::intValue));
    System.out.println("Array sorted: " + sorted(intsArr, Comparator.comparing(Integer::intValue)) + " " + Arrays.toString(intsArr));
}

public static <T> void shuffle(final T[] data) {
    for (int i = data.length - 1; i >= 1; i--) {
        int j = (int) (Math.random() * data.length);
        swap(data, i, j);
    }
}

```


## Complexity
The time complexity of the insertion sort algorithm is ``O(n^2)`` in the worst-case scenario. This arises because, in the worst-case scenario, each element must be compared and possibly moved to its correct position within the sorted sublist, resulting in quadratic time complexity. Specifically, for each element in the unsorted sublist, insertion sort may need to perform up to ``O(i)`` comparisons and swaps, where ``i`` is the position of the element in the sorted sublist. When summed over all elements in the list, this results in a time complexity of ``O(n^2)``. However, in the best-case scenario, where the input list is already sorted, the time complexity reduces to ``O(n)``, as the algorithm only needs to iterate through the list once to confirm that each element is already in its correct position. Despite its quadratic time complexity, **insertion sort can be efficient** for small datasets or nearly sorted lists, making it a practical choice in certain scenarios.