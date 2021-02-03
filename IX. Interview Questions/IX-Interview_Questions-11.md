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

How do we simulate simultaneous traffic to a webpage? We can open up a bunch of windows. Or we can create a program to do it for us. I suggest using multiple threads to send a bunch of HTTP requests via something like curl or HTTPie. I would only get back the HTTP header instead of wasting resources on the body. Actually, it's probably important that we test HTTP server's ability to serve as well. That's an important question because we can see if the webpage is optimized and not an unreasonable file size. After N tests, we see what happens with to HTTP headers. Are we getting an OK? Good. It might also be wise to do this testing from more than one location if we were able to do that because the webpage would probably get cached and those HTTP requests we were making would probably not even get to our servers.

### 11.5 Test a Pen
> How would you test a pen?

First we see if it does what it actually does. It does? Great. Then we can look at its durability. If I accidentally drop a pen an cold floor tile will it shatter like glass? Would it stop working? I'd probably do these tests N times. Then I would test how long we can use this pen before it runs out of ink. If we're not worried about the ink then we can probably skip this. If we're not the supplier of that, then who cares? If we were, then we'd probably want a good lasting pen. How long we aiming here? A couple months, a year before having to replace it? This probably determines a lot of the price. Then we test it under normal circumstances. We perhaps perform surveys to collect data on real-world everyday use. Does it hold up? Are people satisfied?

### 11.6 Test an ATM
> How would you test an ATM in a distributed banking system?