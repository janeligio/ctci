# IX. Interview Questions - Chapter 1 | Arrays and Strings

### 1.1 Is Unique
> Implement an algorithm to determine if a string has all unique characters. What if you cannot use additional data structures?


- Using Set data structure:
JavaScript
```
const isUnique = str => {
	const uniqStr = new Set(str);
	return uniqStr.size === str.length;
}
```

- Without cheating:
  - Algorithm:
      - Insert all characters into hash table
      - If you encounter a collision, the string does not have unique characters
  - O(n)

- What if you can't use additional data structures?
  - Sort string (using in-place algorithm)
  - Loop through string
  - O(n log n)

```
function isUnique(s) {
  let sorted = s.sort();

  let current = sorted[0];

  for(let i = 0; i < sorted.length; i++) {
    if(i < sorted.length-1) {
      if(current === sorted[i+1]) {
        return false;
      } else {
        current = sorted[i+1]
      }
    }
  }

  return true;
}
```

### 1.2 Check Permutation
> Given two strings, write a method to decide if one is a permutation of the other.

```
string a,b

sortedA = mergesort(a)	// O(A log A)
sortedB = mergesort(b)	// O(B log B)

return sortedA == sortedB
```

> 1.3 URLify - Incomplete
> Write a method to replace all spaces in a string with '%20'. You may assume that the string has sufficient space at the end to hold the additional characters, and that you are given the "true" length of the string. (Note: If implementing in Java, please use a character array so that you can perform this operation in place.)

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

	emptySpaces /= 3; // Because an empty space is equivalent to '%20'

	for(let i = 0; i < emptySpaces; i++) {
		// Find index of next white space
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

### 1.4 Palindrome Permutation
> Given a string, write a function to check if it is a permutation of a palindrome. A palindrome is a word or phrase that is the same forwards and backwards. A permutation is a rearrangement of letters. The palindrome does not need to be limited to just dictionary words.

Algorithm: 
- Sort the string. 
- Count occurrences of each character. 
- % by 2 and 
  - if > 0 add to a counter. 
  - If counter > 1, it is not a palindrome permutation.

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


**1.5 One Away**

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

**1.6 String Compression**

- Implement a method to perform basic string compression using the counts of repeated characters. For example, the string aabcccccaaa would become a2b1c5a3. If the "compressed" string would not become smaller than the original string, your method should return the original string. You can assume the string has only uppercase and lowercase letters (a-z).

In JavaScript:

```
let compressString = str => {
  let compressedString = '';

  let count = 1;
  for(let i = 0; i < str.length; i++) {
    if(i === str.length-1) {  // Case if at end of array
      compressedString += str[i] + count;
    } else if(str[i] === str[i+1]) {
      count++
    } else {
      compressedString += str[i] + count;
      count = 1;
    } 
  }

  if(str.length === compressedString.length) return str;
  else return compressedString;
}
```

**1.7 Rotate Matrix**

- Given an image represented by an NxN matrix, where each pixel in the image is 4 bytes, write a method to rotate the image by 90 degrees. Can you do this in place?

4x4 Matrix

Top row:

0,0 -> 3,0
1,0 -> 3,1
2,0 -> 3,2
3,0 -> 3,3

0,1 -> 2,0
1,1 -> 2,1
2,1 -> 2,2
3,1 -> 2,3
In JavaScript:
```
let rotateMatrix = matrix => {
  let n = matrix.length;

  // Initialize new matrix
  let newMatrix = new Array(n);
  for(let i = 0; i < newMatrix.length; i++) {
    newMatrix[i] = new Array(newMatrix.length)
  }

  for(let x = 0; x < n.length; x++)
    for(let y = 0; y < n.length; y++)
      let newX = n - x;
      let newY = x;
      newMatrix[newX][newY] = matrix[x][y]

  return newMatrix;
}
```

A B C    H D A 

D E F -> I E B

H I J    J F C

Notes: I don't know if you can do this in place.

**1.8 Zero Matrix**

- Write an algorithm such that if an element in an MxN matrix is 0, its entire row and column are set to 0.

In JavaScript:
```
let zeroMatrix = matrix => {
  let M = matrix.length;
  let N = matrix[0].length;

  let rows = [];  // The rows to zero
  let cols = [];  // The columns to zero

  // Search for zeroes
  for(let row = 0; row < M; row++)
    for(let col = 0; col < N; col++)
      if(matrix[row][column] === 0)
        rows.push(col);
        cols.push(row);

  // Make unique to reduce runtime
  rows = new Set(rows);
  cols = new Set(cols);

  // Zero out rows and columns
  for(let i = 0; i < rows.length; i++) {
    matrix = zeroRow(rows[i], matrix, M)
  }
  for(let i = 0; i < cols.length; i++) {
    matrix = zeroCol(cols[i], matrix, N)
  }
}

let zeroRow = (row, matrix, M) => {
  for(let i = 0; i < M; i++) {
    matrix[i][row] = 0;
  }
}

let zeroCol = (col, matrix, N) => {
  for(let i = 0; i < N; i++) {
    matrix[col][i] = 0;
  }
}
```

Assumes matrix is mutable.

**1.9 String Rotation - Incomplete**

- Assume you have a method isSubstring which checks if one word is a substring of another. Given two strings, s1 and s2, write code to check if s2 is a rotation of s1 using only one call to isSubstringn (e.g., "waterbottle" is a rotation of "erbottlewat").

In JavaScript:
```
let isStringRotated = (str1, str2) => {
  if(str1.length !== str2.length) return false;


}
```

*I have no idea how to do this*.