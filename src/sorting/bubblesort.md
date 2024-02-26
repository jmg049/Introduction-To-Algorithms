# [Bubble Sort](https://en.wikipedia.org/wiki/Bubble_sort)
Bubble sort is a simple sorting algorithm that repeatedly steps through the list, compares adjacent elements, and swaps them if they are in the wrong order. The pass through the list is repeated until the list is sorted. At each step, the largest unsorted element "bubbles up" to its correct position, hence the name "bubble sort." While conceptually straightforward, bubble sort is not efficient for large lists, as its average and worst-case time complexity are both O(n^2), making it impractical for real-world applications compared to more efficient sorting algorithms like quicksort or mergesort.

## Implementation

###  Pseudocode


```pseudocode
bubbleSort(A)
    n = length(A)
    for i from 0 to n-1 do
        for j from 0 to n-i-1 do
            if A[j] > A[j+1] then
                swap A[j] and A[j+1]
```

### Java 
Using the pseudocode we can implement Bubble Sort in Java. We will need a helper method to swap some array/list elements too. 

```java
import java.util.Comparator;
import java.util.Comparable;
import java.util.List;

public static <T extends Comparable<? super T>> void bubbleSort(final List<T> list, final Comparator<T> comparator) {
    T temp;
    for (int i = 0; i < list.size(); i++) {
        for (int j = 1; j < list.size(); j++) {
            if (comparator.compare(list.get(j), list.get(j - 1)) < 0) {
                swap(list, j, j -1);
            }
        }
    }
}

public static <T extends Comparable<? super T>> void bubbleSort(final T[] array, final Comparator<T> comparator) {
    T temp;
    for (int i = 0; i < array.length; i++) {
        for (int j = 1; j < array.length; j++) {
            if (comparator.compare(array[j], array[j - 1]) < 0) {
                swap(array, j, j - 1);
            }
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


We can see that the Java code closely follows the pseudocode. We loop over each element, and then for each element we loop over the remaining elements and swap them if required. 

We can test our implementation using the following code.:

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

    bubbleSort(ints, Comparator.comparing(Integer::intValue));
    System.out.println("List sorted: " + sorted(intsArr, Comparator.comparing(Integer::intValue)) + " " + ints);
    bubbleSort(intsArr, Comparator.comparing(Integer::intValue));
    System.out.println("Array sorted: " + sorted(intsArr, Comparator.comparing(Integer::intValue)) + " " + Arrays.toString(intsArr));
}
```

## Complexiity
The time complexity of the bubble sort algorithm is ``O(n^2)``. This arises from the nested loops used to traverse the list and compare elements. In the worst-case scenario, where the input list is in reverse sorted order, bubble sort will perform n iterations for the outer loop, and for each iteration, it will perform ``n - 1`` comparisons in the inner loop. This results in a total of approximately ``n^2/2 ``comparisons and swaps. Similarly, in the average case, bubble sort still requires ``O(n^2)`` comparisons and swaps. While bubble sort is simple to implement and understand, its inefficiency for large datasets makes it impractical for real-world applications.