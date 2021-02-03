# IX. Interview Questions - Chapter 11 | Testing

### 11.1 Mistake
> Find the mistake(s) in the following code:

```
unsigned int i ;
for (i = 100; i >= 0; i--)
    printf("%d\n", i);
```

What is the this code supposed to do?
- It's supposed to print all the numbers from 100 to 0, line by line.

Observations:
- `i` is an unsigned int, so it can't be negative. What happens when it goes negative?
- The for loop decrements `i` before executing code.

Mistakes:
- The for loop is infinite; it will hit negative and overflow to the max value of a 4-byte unsigned int.
- It should be printing an unsigned number, not a signed number.
- It decrements `i` first, which could be what it's intended to do, but it would go from 99-0 (if the for loop worked as expected)

### 11.2 Random Crashes
> You are given the source to an application which crashes when it is run. After running it ten times in a debugger, you can find it never crashes in the same place. The application is single threaded, and uses only the C standard library. What programming errors could be causing this crash? How would you test each one?

### 11.3 Chess Test
> We have the following method used in a chess game: `boolean canMoveTo(int x, int y)`. This method is part of the `Piece` class and returns whether or not the piece can move to position (x, y). Explain how you would test this method.

Since this method is part of the Piece class, how much access does it have to the board? We can only test this if we know we have access to the board. How does it have access to the board if the board is probably not in the Piece class? The method signature doesn't have a third parameter.

Let's say we do have access to the board. We have an x and y parameter. First, I would test for normal cases. Each piece has a unique move, so for each piece we would test for `true` on all the moves a piece can do. Then we can test the `false` cases which shouldn't be too bad on an 8x8 grid. What other times can a piece move illegally? When that piece puts itself into check, or if a piece of the same color is occupying the spot at (x, y). We test for those cases and make sure they turn out false.

Then we can test out out-of-bound cases and weird inputs. The bounds should be 0-7 for both integers, or even 1-8 if that's how the game is implemented. We should make sure those return `false`. Then we should check for `null`s or even `char`s or `String`s. To avoid catastrophic failure, we should probably validate those types of inputs.

### 11.4 No Test Tools
> How would you load test a webpage without using any test tools?