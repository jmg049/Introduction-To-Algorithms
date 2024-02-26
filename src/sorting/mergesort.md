# [Merge Sort](https://en.wikipedia.org/wiki/Merge_sort)
Merge sort is a highly efficient comparison-based sorting algorithm known for its stable performance and effectiveness with large datasets. Operating on the principle of divide and conquer, merge sort divides the input array into smaller sub-arrays until each sub-array consists of only one element. It then merges these sub-arrays in a sorted manner, repeatedly combining adjacent sub-arrays until a single sorted array remains. The merging process involves comparing elements from each sub-array and placing them in the correct order in a temporary array, ensuring that the resulting array is sorted. With a guaranteed worst-case time complexity of O(n log n), merge sort is widely used in various applications where stable and predictable performance is required, such as sorting massive datasets or implementing efficient external sorting algorithms. Its recursive nature and straightforward implementation make merge sort a fundamental concept in the study of sorting algorithms and algorithm design.


## Implementation

### Pseudocode
``` pseudocode
mergesort(array)
    if array.length > 1
		left = array[0:array.length / 2]
		right = array[array.length / 2:array.length]
        mergesort(left)
        mergesort(right)
        merge(array, leftIndex,  rightIndex)

merge(array, left, right)
	leftIndex = 0
	rightIndex = 0
	index = 0

	while leftIndex < left.length && rightIndex < right.length
		if left[leftIndex] < right[rightIndex]
			array[index] = left[leftIndex]
			leftIndex += 1
		else
			array[index] = right[rightIndex]
			rightIndex += 1
		index += 1

	while leftIndex < left.length
		array[index] = left[leftIndex]
		leftIndex += 1
		index += 1

	while rightIndex < right.length	
		array[index] = right[rightIndex]
		rightIndex += 1
		index += 1

```

The pseudocode above outlines the merge sort algorithm, which consists of two main functions: ``mergesort`` and ``merge``. The ``mergesort`` function is a recursive function that divides the input array into smaller sub-arrays until each sub-array consists of only one element. The ``merge`` function is used to merge the sub-arrays in a sorted manner, ensuring that the resulting array is sorted. The merging process involves comparing elements from each sub-array and placing them in the correct order in a temporary array.

### Java Code
```java
public class Sorting {
	
	public static <T extends Comparable<? super T>> mergesort(final T[] array,
															  final Comparator<T>) {
		int length = array.length;
		if (length < 2) {
			return;
		}
		// rest of the code									
	}
	
}
```
Starting off the we will define out method signature and include the simple bounds check on the size of our input.

The method takes two arguments: 1) our input data and 2) a comparator, which can be used to control the sort ordering. See [Comparators](../java_reference/comparator.md) for more information on what comparators do and how they are used. For now, they simply let us separate some of the sorting logic so that we don't have to redefine our merge sort method for every different type of ordering (ascending, descending, etc.).

The most complicated part of our method signature is the [Bounded Type Parameter](../java_reference/bounded_type_parameter.md), which is associated with our [Generic Type](../java_reference/generics.md) type ***T***. 
Generics allow us to indicate that any type can be associated with a class or method, a bounded generic type is an a more constrained version of a generic. They allow us to indicate that the generic type we want to use with our class/method must implement some interface, i.e. it has to have some functionality implemented. 

In our method signature, we declare a bounded type parameter ***T***, which must implement the [Comparable](../java_reference/comparable.md). We are simple starting that whatever type ***T*** is, it must implement the the ``Comparable`` interface which allows it to be compared against other objects.
The ``?`` symbol is a feature of the Generics system. It is called a wildcard and is used to enable lower-bounded types. These allow us to pass any subtype/subclass of the type/class ***T*** to our method. Any class that inherits from ***T*** can also be passed to our sort method.
Now that we have looked at the method signature, let's implement the sorting functionality.

The first thing we need to do from the pseudocode is to split our input array into two smaller arrays. Since we are dealing with Arrays, we can make use of the [Java Arrays](https://docs.oracle.com/javase/8/docs/api/java/util/Arrays.html) package.

```java
public class Sorting {
	
	public static <T extends Comparable<? super T>> void mergesort(final T[] array,
															       final Comparator<T>) {
		int length = array.length;

		final T[] left = Arrays.copyOfRange(array, 0, array.length / 2);
		final T[] right = Arrays.copyOfRange(array, array.length / 2, array.length);
		
		// rest of code
	}

}
```

Merge sort can be broadly implemented in two different ways, either Recursively (Top-down) or Non-Recursively (Bottom-up). In this example we will be implementing it Recursively. 

After we split the initial array into two, we want to also sort these two smaller arrays. In fact we want to keep doing this until we hit smaller arrays with just 1 element left in them. So we need two important things:
1) Recusive method calls
2) A base/stopping condition

Let's update the code to reflect these requirements
```java
public class Sorting {
	
	public static <T extends Comparable<? super T>> void mergesort(final T[] array,
															       final Comparator<T>) {
		int length = array.length;
		if (length < 2) {
			return;
		}
		final T[] left = Arrays.copyOfRange(array, 0, array.length / 2);
		final T[] right = Arrays.copyOfRange(array, array.length / 2, array.length);
		
		mergeSort(left, comparator);
		mergeSort(right, comparator);
		
		// rest of code
	}

}
```


We've made sure that we stop at the bottom, but we have yet to implement any sorting. 

Merge sort operates on the basis of dividing the initial array into smaller and smaller slices, and then merge these small slices back together to recreate the initial array.

To do this, we need to *empty* out each of the smaller lists and insert their values into the main array at the correct index. 


```java
public class Sorting {
	
	public static <T extends Comparable<? super T>> void mergesort(final T[] array,
																   final Comparator<T>) {
		int length = array.length;
		if (length < 2) {
			return;
		}
		
		final T[] left = Arrays.copyOfRange(array, 0, array.length / 2);
		final T[] right = Arrays.copyOfRange(array, array.length / 2, array.length);
		
		mergeSort(left, comparator);
		mergeSort(right, comparator);
		
		int leftIndex = 0;
		int rightIndex = 0;
		int index = 0;
		
		while (leftIndex < left.length && rightIndex < right.length) {
			T leftItem = left[leftIndex];
			T rightItem = right[rightIndex];
			
			## If left < right
			if (comparator.compare(leftItem, rightItem) < 0) {
				array[index] = left[leftIndex];
				leftIndex += 1;
			} else {
				array[index] = right[rightIndex];
				rightIndex += 1;
			}
			index += 1;
		}

	}

}
```

One last thing though, depending on how unsorted the original input was, we could end up with a situation that leaves a significant number of elements in either the left or right array. So we need to account for these and insert them at the *end* of the array. This is the correct spot for them since each sub array is sorted. 

```java

public class Sorting {
	
	public static <T extends Comparable<? super T>> void mergesort(final T[] array,
														           final Comparator<T>) {
		int length = array.length;
		if (length < 2) {
			return;
		}
		
		final T[] left = Arrays.copyOfRange(array, 0, array.length / 2);
		final T[] right = Arrays.copyOfRange(array, array.length / 2, array.length);
		
		mergeSort(left, comparator);
		mergeSort(right, comparator);
		
		int leftIndex = 0;
		int rightIndex = 0;
		int index = 0;
		
		while (leftIndex < left.length && rightIndex < right.length) {
			T leftItem = left[leftIndex];
			T rightItem = right[rightIndex];
			
			// If left < right
			if (comparator.compare(leftItem, rightItem) < 0) {
				array[index] = left[leftIndex];
				leftIndex += 1;
			} else {
				array[index] = right[rightIndex];
				rightIndex += 1;
			}
			index += 1;
		}

	
		// merge the remaining elements(if there are any)
		while (leftIndex < left.length) {
			array[index] = left[leftIndex];
			leftIndex += 1;
			index += 1;
		}
		
		while (rightIndex < right.length) {
			array[index] = right[rightIndex];
			rightIndex += 1;
			index += 1;
		}
	}
}

```

And that's our fully functioning merge sort that will sort any array of type ***T*** as long as ***T*** is ``Comparable``.

## Complexity

The average case time complexity for Merge Sort is: $$O(n * log (n))$$

How do we get to this? Well let's look at the operations that happen during merge sort. We have the splitting of the array into two small arrays, which repeats until they have just 1 element. When we reduce the input size by a factor of 2 each iteration, we get a $$log(n)$$ complexity. So that's the first part of the time complexity. So for a given array, we will split it in half ``log n `` times.

The ``n`` part then reflect the total amount of comparisons made during the merge step of our sort. In the worst case each element is compared at most once to every other element, giving the ``n`` component of our complexity.

By joining the two complexities we get $$O(n * log(n))$$

The worst, best and average time complexities for merge sort are all the same.
