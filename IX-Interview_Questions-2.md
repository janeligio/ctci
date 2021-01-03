# IX. Interview Questions - 2. Linked Lists

### 2.1 Remove Dups

- Write code to remove duplicates from an unsorted linked list.
- FOLLOW UP: How would you solve this problem if a temporary buffer is not allowed?

A -> D -> C -> T -> T -> K

Algorithm: Loop through list and add values to a buffer if not already in buffer. If the value is in buffer, delete node.

JavaScript:
(Singly-linked list)
```
const removeDups = head => {
  let buffer = [];
  let curr = head;

  buffer.push(curr.val); // First item is not duplicate

  while(curr.next !== null) {
    let nextVal = curr.next.val;

    let duplicate = false;

    // Determine if dup
    for(let i = 0; i < buffer.length; i++) {
      if(buffer[i] === nextVal)
        duplicate = true;
    }

    if(duplicate) {
      curr.next = curr.next.next;
    } else {
      buffer.push(nextVal)
    }
  }
}
```

### 2.2 Return Kth to Last

- Implement an algorithm to find the kth to last element of a singly inked list.

JavaScript:
```
const returnKth = (head, k) => {
  let curr = head;
  let numNodes = 0;

  if(curr === null) return null;

  while(curr.next !== null) {
    numNodes++;
    curr = curr.next;
  }

  curr = head;
  for(let i = 0; i < numNodes - k; i++) {
    curr = curr.next; // Traverse to correct node
  }

  return curr;
}
```

### 2.3 Delete Middle Node

- Implement an algorithm to delete a node in the middle (i.e., any node but the first and last node, not necessarily the exact middle) of a singly linked list, given only access to that node.

JavaScript:
```
const deleteMiddleNode = node => {
  let val = node.val;
  let nextVal = node.next.val;

  let curr = node;
  while(curr.next !== null) {
    // Case: 2nd to last node
    if(curr.next.next === null) {
      curr.val = curr.next.val;
      curr.next.val = curr.next.next.val;
      curr.next.next = null;
    } else {
      curr.val = nextVal;
      curr = curr.next;
    }
  }
}
```

### 2.4 Partition

- Write code to partition a linked list around a value x, such that all nodes less than x come before all nodes greater than or equal to x. If x is contained within the list, the values of x only need to be after the elements less than x (see below). The partition element x can appear anywhere in "right partition"; it does not need to appear between the left and right partitions.

JavaScript:
```
const partition = (head, partition) => {
  let leftPartitionHead = null;
  let leftCurr = null;
  let rightPartitionHead = null;
  let rightCurr = null;

  let curr = head;
  while(curr.next !== null) {
    if(curr.val < partition) {
      if(leftPartitionHead === null) {
        leftPartitionHead = new Node(curr.val);   // Initialize head
        leftCurr = leftPartitionHead;
      } else {
        leftCurr.next = new Node(curr.val); // Insert into left partition
        leftCurr = leftCurr.next;
      }
    } else {
      if(rightPartitionHead === null) {
        rightPartitionHead = new Node(curr.val); // Initialize head
        rightCurr = rightPartitionHead;
      } else {
        rightCurr.next = new Node(curr.val); // Insert into right partition
        rightCurr = rightCurr.next
      }
    }
  }

  let leftTail = leftPartitionHead;
  while(leftTail.next !== null) {
    leftTail = leftTail.next;
  }
  leftTail.next = rightPartitionHead;   // Connect list

  return leftPartitionHead;
}
```

### 3.5 Sum Lists

- You have two numbers represented by a linked list, where each node contains a single digit. The digits are stored in *reverse* order, such that the 1's digit is at the head of the list. Write a function that adds the two numbers and returns the sum as a linked list.

JavaScript:
```
const sumLists = (list1, list2) => {
  return convert(list1) + convert(list2);
}

const convert = head => {
  let curr = head;
  let exp = 0; // Current power of 10
  let sum = 0;

  while(curr !== null) {
    sum += curr.val * Math.pow(10, exp);
    exp++;
    curr = curr.next;
  }

  return sum;
}

```

### 2.6 Palindrome

- Implement a function to check if a linked list is a palindrome.

Algorithm: Use stack and compare to original list.

```
const palindrome = head => {
  let stack = [];

  let curr = head;
  while(curr !== null) {
    stack.push(new Node(curr.val));
    curr = curr.next;
  }

  curr = head;
  let palindrome = true;
  while(curr !== null && palindrome) {
    if(curr.val !== stack.pop()) {
      palindrome = false;
    } else {
      curr = curr.next;
    }
  }

}
```