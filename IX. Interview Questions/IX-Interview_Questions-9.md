# IX. Interview Questions - Chapter 9 | System Design and Scalability

*Basic Sorting Algorithms Implementation*

##### Bubble Sort

Java:
```
void bubbleSort(int[] arr) {
        for(int i = 0; i < arr.length; i++) {
            for(int j = 0; j < arr.length-1; j++) {
                if(arr[j] > arr[j+1]) {
                    swap(arr, j, j+1);
                }
            }
        }
    }
```

##### Selection Sort
Java:
```
void selectionSort(int[] arr) {
        for(int i = 0; i < arr.length; i++) {
            int min = i;
            for(int j = i; j < arr.length; j++) {
                if(arr[j] < arr[min]) {
                    min = j;		
                }
            }
            swap(arr, i, min);
        }
    }
```

##### Insertion Sort
```
void insertionSort(int[] arr) {
        for(int i = 1; i < arr.length; i++) {
            int toInsert = i;
            // Insert in the correct place in the sorted pile
            int startOfSorted = toInsert-1;
            while(startOfSorted > -1  && arr[toInsert] < arr[startOfSorted]) {
                swap(arr, toInsert, startOfSorted);
                toInsert = startOfSorted;
                startOfSorted = toInsert-1;
            }
        }
    }
```

##### Merge Sort

```
    void mergeSort(int[] arr) {
        int[] helper = new int[arr.length];
        mergeSort(arr, helper, 0, arr.length-1);
    }

    void mergeSort(int[] array, int[] helper, int low, int high) {
        if(low < high) {
            int middle = (low + high)/2;
            mergeSort(array, helper, low, middle); // Sort left half
            mergeSort(array, helper, middle+1, high); // Sort right half
            merge(array, helper, low, middle, high); // Merge
        }
    }

    void merge(int[] array, int[] helper, int low, int middle, int high) {
        // Copy both halves into helper array
        for(int i = low; i <= high; i++) {
            helper[i] = array[i];
        }
        
        int helperLeft = low;
        int helperRight = middle+1;
        int current = low;
        
        // Iterate through helper array. Compare the left and right half, copying back the smaller
        // element from the two halves into the original array
        while(helperLeft <= middle && helperRight <= high) {
            if(helper[helperLeft] <= helper[helperRight]) {
                array[current] = helper[helperLeft];
                helperLeft++;
            } else {
                array[current] = helper[helperRight];
                helperRight++;
            }
            current++;
        }
        
        // Copy the rest of the left side of the array into the target array
        int remaining = middle - helperLeft;
        for(int i = 0; i <= remaining; i++) {
            array[current+i] = helper[helperLeft + i];
        }
    }
```

##### Quicksort

```
	void quickSort(int[] arr, int left, int right) {
		int index = partition(arr, left, right);
		if(left < index - 1) {
			quickSort(arr, left, index - 1);
		}
		if(index < right) {
			quickSort(arr, index, right);
		}
	}
	
	int partition(int[] arr, int left, int right) {
		int pivot = arr[(left+right) / 2]; // Pick pivot;
		while(left <= right) {
			// Find element on left that should be on right
			while(arr[left] < pivot) left++;
			
			// Find element on right that should on left
			while(arr[right] > pivot) right--;
			
			// Swap elements, and move left and right indices
			if(left <= right) {
				printArray(arr);
				System.out.println("Swapping: " + arr[left] + " with " + arr[right]);
				swap(arr, left, right);
				left++;
				right--;
			}
		}
		return left;
	}
```

### 10.1 Sorted Merge
> You are given two sorted arrays, A and B, where A has a large enough buffer at the end to hold B. Write a method to merge B into A in sorted order.

A looks like [23 43 2354 254 2 2              ]
B looks like [ 23 2354 25 25]

Algorithm:
- Start at the end of A, where the largest item should be.
- Insert into that space the larger of the ends of array A and array B
- Do this until all of B is inserted

Java:
```
	// 10.1 Sorted Merge
	void sortedMerge(int[] a, int[] b) {
		int index = a.length - 1;
		int indexA = a.length - b.length - 1;
		int indexB = b.length - 1;
		
		while(indexB >= 0) {
			if(a[indexA] > b[indexB]) {
				swap(a, indexA, index);
				index--;
				indexA--;
			} else if(a[indexA] == b[indexB]) {
				a[index] = a[indexA];
				index--;
				a[index] = b[indexB];
				indexA--;
				indexB--;
			} else {
				a[index] = b[indexB];
				index--;
				indexB--;
			}
			index--;
		}
	}
```

Runtime: O(b)
Spacetime: O(1)

### 10.2 Group Anagrams
> Write a method to sort an array of Strings so that all the anagrams are next to each other.

If string A is an anagram of string B, string A is equal to string B if they are both sorted

Brute-force:
- Implement boolean isAnagram(String a, StringB) that checks if one string is an anagram of another
- For each element, find its anagrams, and swap places

Java:
```
	static void groupAnagrams(String[] strings) {
		int sortedIndex = 0;
		for(int i = strings.length-1; i > sortedIndex; i--) {
			swap(strings, i, sortedIndex);
			int j = strings.length-1;
			// Find anagrams
			String currentString = strings[sortedIndex];
			out.println(currentString);
			while(j >= sortedIndex) {
				printArray(strings);
				out.println(j);
				if(isAnagram(strings[j], currentString)) {
					sortedIndex++;
					swap(strings, j, sortedIndex);
				}
				j--;
			}
			sortedIndex++;
			out.println("Sorted index: " +sortedIndex);
		}
	}
	
	static void swap(String[] strings, int x, int y) {
		String temp = strings[y];
		strings[y] = strings[x];
		strings[x] = temp;
	}
	
	static boolean isAnagram(String a, String b) {
		a = sortString(a);
		b = sortString(b);
		if(a.length() != b.length()) return false;
		
		int i = 0;
		while(i < a.length()) {
			if(a.charAt(i) != b.charAt(i)) return false;
			i++;
		}
		
		return true;
	}
    static String sortString(String inputString) 
    { 
        // convert input string to char array 
        char tempArray[] = inputString.toCharArray(); 
          
        // sort tempArray 
        Arrays.sort(tempArray); 
          
        // return new sorted string 
        return new String(tempArray); 
    }
```

### 10.3 Search in Rotated Array - Incomplete
> Given a sorted array of n integers that has been rotated an unknown number of times, write code to find an element in the array. You may assume that the array was originally sorted in increasing order.


1. Find the start of the array
2. Implement binary search that handles this
3. Run binary search

### 10.4 Sorted Search, No Size
> You are given an array-like data structure `Listy` which lacks a size method. It does, however, have an elementAt(i) method that returns the element at index i in O(1) time. If i is beyond the bounds of the data structure, it returns -1. (For this reason, the data structure only supports positive integers.) Given a Listy which contains sorted, positive integers, find the index at which an element x occurs. If x occurs multiple times, you may return any index.

This problem consists of finding the size of this list. How do we do that? It would perform like a binary search where you keep incrementing by an exponential amount until what you get is < 0. The problem is how much do we increment by? 

```
void sortedSearch(Listy arr, int val) {

    int incrementBy = 10;
    int length = 0;
    int range = 0;
    while(i > 0) {
        if(arr.get(range) < 0) {
            for(int i = range-incrementBy; i < range; i++) {
                if(arr.get(i) < 0) {
                    length =  i;
                }
            }
        }
        range += incrementBy;
    }

    return binarySearch(arr, 0, length);
}
```