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
- The sign bit is stored in the leftmost bit where 1 indicates a negative value

## Arithmetic vs. Logical Right Shift

- Logical Right Shift: Shift the bits and put a 0 in the most significant (left-most) bit. Indicated by a >>> operator

- Arithmetic Right Shift: Shift value but fill in new bits with the value of the sign bit

## Common Bit Tasks: Getting and Setting

1. Get Bit

2. Set Bit

3. Clear Bit

4. Update Bit