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

