# [Selection Sort](https://en.wikipedia.org/wiki/Selection_sort)
Selection sort is a straightforward sorting algorithm that divides the input list into two parts: a sorted sublist and an unsorted sublist. Initially, the sorted sublist is empty, while the unsorted sublist contains all the elements of the input list. The algorithm iterates through the unsorted sublist, finding the smallest (or largest, depending on the sorting order) element and swapping it with the leftmost unsorted element, effectively expanding the sorted sublist. This process continues until the entire list is sorted. Selection sort has a time complexity of ``O(n^2)``, making it inefficient for large datasets.


## Implementation

### Pseudocode

```pseudocode
selectionSort(A)
    n = length(A)
    for i from 0 to n-1
        min_index = i
        for j from i+1 to n-1
            if A[j] < A[min_index]
                min_index = j
        swap A[i] and A[min_index]
```

Note that in this pseudocode there isn't an explicit sorted and unsorted input. We will inlcude them explicity though in the real code for demonstration purposes.

```java
import java.util.List;
import java.util.ArrayList;
import java.utll.Comparator;
import java.util.Comparable;

public static <T extends Comparable<? super T>> void selectionSort(final List<T> list, final Comparator<T> comparator) {
    List<T> result = new ArrayList<T>();
    T temp = list.get(0);
    int index = 0;
    while (result.size() < list.size()) {
        for (int i = 0; i < list.size(); i++) {
            if (list.get(i) != null && comparator.compare(list.get(i), temp) < 0) {
                temp = list.get(i);
                index = i;
            }
        }
        result.add(temp);
        list.set(index, null);
    }
    for (int i = 0; i < list.size(); i++) {
        list.set(i, result.get(i));
    }
}

public static <T extends Comparable<? super T>> void selectionSort(final T[] array, final Comparator<T> comparator) {
    List<T> result = new ArrayList<T>(); // this could also be an array of size array.length
    T temp = array[0];
    int index = 0;
    while (result.size() < array.length) {
        for (int i = 0; i < array.length; i++) {
            if (array[i] != null && comparator.compare(array[i], temp) < 0) {
                temp = array[i];
                index = i;
            }
        }
        result.add(temp);
        array[index] = null;
    }
    for (int i = 0; i < array.length; i++) {
        array[i] = result.get(i);
    }
}
``` 

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

public static <T> void swap(final T[] data, final int i, final int j) {
    T tmp = data[i];
    data[i] = data[j];
    data[j] = tmp;
}
```

## Complexity
Selection sort has an interesting complexity. While it is ``O(n^2)`` (fairly obvious from the nested loops), how we arrive at that complexity isn't quite as straightforward.

The time complexity of selection sort comes from [``arithmetic progression``](https://en.wikipedia.org/wiki/Arithmetic_progression) formula. Each iteration of the loop does an inner loop from ``i+1`` to the length of the array. Another way of looking at this  is that at each iteration there is ``(n-1) - i`` inner iterations occuring. An expression for the total number of iterations can be given by ``(n-1)  + (n - 2) + ... + 1``. Thankfully someone has worked this out for us:

\\[ 
	(n-1) + (n-2) + ... + 1 = \sum_{i=1}^{n-1} i
\\]

By arithmetic progression:

\\[
	 \sum_{i=1}^{n-1} i= \frac{(n-1) + 1}{2} (n-1) = \frac{1}{2} n (n-1) = \frac{1}{2}(n^2 - n)
\\]

Then since we only care about the highest power term we can drop the ``n``  and the constant ``1/2``.

A downside to our implementation is the auxiliary space complexity (the additional space in memory needed to run our algorithm). Since we maintain an additonal list/array, we require an extra ``O(n)`` space to store it. 