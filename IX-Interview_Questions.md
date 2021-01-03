# IX. Interview Questions

## 1. Arrays and Strings

**1.1 Is Unique**
- Implement an algorithm to determine if a string has all unique characters. What if you cannot use additional data structures?

JavaScript:

Using Set data structure

```
const isUnique = str => {
	const uniqStr = new Set(str);
	return uniqStr.size === str.length;
}
```

** 1.2 Check Permutation **
- Given two strings, write a method to decide if one is a permutation of the other.

```
string a,b

sortedA = mergesort(a)	// O(A log A)
sortedB = mergesort(b)	// O(B log B)

return sortedA == sortedB
```

** 1.3 URLify - Incomplete** 

- Write a method to replace all spaces in a string with '%20'. You may assume that the string has sufficient space at the end to hold the additional characters, and that you are given the "true" length of the string. (Note: If implementing in Java, please use a character array so that you can perform this operation in place.)

Input: const str = 'Mr John Smith      ', 13
Output: 'Mr%20John%20Smith'
```
const URLify = str => {
	// Get empty spaces
	let emptySpaces = 0;
	let i = str.length - 1;

	while(str[i] === ' ') {
		emptySpaces++;
		i--;
	}

	emptySpaces /= 3; // Because an empty space is equivalentn to '%20'

	for(let i = 0; i < emptySpaces; i++) {
		// Find index of white space
		let index = 0;
		let j = 0;
		while(str[j] !== ' ') {
			if(str[j] === ' ') index = j;
			else j++;
		}

		// Shift characters
		for(let k = index+1; k < str.length - emptySpaces; k+=5) {
			let temp = [];
			for(let l = k + 5; l > 0; l--) {
				temp.push(str[l]);
			}
			str[k+1] = str[k];
			str[k+2] = temp.pop();
		}
	}
}
```

** 1.4 Palindrome Permutation **

- Given a string, write a function to check if it is a permutation of a palindrome. A palindrome is a word or phrase that is the same forwards and backwards. A permutation is a rearrangement of letters. The palindrome does not need to be limited to just dictionary words.

Algorithm: Sort the string. Count occurrences of each character. % by 2 and if > 0 add to a counter. If counter > 1, it is not a palindrome permutation.

Reasoning: For each unique character in a palindrome, it either has a pair or it doesn't. If it doesn't have a pair, that means that character is in the middle of the string (number of characters in the string is odd).

Pseudocode:
```
	mergesort(input)

	let count = 1;
	let isOdd = 0;
	for(let i = 0; i < input.length; i++)
		if(input[i+1] === input[i])
			count++
		else
			if(count % 2 == 1) isOdd++
			count = 0;

	return isOdd > 1;

```

*Runtime: O(N log N)*


** 1.5 One Away **

- There are three types of edits that an be performed on strings: insert a character, remove a character, or replace a character. Given two strings, write a function to check if they are one edit (or zero edits) away.

Method: Check all three instances
- Insert a character is true if: there exists a character in the string that, if removed, would make the two strings equal.

- Remove a character is true if: same as insert except you can switch parameters.

- Replace a character is true if: there exists a character x in string A and a character y in string B that, if removed, would make string A and B equal.

In JavaScript:
```
let oneAway = (stringA, stringB) => {
  if(stringA === stringB) return true;

  if(Math.abs(stringA.length - stringB.length) > 1) return false;

  if(insertCharacterTrue(stringA, stringB)) return true;
  if(insertCharacterTrue(stringB, stringA)) return true; // Check if remove character true by flipping parameters
  if(replaceCharacterTrue(stringA, stringB)) return true;

  return false;
}

let insertCharacterTrue = (stringA, stringB) => {
  if(stringA.slice(1,stringA.length-1) === stringB) return true;

  for(let i = 0; i < stringA.length - 1; i++)
    if(stringA[i] !== stringB[i])
      let str = stringA.splice(0,i-1) + stringA.splice(i+1,stringA.length-1);
      return str === stringB;
}

let replaceCharacterTrue = (stringA, stringB) => {
  if(stringA.length !== stringB.length) return false;

  for(let i = 0; i < stringA.length; i++) 
    if(stringA[i] !== stringB[i])
      let x = stringA.slice(0,i-1) + stringA.slice(i+1, stringA.length-1);
      let y = stringB.slice(0,i-1) + stringB.slice(i+1, stringB.length-1);
      return x === y;
}
```

