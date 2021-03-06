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

### 6.1 The Heavy Pill
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

### 6.2 Basketball - Incomplete (I'm bad at math)
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

The probability of 1 is P(S)\*P(S)\*(1-P(S))

- If S = 0.6
- The probability of 2/3 shots is 0.6\*0.6\*0.4 = 0.144
- Three of these events exist
- The probability of 2/3 shots is 0.6^3 =  0.216
3(0.144)+0.216 = 0.648

The probability of winning Game 2 is higher than the probability of making a shot

The probability generalized:
- S = making a shot
- W = winning
Winning Game 1: P(W) = P(S)

Winning Game 2: P(W) = 3(P(S)\*P(S)\*(1-P(S))) + P(S)^3
	= 3P(S)^2 + 3(1-P(S)) + P(S)^3
	= P(S)^3 + 3P(S)^2 - P(S) + 3

Answer: Pick Game 2 if S the probability of making a shot is less than S^3 + 3S^2 - S + 3

### 6.3 Dominos
> There is an 8x8 chessboard in which two diagonally opposite corners have been cut off. You are given 31 dominos, and a single domino can cover exactly two squares. Can you use the 31 dominos to cover the entire board?

- 32 White spaces, 32 Black spaces
- Now either 30 W or 30 B spaces and 32 W/B spaces
- A single domino covers 1 B&W space.
- 31 Dominos = 31 B&W spaces
- This implies that there aren't enough spaces for 31 dominos to fit onto the board

### 6.4 Ants on a Triangle - Incomplete (unverified)
> There are three ants on different vertices of a triangle. What is the probability of collision (between any two or all of them) if they start walking on the sides of the triangle? Assume that each ant randomly picks a direction, with either direction being equally likely to be chosen, and that they walk at the same speed.
> Similarly, find the probability of collision with n ants on an n-vertex polygon.

- The probability of ant meeting another ant is the probability of it moving in that direction and the probability of that other ant also going in that direction
- Since the probabilities are equal (0.5) then the chance is 0.5 * 0.5 = 0.125
- The probability of two ants colliding and the other one walking freely is the sum of the events
- The probability of an ant not colliding is the probability of it choosing a direction * the probability of the end on the other end of the vertex not choosing that direction
	- The probabilities are equal so 0.5 * 0.5 = 0.125
- Therefore the probability of 1 collision and 1 no collision = 0.125 + 0.125 = 0.25

### 6.5 Jugs of Water
> You have a five-quart jug, a three-quart jug, and an unlimited supply of water (but no measuring cups). How would you come up with exactly four quarts of water? Note that the jugs are oddly shaped, such that filling up exactly "half" of the jug would be impossible.


The first number indicates the number of quarts in the 5-quart jug and the second, the 3-quart jug
1. 0 0
2. 5 0 (filled up 5q)
3. 2 3 (dropped 3 into 3q)
4. 0 3 (emptied 5q)
5. 3 0 (put 3 into 5q)
6. 3 3 (filled up 3q)
7. 5 1 (put 2 into 5q)
8. 0 1 (emptied 5q)
9. 1 0 (put 1 into 5q)
10. 1 3 (filled up 3q)
11. 4 0 (put 3 into 5q) - done, easy peazy

### 6.6 Blue-Eyed Island (Looked at answer)
> A bunch of people are living on an island, when a visitor comes with a strange order: all blue-eyed people must leave the island as soon as possible. There will be a flight out at 8:00 pm every evening. Each person can see everyone else's eye color, but they do not know their own (nor is anyone allowed to tell them). Additionally, they do not know how many people have blue eyes, although they do know that at least one person does. How many days will it take the blue-eyed people to leave?

Answer: If there x number of blue-eyed people on the island it will x days for them to leave
Why?

### 6.7 The Apocalypse - (Looked at answer)
> In the new post-apocalyptic world, the world queen is desperately concerned about the birth rate. Therefore, she decrees that all families should ensure that they have one girl or else they face massive fines. If all families abide by this policy-that is, they have continue to have children until they have one girl, at which point they immediately stop-what will the gender ratio of the new generation be? (Assume that that odds of someone having a boy or a girl on any given pregnancy is equal.) Solve this out logically and then write a computer simulation of it.

- What number of kids grants the higheset percentage of there existing one girl at the end of it all?
- What number of consecutive boys is it virtually impossible not to have had a girl?

B = 0.5
BB = 0.5*0.5 = 1/4
BBB = 0.5^3 = 1/8
BBB = 0.5^4 = 1/16
BBBB = 0.5^5 = 1/32

G 0.5
BG 0.75
BBG 0.875
BBBG 0.9375
BBBBG 0.96875
BBBBBG 0.984375

Simulation:
```
function simulation(numFam) {
	let boys = 0;
  let girls = 0;
  
  for(let i = 0; i < numFam; i++) {
  	let baby;
    do {
    	isGirl = makeBaby();
      if(isGirl === 0) girls++;
      else boys++;
    } while(isGirl !== 0)  
  }
  console.log(`Boys: ${boys} Girls: ${girls} Ratio:${girls/(boys+girls)}`);
  
}

function makeBaby() {
	return getRandomInt(2);
}

function getRandomInt(max) {
  return Math.floor(Math.random() * Math.floor(max));
}

simulation(100000);
```

Answer: 1:1

### 6.8 The Egg Drop Problem
> There is a building of 100 floors. If an egg drops from the Nth floor or above, it will break. If it's dropped from any floor below, it will not brea. You're given two eggs. Find N, while minimizing the number of drops for the worst case.

Brute-force:
1. This seems like an logarithmic deduction. Start at the middle of 100 floors and drop
2. If it broke, check the middle of the lower floors
3. If it didn't check the middle of the upper floors.
4. Repeat until the breaks on floor N but not floor N-1

Worst case scenario is N = 100
- How do we optimize?
If we start at floor 50 and the egg breaks, then N = somewhere between 1-49. We can try dropping the second egg from 1 all the way up to 49 until it breaks, in which we have found floor N. 

If we start at floor 10 and drop the eggs at then 20 then 30.. 100, we can start dropping the eggs at the set of ten floors before it breaks. Worst-case this would be ten 10*ith floor drops + max of ten times going from the ith floor to 100. Ex: dropping from 1, 10,... 100. Then dropping from 91-98 (if we reached 98 then we'd know it was 99). That is equal 11 + 8 times.

If we did it increments of 5 floors it would be 20 drops then 19 additional drops.

How about 15 floors? 15, 30..90. That's 6 drops plus 14 drops = 20 drops.

The worst case can be defined as N being placed on the floor where, given the best possible algorithm. The algorithm will consist of dropping the first egg incrementally by x floors and then dropping the second egg x-1 times. This can be looked at as y = 100/x + (x - 1) where y is equal to the number of times an egg is dropped. How do we find x where y is minimized? Math.

Unfortunately, I forgot how to do that from Calc I, but I plugged the formula into a calculator and the minima is, in fact, at x = 10.

*This was not the fully-optimized solution, but I tried. Refer to solutions of book for it*

### 6.9 100 Lockers
> There are 100 closed lockers in a hallway. A man begins by opening all 100 lockers. Next, he closes every second locker. Then, on his third pass, he toggles every third locker (closes it if it is open or opens it if it is closed). This process continues for 100 passes, such that on each passi, the man toggles every ith locker. After his 100th pass in the hallway, in which he toggles only locker #100, how many lockers are open?

Let's first try defining how many times a locker has been toggled. 

For even numbers, if it has been toggled an even number of times, it will be closed. For odd numbers, if it has been toggled an even number of times, it will be open. 

Prime numbers will end up being closed, because they will have only been touched one time - when they are encountered in their iteration.

The number of times a locker has been toggled is how many multiples it has. If the number has an even number of factors, it will be closed by the 100th iteration. If it has an odd number of factors, it will be open. I learned this from the solution: only perfect squares have an odd number of factors. 

Why?
- Think about how factors work. They can be thought of as being paired. Let's look at 10. The factors are (1, 10) and (2, 5). Now look at a perfect square like 16: (1, 16), (4, 4), (8, 2). These factors can also be paired up, however it's square is only counted as one number, making the number of factors odd.

Therefore, the number of lockers open is equal to the number of perfect squares. You can derive this by squaring the number from 1 until it exceeds 100. You can do this up until 10. Therefore there will be 10 lockers open.

### Poison
> You have 1000 bottles of soda, and exactly one is poisoned. You have 10 test strips which can be used to detect poison. A single drop of poison will turn the test strip positive permanently. You can put any number of drops on a test strip at once and you can reuse a test strip as many times as you'd like (as long as the results are negative). However, you can only run tests once per day and it takes seven days to return a result. How would you figure out the poisoned bottle in as few days as possible?

1. use 100 drops on each test strip. This will leave 9 strips left and 7 days used.
2. Use 10 drops on each strip resulting in one unused strip. Either 8 or 9 strips left and 7 days used.
3. If it was 9 strips left, then use 1 drop on each strip. 7 days, and you will have figured out the poisoned bottle in a week.
	1. If it was 8 strips then you've found the poisoned bottle, but you gotta wait a week for the results.

This method took 4 weeks.

Next steps: *Optimize*.