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

### 3.1 Three in One - Incomplete
> Describe how you could use a single array to implement three stacks.

What is a stack?
- a data structure where the first element that is inserted is the last one removed
- also the last element inserted is the first removed

How do we implement one stack with a single array?
- use one pointer to keep track of the top of the stack
- each time you insert you update the pointer (increment)
- each time you remove, you update the pointer (decrement)

```
class ArrayStack {
  constructor(n) {
    this.stack = new Array(n);
    this.top = null;
  }
  push(item) {
    if(this.top === null) {
      this.top = 0;
      this.stack[this.top] = item;
    } else {
      this.stack[this.top+1] = item;
      this.top = this.top + 1;
    }
  }
  pop() {
    if(this.top === null) {
      console.log('error');
    } else {
      let ret = this.stack[top];
      this.top = this.top - 1;
      return ret;
    }
  }

  peek() {
    return this.stack[this.top];
  }
}
```

How do we achieve this with three stacks in one array?
- Same concept except have three pointers

```
class ArrayStackOfThree {
  constructor(n) {
    this.stack = new Array(n);
    this.topA = null;
    this.topB = null;
    this.topC = null;
  }

  push(stackN, item) {
    let stack = `top${stackN}`;

    if(this[stack] === null) {
      switch(stack) {
        case 'topA':
          this[stack] = 0;
          this.stack[this[stack]] = item;
          break;
        case 'topB':
          this[stack] = 1;
          this.stack[this[stack]] = item;
          break;
        case 'topC':
          this[stack] = 2;
          this.stack[this[stack]] = item;
          break;
      }
    } else {
      this.stack[this[stack]] = item;
      this.top = this.top + 3;
    }
  }

  pop(stackN) {
    let stack = `top${stackN}`;

    if(this[stack] === null) {
      console.log('error');
    } else {
      let ret = this.stack[this[stack]];
      this.top = this.top - 3;

      if(this[stack] < 4) {
        this[stack] = null;
      }
      
      return ret;
    }

  }

  peek(stackN) {
    let stack = `top${stackN}`;

    return this.stack[this[stack]];
  }
}
```

### 3.2 Stack Min
> How would you design a stack which, in addition to push and pop, has a function `min` which returns the minimum element? `Push`, `pop` and `min` should all operate in O(1) time.

Answer: 
- You hold the min in a variable and update min every time push is called. However, how do you update min when you pop an item? You can maybe include a variable in each item that contains the next largest item. You would do this when you push an item.
- Another answer based on hint #27: The obvious way is to calculate min every time you push an item. If popping the min element, recalculate by looping through stack. This uses O(n) space-time complexity. However, you can derive that the amortized runtime would still be O(1) from looking at how Java's ArrayList class works. Doubling an array is an O(n) operation, but is amortized to be O(1) as n grows.

### 3.3 Stack of Plates
> Imagine a (literal) stack of plates. If the stack gets too high, it might topple. Therefore, in real life, we would likely start a new stack when the previous stacks exceeds some threshold. Implement a data structure SetOfStacks that mimics this. SetOfStacks should be composed of several stacks and should create a new stack once the previous one exceeds capacity. SetOfStacks.push() and SetOfStacks.pop() should behave identically to a single stack (that is, pop() should return the same values as it would if there were just a single stack).
> FOLLOW UP: Implement a function popAt (int index) which performs a pop operation on a specific sub-stack.

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
> Implement a MyQueue class which implements a queue using two stacks

Design:
```
- Has two variables: stack1, stack2
- add(item): should add to the end of the line
  - if stack1 empty add to stack1
  - else push everything from stack1 to stack2, push item to stack1, then push everything from stack2 to stack1
- remove(): should remove the start of the line
  - if empty throw exception
  - else pop stack1
- peek(): return top of queue
  - if empty throw exception
  - else return stack1.peek()
- isEmpty()
  - return stack1.isEmpty()
```

### 3.5 Sort Stack
- Write a program to sort a stack such that the smallest items are on the top. You can use an additional temporary stack, but you may not copy the elements into any other data structure (such as an array). The stack supports the following operations: push, pop, peek, and isEmpty.

Design:
```
Most of the design is going to involve pushing:
- push(item)
  - if empty, push item
  - else peek() and compare
    - if top of stack is less than item push to temporary stack and repeat until it is pushed
    - else push item to top of stack

Implementation:

push(item) {
  let new = new StackNode(item);
  if(isEmpty()) top = new;
  else
    const topOfStack = peek();
    if(topOfStack.data > new.data)
      topOfStack.next = new;
      top = new;
    else
      const tempStack = new Stack();
      
      while(topOfStack <= new.data && !isEmpty())
        tempStack.push(pop());
        topOfStack = peek();

      if(isEmpty())
        top = new;
        while(!tempStack.isEmpty())
          push(tempStack.pop())
      else
        peek().next = new;
        while(!tempStack.isEmpty())
          push(tempStack.pop())
}
```

### 3.6 Animal Shelter - Incomplete
- An animal shelter, which holds only dogs and cats, operates on a strictly "first in, first out" basis. 
- People must adopt either the "oldest" (based on arrival time) of all animals at the shelter, or they can select whether they would prefer a dog or a cat (and will receive the oldest animal of that type). 
- They cannot select which specific animal they would like. 
- Create the data structures to maintain this system and implement operations such as enqueue, dequeueAny, dequeueDog, and dequeueCat. 
- You may use the built-in LinkedList data structure.

