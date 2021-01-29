# IX. Interview Questions - Chapter 5 | Bit Manipulation


## Bit Manipulation By Hand
(row by row)
1. 0110 + 0010 = 1000
2. 0011 * 0101 = 1110
3. 0110 + 0110 = 1100
4. 0011 + 0010 = 0101
5. 0011 * 0011 = 1001
6. 0100 * 0011 = 1100
7. 0110 - 0011 = 0011
8. 1101 >> 2 = 0011
9. 1101 ^ (~1101) = 1111
10. 1000 - 0110 = 0010
11. 1101 ^ 0101 = 1000
12. 1011 & (~0 << 2) = 1000

## Bit Facts and Tricks

x ^ 0s = x
- Explanation: A string XORed by zeroes won't affect the string because only a 1 would affect the string. 

x & 0s = 0
- Explanation: A string AND zeroes, zeroes out the string because you need a 1 just to get a 1 so all zeroes means it wouldn't result in any 1s

x | 0s = x
- Explanation: A string ORed by 0s equals the same string much like XORing it

x ^ 1s = ~x
- Explanation: A string XORed by 1s is the negation of that string because in the original string, 1s would get turned to 0 by 1s. The zeroes in the original string would turn into 1, therefore every bit will flip.

x & 1s = x
- Explanation: A string AND 1s equals the original string. The string won't change. 1 & 1 1 and 0 & 1 = 0. No bits change.

x | 1s = 1s
- Explanation: A string ORed by 1s equals 1s. 1 | 1 = 1 and 0 | 1 = 1. Therefore the string will result in a string of 1s.

x ^ x = 0
- Explanation: A string XORed by itself = 0. 1 ^ 1 = 0 and 0 ^ 0 = 0. All the 1 bits flip to 0 and all the zero bits don't change. The string gets zeroed out.

x & x = x
- Explanation: A string AND itself doesn't affect the string. 1 & 1 = 1 and 0 & 0 = 0. No bits change.

x | x = x
- Explanation: A string ORed by itself doesn't affect the string. 1 | 1 = 1 and 0 | 0 = 0. No bits change.

## Two's Complement and Negative Numbers
- The two's complement is equal to the NOT the integer plus 1 with the left-most bit representing the sign
- The sign bit is stored in the leftmost bit where 1 indicates a negative value

## Arithmetic vs. Logical Right Shift

- Logical Right Shift: Shift the bits and put a 0 in the most significant (left-most) bit. Indicated by a >>> operator

- Arithmetic Right Shift: Shift value but fill in new bits with the value of the sign bit

## Common Bit Tasks: Getting and Setting

1. Get Bit
**Algorithm:**
- Shift `1` by i bits.
- AND it with the number
- If the bit at bit i is a 1, the result will be something > 0
- If it's a 0 then it will return 0 

2. Set Bit.
**Algorithm:**
- Shift `1` by i bits.
- OR the number with it and return it.
- The mask should have only impacted the bit at bit i. If it was a 0, turns to 1. If 1 then it would stay 1.

3. Clear Bit
**Algorithm:**
- Shift `1` by i bits.
- Negate it so it looks like 11101111
- AND it with that mask
- Rationale: 1s will stay 1s and 0s will stay 0s. However, the 0 at bit i will always result in a 0.
**Clearing all bits from the most significant bit through i (inclusive)**
- Shift `1` by i bits
- Subtract 1 from that number: Ex. 00100000 -> 00011111
- AND it with that mask
**Think about this: How do we clear all bits from i through 0? (i-inclusive to the least significant bits)**

4. Update Bit

# Interview Questions

### 5.1 Insertion
> You are given two 32-bit numbers, N and M, and two bit positions, i and j. Write a method to insert M into N such that M start at bit j and ends at bit i. You can assume that the bits j through i have enogh space to fit all of M. That is, if M = 10011, you can assume that there are at least 5 bits between j and i. You would not, for example, have j = 3 and i = 2, because M could not fully fit between bit 3 and bit 2.

Algorithm:
1. Left-shift by i times
2. OR M with N
```
insertion(N, M, i, j) {
	M = M << i;
	return N | M;
}
```

Correction:
- You will need to clear the bits from i to j first


### 5.2 Binary to String
> Given a real number between 0 and 1 (e.g., 0.72) that is passed in as a double, print the binary representation. If the number cannot be represented accurately in binary with at most 32 characters, print "ERROR".

Algorithm:
1. Multiply the number until it is > 0 or print "ERROR" if number turns > 2^31 (which represents the max value of a signed int)
2. Loop through bits, printing.

```
binaryToString(double) {
	while(double < 0) {
		if(double > pow(2, 31)) {
			print("ERROR");
			return;
		}
		double = double * 10;
	}

	for(int i = 0; i < 32; i++) {
		print(getBit(double, i));
	}
}

getBit(num, i) {
	int mask = 1 << i;
	return num & mask;
}
```

### 5.3 Flip Bit to Win
> You have an integer and you can flip exactly one bit from a 0 to a 1. Write code to find the length of the longest sequence of 1s you could create.

Algorithm:
1. First, find the first 1 bit in the integer.
2. Store max sequence in an integer
3. Start nested loop for each bit
	1. Each loop has a counter for sequence of 1s found
	2. Also has an integer numZeroes for each zero it encounters
		1. If a zero is encountered, increment counter and numZeroes
		2. If numZeroes > 1 break.
		3. If done looping through, break.
		4. Store in max if max.

```
flipBitToWin(num) {
	// Find first 1 to start looping through num
	int start;
	for(int i = 0; i < 32; i++) {
		if(getBit(num, i) > 0) {
			start = i;
		}
	}

	int max = 0;
	// Loop through rest of string of bits to find max sequence
	for(int i = start; i < 32; i++) {
		int counter = 0;
		int numZeroes = 0;
		for(int j = i; i < 32; i++) {
			if(numZeroes > 1) {
				break;
			} else if(getBit(num, j) == 0) {
				numZeroes++;
			} else if(getBit(num, j) > 0 && numZeroes < 2) {
				counter++;
			}
		}
		if(counter > max) max = counter;
	}

	return max;
}

// getBit except the ith bit is counted from the most significant bits
getBit(num, i) {
	int mask = 1 << (32-i);
	return num & mask;
}
```

### 5.4 Next Number
> Given a positive integer, print the next smallest and the next largest number that have the same number of 1 bits in their binary representation.

Brute-Force:
1. To get the next smallest number: decrement the number until you find one with the same number of 1 bits
2. Same with next largest except keep incrementing.

```
nextNumber(num) {

	int nextSmallest, nextLargest;

	// First, get the number of 1 bits in num
	int numBits = count1Bits(num);

	int numTemp = --num;
	while(numTemp > 0) {
		if(count1Bits(numTemp) == numBits)
			nextSmallest = numTemp;
			break;
		numTemp--;
	}

	numTemp = ++num;
	boolean found = false;
	while(!found) {
		if(count1Bits(numTemp) == numBits)
			nextLargest = numTemp;
			found = true;
		numTemp++;
	}

	print(nextSmallest);
	print(nextLargest);
}

count1Bits(num) {
	int numBits = 0;
	int numTemp = num;
	while(numTemp > 0) {
		if(getBit(numTemp, 0) > 0) numBits++;
		numTemp >> 1;
	}
	return numBits;
}
```

### 5.5 Debugger
> Explain what the following code does: ((n & (n-1)) == 0)

Answer:
1. What does n-1 do?
	1. If it's odd, it just 0s the least significant bit.
	2. Otherwise, it switches around some 1s and 0s
2. When is n & (n-1) equal to 0?
	1. When the left-most bit of n is equal the sole 1 bit.
3. What does it imply?
	1. That n is some exponent of 2.
4. Therefore, the piece of code checks if n is some exponent of 2 where log_x(n) = 1 where x is the place of the most significant bit.

### 5.6 Conversion
> Write a function to determine the number of bits you would need to flip to convert integer A to integer B

Algorithm:
1. Take the larger of the two numbers
2. Obtain the place of the most significant bit of that number
3. Loop through both integers and count how many bits to be flipped

```
conversion(intA, intB) {
	int start = 0;
	int startNum = intA > intB ? intA : intB;
	for(int i = 0; i < 32; i++) {
		if(getBit(startNum, i) > 0)
			start = i;
			break;
	}

	int counter = 0;
	for(int i = start; i < 32; i++) {
		if(getBit(intA, i) != getBit(intB, i))
			counter++;
	}

	return counter;
}

// getBit except the ith bit is counted from the most significant bits
getBit(num, i) {
	int mask = 1 << (32-i);
	return num & mask;
}
```

### 5.7 Pairwise Swap
> Write a program to swap odd and even bits in an integer with as few instructions as possible (e.g., bit 0 and bit 1 are swapped, bit 2 and bit 3 are swapped, and so on).

```
pairwiseSwap(num) {
	for(int i = 0; i < 31; i++) {
		int even = getBit(num, i);
		int odd = getBit(num, i+1);
		setBit(num, odd, i);
		setBit(num, even, i+1);
	}
}

setBit(num, bit, i) {
	// If bit is 0, zero out at i by ANDing num
	if(bit == 0) return (num & ~(1 << i))
	// If bit is 1, set bit i to 1
	else return (num | (1 << i))
}
```

Optimized:
- Create masks for odd and even places
	- Odd bit mask looks like: 0101 0101
		- = 2^0 + 2^2 + 2^4 + 2^6...
	- Even bit mask looks like: 1010 1010
		- = 2^1 + 2^3 + 2^5 + 2^7...
	- How do we create this mask?
- AND the masks with the integer
- Left shift the odd mask
- Right shift the even mask
- OR them together

```
function optimized(n) {
	int oddMask = 0;
	int evenMask = 0;

	for(int i = 0; i <= 32; i+=2) {
		oddMask |= 1 << i;
	}
	for(int i = 1; i < 32; i+=2) {
		evenMask |= 1 << i;
	}

	int oddBits = n & oddMask;
	int evenBits = n & evenMask;

	oddBits = oddBits << 1;
	evenBits = evenBits >> 1;

	return oddBits | evenBits;
}
```

### 5.8 Draw Line
> A monochrome screen is stored as a single array of bytes, allowing eight consecutive pixels to be stored in one byte. The screen has width w, where w is divisible by 8 (that is, no byte will be split across rows). The height of the screen, of course, can be derived from the length of the array and the width. Implement a function that draws a horizontal line from (x1, y) to (x2, y).
- The method signature should look something like: drawLine(byte[] screen, int width, int x1, int x2, int y)

```
drawLine(byte[] screen, int width, int x1, int x2, int y) {
	int height = screen.length / width;
	int startOfRow = (y - 1) * width;

	int startX = startOfRow + x1;

	for(int i = startX; i < startX + x2; i++) {
		screen[i] = screen[i] | (~0);
	}
}
```
