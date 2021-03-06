# IX. Interview Questions - Chapter 10 | Sorting and Searching

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

### 10.5 Sparse Search
> Given a sorted array of strings that is interspersed with empty strings, write a method to find the location of a given string.

### 10.6 Sort Big File
> Imagine you have a 20 GB file with one string per line. Explain how you would sort the file.

Realistically, you can't put 20GB into memory, so you'd have to sort this partition by partition.

1. Decide how big each partition is. Let's say 1GB
2. For the first GB, we run bucket sort.
    - If we just assume that the strings only contain characters from a-z, we'd have 26 buckets.
3. Write to file
    - We would have 26 files
4. Keep processing original file, GB by GB
    - At each GB we drop each string in the bucket that corresponds to its first letter
5. Once all the strings have been dropped into a bucket, we sort each bucket.
    - After a bucket is sorted, copy back to a master file that will store the sorted array of strings.

### 10.7 Missing Int
> Given an input file with four billion non-negative integers, provide an algorithm to generate an integer that is not contained in the file. Assume you have 1 GB of memory available for this task

> FOLLOW UP: What if you have only 10 MB of memory? Assume that all the values are distinct and we now have no more than one billion non-negative integers.

1. Find the min or max
2. We know for sure that an integer outside of the min and max range will not be an integer in the list
3. Process file, GB at a time and simply iterate through all integers.
4. Once the file is processed, you will have a min and max and you can choose any number outside of this range.

### 10.8 Find Duplicates
> You have an array with all numbers from 1 to N, where N is at most 32,000. The array may have duplicate entries and you do not know what N is. With only 4 kilobytes of memory available, how would you print all duplicate elements in the array.

4 Kilobytes = 1K 4-byte ints = 1024 4-byte ints

Max value of 2-byte short is 65535.

So you can process 2048 2-byte shorts.

For this kind of problem, it is wise to think about using bit vectors.

### 10.9 Sorted Matrix Search
> Given an M x N matrix in which each row and each column is sorted in ascending order, write a method to find an element.

Peforming binary search at each row O(min(M,N)log(max(M,N)))
- Naive approach

Driver program to generate such a matrix:
```
	int[][] initSortedMatrix(int M, int N) {
		int[][] matrix = new int[M][N];
		Random rand = new Random();
		for(int i = 0; i < matrix.length; i++) {
			for(int j = 0; j < matrix[i].length; j++) {
				if(i == 0 && j == 0) {
					matrix[i][j] = rand.nextInt(10);
				} else if(i-1 < 0) {
					int greater = matrix[i][j-1];
					matrix[i][j] = rand.nextInt(10)+greater;
				} else if(j-1 < 0) {
					int greater = matrix[i-1][j];
					matrix[i][j] = rand.nextInt(10)+greater;
				} else {
					int greater = matrix[i][j-1] > matrix[i-1][j] ? matrix[i][j-1] : matrix[i-1][j];
					int newRandom = rand.nextInt(10) + greater;
					matrix[i][j] = newRandom;				
				}
				matrix[i][j] = matrix[i][j] + 1;
			}
		}
		return matrix;
	}
```

Java:
```
	static int sortedMatrixSearch(int[][] matrix, int value) {
		for(int i = 0; i < matrix.length; i++) {
			int result = binarySearch(matrix[i], value);
			if(result > 0) {
				return result;
			}
		}
		return -1;
	}
	
	static int binarySearch(int[] array, int value) {
		int result = -1;
		
		int low = 0;
		int high = array.length-1;
		while(low <= high) {
			int middle = (low/high)/2;
			if(array[middle] == value) {
				return array[middle];
			} else if(array[middle] > value) {
				low = middle+1;
			} else {
				high = middle-1;
			}
		}
		
		return result;
	}
```

A matrix of such kind would look something like this:
```
[9][12][17][18]
[13][15][20][27]
[22][28][38][42]
[32][39][47][54]
[35][45][56][59]
[43][50][58][61]
```

~~We can see that at all numbers at each "diagonal", is greater than all the numbers in the previous diagonal and less than all the numbers in the next diagonal.~~

That matrix could also look like this:

```
[10][11][12][13][14]
[15][16][17][18][19]
[20][21][22][23][24]
[25][26][27][28][29]
[30][31][32][33][34]
```

We can eliminate a column from contention by checking if the value falls within the range of matrix[i][0] and matrix[i][matrix[i].length-1].

We can also see that at each "subsquare", the bottom index contains the integer smaller than each integer in the subsquare. The top index contains the integer greater than each integer in the subsquare.

So we can perform searches like this:

```
Looking for: 16

[10][11][12][13][14]
[15][16][17][18][19]
[20][21][22][23][24]
[25][26][27][28][29]
[30][31][32][33][34]

1. Split into squares and check top and bottom indices

Subsquare 1
([10])[11][12][13][14]
[15][16][17][18][19]
[20][21][22][23]([24])

Subsquare 2
([25])[26][27][28][29]
[30][31][32][33]([34])

Range of ss1 is 10-24
Range of ss2 is 25-34

So check ss1

2. Split into squares and check top and bottom indices

Subsquare 1
([10])[11][12][13][14]
[15][16][17][18]([19])

Subsquare 2
([20])[21][22][23]([24])

Range of ss1 is 10-19
Range of ss2 is 20-24

So check ss1

3. Split into squares and check top and bottom indices

Subsquare 1
([10])[11][12][13]([14])

Subsquare 2
([15])[16][17][18]([19])

Range of ss1 is 10-14
Range of ss2 is 15-19

4. If we're left with one row/one array, let's just do binary search on ss1
```

Runtime analysis:
Finding the correct row is a matter of how many rows there are. Performing binary search after finding the row is a log(N) operation. Since these are both logarithmic runtimes, it doesn't matter if you split up the squares row-wise or column-wise.

```
	int sortedMatrixSearchOptimized(int[][] matrix, int value) {
		int[] range = {0, matrix.length-1};
		
		int c = matrix[0].length;
		
		while(range[0] != range[1]) {
			int mid = (range[1] - range[0])/2;
			
			int bottomIndex1 = matrix[range[0]][0];
			int topIndex1 = matrix[mid][c-1];
			int[] subSquare1 = {bottomIndex1, topIndex1};

			int bottomIndex2 = matrix[mid+1][0];
			int topIndex2 = matrix[range[1]][c-1];
			int[] subSquare2 = {bottomIndex2, topIndex2};
			
			if(inRange(subSquare1, value)) {
				range[1] = mid;
			} else if(inRange(subSquare2, value)) {
				range[0] = mid+1;
			}
		}
		
		int item = binarySearch(matrix[range[0]], value);
		
		return item;
	}
	
	boolean inRange(int[] square, int value) {
		if(value > square[0] && value < square[1]) {
			return true;
		}
		return false;
	}
```

### 10.10 Rank from Stream - Incomplete
> Imagine you are reading in a stream of integers. Periodically, you wish to be able to look up the rank of a number x (the number of values less than or equal to x). Implement the data structures and algorithms to support these operations. That is, implement the method track(int x), which is called when each number is generated, and the method getRankOfNumber(int x), which returns the number of values less than or equal to x (not including x itself).

EXAMPLE

Stream (in order of appearance): 5, 1, 4, 4, 5, 9, 7, 13, 3

`getRankOfNumber(1) = 0`
`getRankOfNumber(3) = 1`
`getRankOfNumber(4) = 3`

### 10.11 Peaks and Valleys - Incomplete
> In an array of integers, a "peak" is an element which is greater than or qual to the adjacent integers and a valley is an element which is less than or equal to the adjacent integers. For example, in the array {5, 8, 6, 2, 3, 4, 6}, {8, 6} are peaks and {5, 2} are valleys. Given an array of integers, sort the array into an alternating sequence of peaks and valleys

9 19 10 4 19 9 13 13 1 7 
19 9 10 4 19 9 13 13 1 7 