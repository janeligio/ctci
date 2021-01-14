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
- You are given two 32-bit number, N and M, and two bit positions, i and j. Write a method to insert M into N such that M start at bit j and ends at bit i. You can assume that the bits j through i have enogh space to fit all of M. That is, if M = 10011, you can assume that there are at least 5 bits between j and i. You would not, for example, have j = 3 and i = 2, because M could not fully fit between bit 3 and bit 2.