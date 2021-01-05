# IX. Interview Questions - Chapter 3 | Stacks and Queues

## Stack Data Structure

**last-in-first-out (LIFO)**
Functions:
- `pop()`
- `push(item)`
- `peek()`
- `isEmpty()`

## Queue Data Structure

**first-in-first-out (FIFO)**
Functions:
- `add(item)`
- `remove()`
- `peek()`
- `isEmpty()`

### 3.1 Three in One
- Describe how you could use a single array to implement three stacks.

Answer: 

### Stack Min
- How would you design a stack which, in addition to push and pop, has a function `min` which returns the minimum element? `Push`, `pop` and `min` should all operate in O(1) time.

Answer: 
- You hold the min in a variable and update min every time push is called. However, how do you update min when you pop an item? You can maybe include a variable in each item that contains the next largest item. You would do this when you push an item.
- Another answer based on hint #27: The obvious way is to calculate min every time you push an item. If popping the min element, recalculate by looping through stack. This uses O(n) space-time complexity. However, you can derive that the amortized runtime would still be O(1) from looking at how Java's ArrayList class works. Doubling an array is an O(n) operation, but is amortized to be O(1) as n grows.

### Stack of Plates
- Imagine a (literal) stack of plates. If the stack gets too high, it might topple. Therefore, in real life, we would likely start a new stack when the previous stacks exceeds some threshold. Implement a data structure SetOfStacks that mimics this. SetOfStacks should be composed of several stacks and should create a new stack once the previous one exceeds capacity. SetOfStacks.push() and SetOfStacks.pop() should behave identically to a single stack (that is, pop() should return the same values as it would if there were just a single stack).
- FOLLOW UP: Implement a function popAt (int index) which performs a pop operation on a specific sub-stack.

Design:
```
- SetOfStacks has:
	- ArrayList to store stacks (list)
	- threshold
- push(item)
	- if list is empty add new stack and push item to stack
	- else if the stack on the end of list is at threshold add new stack and push item to that stack
	- else push item to the stack at the end of list
- pop()
	- if list is empty throw exception
	- pop the stack at the end of list and check if that stack is empty
		- if empty, remove from the list
- isEmpty()
	- check if list length is 0
- popAt(index)
	- check if index is < 0 or >= list.length
	- pop the stack at list[index]
		- if stack is empty after popping
			- remove from list
			- shift items to the left to fill that space
```

### 3.4 Queue via Stacks
- Implement a MyQueue class which implements a queue using two stacks