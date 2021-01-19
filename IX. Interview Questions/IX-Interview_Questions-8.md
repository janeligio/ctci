# IX. Interview Questions - Chapter 8 | Recursion and Dynamic Programming

### 8.1 Triple Step
> A child is running up a staircase with n steps and can hop either 1 step, 2 steps, or 3 steps a time. Implement a method to count how many possible ways the child can run up the stairs.

- Can be thought of as the number of permutations an array of x integers, where x can be 1, 2, or 3, can add up to n.
- Can also be thought of as how many times n can be subtracted by an integer in the domain of {1, 2, 3} until it reaches 0.
- The relation is just like the fibonacci sequence: tripleStep(n) = tripleStep(n-1) + tripleStep(n-2) + tripleStep(n-3)
```
// Top-down approach (really bad but works)
function tripleStep(n) {
    // Base cases
    if(n < 0) return 0;
    if(n === 0) return 1;
    return tripleStep(n-1) + tripleStep(n-2) + tripleStep(n-3);
}

// Bottom-up memoized approach
// With sequence in mind: tripleStep(n) = tripleStep(n-1) + tripleStep(n-2) + tripleStep(n-3)

function tripleStep(n) {
    let memo = new Array(n+1);
    for(let i = 0; i < n; i++) {
        memo[i] = 0;
    }

    memo[1] = 1;
    memo[2] = 2;
    memo[3] = 4;

    for(let i = 4; i < n; i++) {
        if(memo[i] === 0) {
            memo[i] = memo[i-1] + memo[i-2] + memo[i-3];
        }
    }
    return memo[i];
}
```

### 8.2 Robot in a Grid
> Imagine a robot sitting on the upper left corner of grid with r rows and c columns. The robot can only move in two directions, right and down, but certain cells are "off limits" such that the robot cannot step on them. Design an algorithm to find a path for the robot from the top left to the bottom right.