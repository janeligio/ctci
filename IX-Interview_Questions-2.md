# IX. Interview Questions - 1. Arrays and Strings

### Remove Dups

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