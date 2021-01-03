Cracking the Coding Interview

VI. Big O

Additional Problems (pp. 55)

VI.1

O(b)

- loop is only dependent on variable b

VI.2

O(b)

- last case is called b times

VI.3

O(1)

- every operation is performed in constant time

VI.4

O(a/b)

- while statement is dependent on a and b.

VI.5 (review)

O(n)

VI.6

O(sqrt n)

- loop only goes up to sqrt n

VI.7 

If a binary search tree is not balanced, how long might it take (worst case) to find an element in it?

- Balanced BST: the height of the left and right subtree of any node differ by not more than 1. Worse case scenario means the tree is just a linked list. If the an element to be searched was the at the end of this linked list, it would perform in O(n) time.

VI.8

You are looking for a specific value in a binary tree, but the tree is not a binary search tree. What is the time complexity of this?

- Binary Tree: Each node has at most two children

- Binary Search Tree: Each node's left subtree has keys that are < that node and right subtree has keys > that node AND the left and right subtree must be a binary search tree.

- Searching in a binary tree would be O(n) because it's not sorted in any way.

VI. 9

How long does copying an array take?

- copyArray has a loop that calls appendToNew n times.
- appendToNew's loop gets executed like 1 + 2 +...n times
- this is equivalent to n(n-1)/2
- so the runtime is O(n^2)

VI. 10

- runtime is based on how many digits are in a number
- how can we represent this?
- the number of digits in a number is equal to how many times n can be divided by 10
- therefore the runtime is O(log N)
- Technically the base of the log is 10

VI. 11 (review)

VI. 12

A # items in a
B # items in b

- mergesort runtime: O(B log B)
- iteration through a runtime: O(A log B)
- total runtime: O(B log B + A log B) = O((A+B)log B)