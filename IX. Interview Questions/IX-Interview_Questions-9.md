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