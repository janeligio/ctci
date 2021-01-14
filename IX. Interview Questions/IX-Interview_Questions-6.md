# IX. Interview Questions - Chapter 6 | Math and Logic Puzzles


#### Generating a List of Primes: The Sieve of Eratosthenes
- The Sieve of Eratosthenes is a highly efficient way to generate a list of primes.
- It works by recognizing that all non-prime numbers are divisible by a prime number 

Algorithm:
1. Start with a list of all the number up to some value `max`
2. Cross off all number divisible by 2
3. Look for next prime
4. Cross of all numbers divisible by it
5. Repeat until you end up with a list of all prime numbers from 2 through `max`

### The Heavy Pill
> You have 20 bottles of pills. 19 bottles have 1.0 gram pills, but one has pills of weight 1.1 grams. Given a scale that provides an exact measurement, how would you find the heavy bottle? You can only use the scale once.

- Each bottle has x pills with y weight
- One bottle has y = 1.1 and the rest 1.0
1. Number the bottles from 1-20
2. From each bottle put x number of pills on the scale where x is the bottle's number
3. The scale should come out to 20 + 19 + 18 ... grams = 1 + 19/20 + 18/20 + 17/20 ... + 1 /20 = n(n+1) /2 = (20*21)/2 = 210 + some multiple of 0.1.
4. The number of the bottle is (total weight - 210) / 0.1
Ex. The heavier pills are in bottle 1.
	- Weight would come out to 210.1
	- 0.1 / 0.1 = 1 (bottle #1)
Ex. The heavier pills are in bottle 15
	- Weigh would come out to 211.5
	- 211.5 - 210 = 1.5
	 - 1.5 / 0.1 = 15

### Basketball - Incomplete (I'm bad at math)
> You have a basketball hoop and someone says that you can play one of two games.
- Game 1: You get one shot to make the hoop.
- Game 2: You get three shots and you have to make two of three shots.
> If p is the probability of making a particular shot, for which values of p should you pick one game or the other?

S = making a shot
W = winning

S = 1
Game 1: P(W) = P(S) = 100%
Game 2: P(W) = P(S)P(S) = 100%

S = 9/10
Game 1: P(W) = P(S) = 90%
Game 2: P(W) = P(S)P(S) = 81%

S = 0.5

Game 2: P(W) =

Enumeration of all winning outcomes

1. 110
2. 101
3. 111
4. 011

The probability of 1 is P(S)*P(S)*(1-P(S))

- If S = 0.6
- The probability of 2/3 shots is 0.6*0.6*0.4 = 0.144
- Three of these events exist
- The probability of 2/3 shots is 0.6^3 =  0.216
3(0.144)+0.216 = 0.648

The probability of winning Game 2 is higher than the probability of making a shot

The probability generalized:
- S = making a shot
- W = winning
Winning Game 1: P(W) = P(S)

Winning Game 2: P(W) = 3(P(S)*P(S)*(1-P(S))) + P(S)^3
	= 3P(S)^2 + 3(1-P(S)) + P(S)^3
	= P(S)^3 + 3P(S)^2 - P(S) + 3

Answer: Pick Game 2 if S the probability of making a shot is less than S^3 + 3S^2 - S + 3

### Dominos
> There is an 8x8 chessboard in which two diagonally opposite corners have been cut off. You are given 31 dominos, and a single domino can cover exactly two squares. Can you use the 31 dominos to cover the entire board?

- 32 White spaces, 32 Black spaces
- Now either 30 W or 30 B spaces and 32 W/B spaces
- A single domino covers 1 B&W space.
- 31 Dominos = 31 B&W spaces
- This implies that there aren't enough spaces for 31 dominos to fit onto the board