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

### 3.1 Three in One:
- Describe how you could use a single array to implement three stacks.

Answer: 

### Stack Min
- How would you design a stack which, in addition to push and pop, has a function `min` which returns the minimum element? `Push`, `pop` and `min` should all operate in O(1) time.

Answer: 
- You hold the min in a variable and update min every time push is called. However, how do you update min when you pop an item? You can maybe include a variable in each item that contains the next largest item. You would do this when you push an item.
- Another answer based on hint #27: The obvious way is to calculate min every time you push an item. If popping the min element, recalculate by looping through stack. This uses O(n) space-time complexity. However, you can derive that the amortized runtime would still be O(1) from looking at how Java's ArrayList class works. Doubling an array is an O(n) operation, but is amortized to be O(1) as n grows.