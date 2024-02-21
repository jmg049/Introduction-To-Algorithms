# [Quick Sort](https://en.wikipedia.org/wiki/Quicksort)
Quicksort is a highly efficient sorting algorithm based on the divide-and-conquer paradigm. It works by selecting a pivot element from the array and partitioning the array into two sub-arrays, one with elements smaller than the pivot and the other with elements greater than the pivot. This process is recursively applied to the sub-arrays until the entire array is sorted. With an average-case time complexity of ``O(n log n)``, quicksort is widely used for sorting large datasets due to its speed and simplicity. 

However, in the worst-case scenario, its time complexity can degrade to ``O(n^2)`` if poorly chosen pivot elements consistently create unbalanced partitions. To address this, various strategies like median-of-three pivot selection or randomized pivot selection have been proposed, ensuring quicksort's resilience and effectiveness as a fundamental sorting algorithm in computer science.


## Implementation

### Pseudocode

```pseudocode
quicksort(array, low, high)
    if low < high
        pivotIndex = partition(array, low, high)
        quicksort(array, low, pivotIndex - 1)
        quicksort(array, pivotIndex + 1, high)

partition(array, low, high)
    pivot = array[high]
    i = low - 1
    for j = low to high - 1
        if array[j] < pivot
            i = i + 1
            swap(array[i], array[j])
    swap(array[i + 1], array[high])
    return i + 1

```

There are two functions in the pseudocode above: ``quicksort`` and ``partition``. The ``quicksort`` function is the main function that calls itself recursively to sort the array. The ``partition`` function is used to partition the array into two sub-arrays and return the index of the pivot element. The pivot element is chosen as the last element of the array in this pseudocode, but it can be any element in the array.

### Java Code

Based on the pseudocode above, we can start to implement the quick sort algorithm in Java. The pseudocode has two functions, ``quicksort`` and ``partition``. Howvever, we don't the end-user to have to include the low and high parameters when they call out sorting function. Therefore, we can create a wrapper function that calls the ``quicksort`` function with the low and high parameters set to the first and last index of the array, respectively.

```java
public static <T extends Comparable<? super T>> void quicksort(final T[] array, final Comparator<T> comparator) {
    int low = 0;
    int high = size - 1;

    _quicksort(array, low, high, comparator);
}

private static <T extends Comparable<? super T>> void _quicksort(final T[] array, int low, int high, final Comparator<T> comparator) {}

private static <T extends Comparable<? super T>> int partition(final T[] array, int low, int high, final Comparator<T> comparator) {}
```

We will be implementing quick sort recursively. That means it's probably going to be short method that does a lot. As with any recursive function/method we will need a base case and the a recursive call to keep us moving through the problem. For now we will assume the ``partition`` method is implemented and we will focus on the ``_quicksort`` method.

The method takes two arguments: 1) our input data and 2) a comparator, which can be used to control the sort ordering. See [Comparators](./java_reference/comparators.md) for more information on what comparators do and how they are used. For now, they simply let us separate some of the sorting logic so that we don't have to redefine our merge sort method for every different type of ordering (ascending, descending, etc.).

The most complicated part of our method signature is the [Bounded Type Parameter](./java_reference/bounded_typed_parameter.md), which is associated with our [Generic Type](./java_reference/generics.md) type ***T***. 
Generics allow us to indicate that any type can be associated with a class or method, a bounded generic type is an a more constrained version of a generic. They allow us to indicate that the generic type we want to use with our class/method must implement some interface, i.e. it has to have some functionality implemented. 

In our method signature, we declare a bounded type parameter ***T***, which must implement the [Comparable](./java_reference/comparable.md). We are simple starting that whatever type ***T*** is, it must implement the the ``Comparable`` interface which allows it to be compared against other objects.
The ``?`` symbol is a feature of the Generics system. It is called a wildcard and is used to enable lower-bounded types. These allow us to pass any subtype/subclass of the type/class ***T*** to our method. Any class that inherits from ***T*** can also be passed to our sort method.

```java
public static <T extends Comparable<? super T>> void quicksort(final T[] array, final Comparator<T> comparator) {
    int low = 0;
    int high = size - 1;

    _quicksort(array, low, high, comparator);
}

private static <T extends Comparable<? super T>> void _quicksort(final T[] array, int low, int high, final Comparator<T> comparator) {
    if (low < high) {
        int pivotIndex = partition(array, low, high, comparator);
        _quicksort(array, low, pivotIndex - 1, comparator);
        _quicksort(array, pivotIndex + 1, high, comparator);
    }
}

private static <T extends Comparable<? super T>> int partition(final T[] array, int low, int high, final Comparator<T> comparator) {}
```
In the above code, we first check our base case, and if it is not met we call the ``partition`` method and then call the ``_quicksort`` method recursively on the two sub-arrays. That's all there is to this method. The only thing left to do is implement the ``partition`` method.


```java
public static <T extends Comparable<? super T>> void quicksort(final T[] array, final Comparator<T> comparator) {
    int low = 0;
    int high = array.length - 1;

    _quicksort(array, low, high, comparator);
}

private static <T extends Comparable<? super T>> void _quicksort(final T[] array, int low, int high, final Comparator<T> comparator) {
    if (low < high) {
        int pivotIndex = partition(array, low, high,  comparator);
        _quicksort(array, low, pivotIndex - 1, comparator);
        _quicksort(array, pivotIndex + 1, high,  comparator);
    }
}

private static <T extends Comparable<? super T>> int partition(final T[] array, int low, int high, final Comparator<T> comparator) {
    // select the pivot element
    T pivot = array[high];

    int i = low - 1;

    // iterate through the array and move elements less than the pivot to the left
    for (int j = low; j < high; j++) {
        if (comparator.compare(array[j], pivot) < 0) {
            i++;
            swap(array, i, j);
        }
    }
    swap(array, i + 1, high);
    return i + 1;
}

private static <T extends Comparable<? super T>> void swap(final T[] array, int i, int j) {
    T temp = array[i];
    array[i] = array[j];
    array[j] = temp;
}
```

## Complexity

- Best case: In the best-case scenario, Quicksort's time complexity is ``O(n * log n)``. This occurs when the pivot selection consistently divides the array into approximately equal halves, resulting in balanced partitions at each recursive step. As a result, the algorithm efficiently sorts the array with a relatively small number of comparisons and swaps.

- Average case: Quicksort's average-case time complexity is also ``O(n * log n)``. This average complexity is achieved when the pivot selection leads to reasonably balanced partitions at each step of the recursion. Although the exact number of comparisons and swaps may vary based on the input data and the pivot selection strategy, Quicksort generally exhibits consistent performance with a time complexity proportional to ``n * log n`` on average.

- Worst case: In the worst-case scenario, Quicksort's time complexity is O(n<sup>2</sup>). This occurs when the pivot selection consistently results in unbalanced partitions, such as when the smallest or largest element is always chosen as the pivot. In such cases, each partitioning step only reduces the size of one partition by one element, leading to ``n`` recursive calls and a total time complexity of O(n<sup>2</sup>). However, with proper pivot selection strategies (such as median-of-three or randomized pivot selection), the likelihood of encountering the worst-case scenario can be significantly reduced, ensuring better overall performance.

