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

    // Zero out because JavaScript doesn't automatically do it
    for(let i = 0; i < n+1; i++) {
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

```
function robotInAGrid(r, c, grid) {
    return robotInAGridHelper(0, 0, r, c, grid, '');
}

function robotInAGridHelper(x, y, r, c, grid, path) {
    let rowTraversal = 'r';
    let colTraversal = 'c';

    if(x === r-1 && y === c-1) {
        return '';
    }

    if(grid[x+1][y] !== 'off limits' && (x+1) < r-2) {
        return robotInAGridHelper(x+1, y, r, c, grid, path + rowTraversal);
    }
    if(grid[x][y+1] !== 'off limits' && (y+1) < c-2) {
        return robotInAGridHelper(x, y+1, r, c, grid, path + colTraversal);
    }

}
```

### 8.3 Magic Index
> A magic index in an array A[0...n-1] is defined to be an index such that A[i] = i. Given a sorted array of distinct integers, write a method to find a magic index, if one exists, in array A.

> *FOLLOW UP - What if the values are not distinct?*

- Since the array is sorted, binary search seems like a viable option.
- arr[i] = x
- if i < x ... 
    - the magic index can't be i
    - the magic index can't be > i
    - so look in the left side
- if i > x ... a magic index may be in the "right" side of the array
    - the magic index can't be i
    - the magic index can't be > i
    - so look in the right side

```
// First Attempt
function magicIndex(arr) {
    if(arr.length === 2) {
        if(arr[0] === 0) return 0;
        if(return [1] === 1) return 1;
        return 'No magic index';
    }

    let middleIndex = Math.floor(arr.length / 2);

    let i = arr[middleIndex];

    if(i === middleIndex) {
        return middleIndex;
    } else if(middleIndex < i) {
        magicIndex(arr.slice(0, middleIndex));
    } else if(middleIndex > i) {
        magicIndex(arr.slice(middleIndex + 1, arr.length))
    }
}
```

- You can deduce that if you traverse the array from 0 to n-1, if arr[i] > i, the next index to search from is arr[i] + 1, because the magic index can't be in those indeces. 
- Ex: [2, 10, ... ]
    - arr[2] > 10
    - therefore, if a magic index exists, it is not at arr[3] because arr[3] > 10 and 3 is definitely less than 10.
    - it also can't be at arr[10+1] because 11 (if it exists) has to be somwhere between arr[3] and arr[arr[3]+1]
    - wait... yeah it can't be in the right side of the array
- So where do we start searching?
    - Let's assume the magic index is 5 => arr[5] = 5
    - If the array is sorted, then there exists numbers < 5 that populate the array
        - Those numbers will also be magic indeces

### 8.4 Power Set - Incomplete
> Write a method to return all subsets of a set.

- Power Set: The set of all subsets of a set including empty set and itself.

```
// Brute-force
powerSet(set) {

}
```

### 8.5 Recursive Multiply
> Write a recursive function to multiply two positive integers without using the * operator. You can use addition, subtraction, and bit shifting, but you should minimize the number of those operations.

```
function recursiveMultiply(x, y) {
    if(y === 0) return 0;

    return x + recursiveMultiplyHelper(x, y-1);
}
```
A little more optimized:
```
function recursiveMultiply(x, y) {
    if(y === 0) return 0;
    if(y === 1) return x;
    return recursiveMultiplyHelper((x << 1), (y-2));
}
```

### 8.6 Towers of Hanoi - Incomplete
> In the classic problem of the Towers of Hanoi, you have 3 towers and N disks of different sizes which can slide onto any tower. The puzzle starts with disks sorted in ascending order of size from top to bottom (i.e., each disk sits on top of an even larger one). You have the following constraints:
1. Only one disk can be moved at a time.
2. A disk is slid off the top of one tower onto another tower.
3. A disk cannot be placed on top of a smaller disk.
> Write a program to move the disks from the first tower to the last using stacks.

```
function init(N) {
    let towerA = [];
    let towerB = [];
    let towerC = [];
    
    for(let i = N; i > 0; i--) {
        towerA.push(i);
    }

    towersOfHanoi(towerA, towerB, towerC, N);
}
function towersOfHanoi(towerA, towerB, towerC, N) {
    // Base case?
    if(towerA.length === 0 && towerB.length === 0 && towerC.length === N) {
        return ; // something
    }



}

function moveDisk(tower1, tower2) {
    if()
}
```

### 8.7 Permutations without Dups
> Write a method to compute all permutations of a string of unique characters.

```
// Brute force implementation

function permutationWithoutDups(str) {
    let permutations = [];
    
    permutations.push(str);
    let s = '';
    for(let i = str.length-1; i >= 0; i--) {
        s += str[i];
    }
    permutations.push(s);

    for(let i = 0; i < str.length; i++) {
        let strWithoutCharacter = str.slice(0, i) + str.slice(i+1, str.length);
        let permutation = insertCharInEachPos(str[i], strWithoutCharacter);
        permutations = [...permutations, ...permutation];
    }

    return new Set(permutations);
}

function insertCharInEachPos(c, str) {
    let permutations = [];

    let s = '';
    for(let i = 0; i < str.length; i++) {
        let beginningStr = str.slice(0, i);
        let endStr = str.slice(i, str.length);
        s = beginningStr + c + endStr;
        permutations.push(s);
    }
    permutations.push(str+c);
    return permutations;
}
```

### 8.8 Permutations with Dups - Incomplete
> Write a method to compute all permutations of a string whose characters are not necessarily unique. The list of permutations should not have duplicates.

What does a string without unique characters imply?

### 8.9 Parens
> Implement an algorithm to print all valid (e.g., properly opened and closed) combinations of n pairs of parentheses.

What is a valid parenthesis pair?
- p = ( ) or ''
- p = \*( \* ) \*
- \* means zero or more instances can occur

*A valid string of N parentheses pairs is one where there are N opening parentheses and N closing parentheses.*

