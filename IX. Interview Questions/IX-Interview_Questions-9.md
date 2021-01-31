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
				printArray(arr);

				toInsert = startOfSorted;
				startOfSorted = toInsert-1;
			}
		}
    }
```