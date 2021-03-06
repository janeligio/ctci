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
    return memo[n-1] + memo[n-2] + memo[n-3];
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

If y is even, you can double (left-shift) x and halve y (right-shift).
If y is odd, you can add x to the sum.
```
function recursiveMultiply(x, y) {
    recursiveMultiplyHelper(x, y, x);
}

function recursiveMultiplyHelper(x, y) {
    if(y === 0) return 0;
    if(y % 2 !== 0) return x + recursiveMultiplyHelper(x, y-1);
    return x << 2 + recursiveMultiplyHelper(x, (y>>2));
}
```

### 8.6 Towers of Hanoi - Incorrect
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
    let moves = ['AB', 'AC', 'BA', 'BC', 'CA', 'CB'];

    console.log(towersOfHanoi(towerA, towerB, towerC, N, moves, moves[0]));
}

function towersOfHanoi(towerA, towerB, towerC, N, moves, currentMove) {
    // Base case?
    if(towerA.length === 0 && towerB.length === 0 && towerC.length === N) {
        return [towerA, towerB, towerC]; // something
    }

    
    let newTowerA = [...towerA];
    let newTowerB = [...towerB];
    let newTowerC = [...towerC];

    // Possible moves: A to B, A to C, B to A, B to C, C to A, C to B
    if(currentMove === 'AB') {
        let newState = moveDisk(towerA, towerB);
        if(!newState) return false;
        newTowerA = newState[0];
        newTowerB = newState[1];
    }
    if(currentMove === 'AC') {
        let newState = moveDisk(towerA, towerC);
        if(!newState) return false;
        newTowerA = newState[0];
        newTowerC = newState[1];
    }
    if(currentMove === 'BA') {
        let newState = moveDisk(towerB, towerA);
        if(!newState) return false;
        newTowerB = newState[0];
        newTowerA = newState[1];
    }
    if(currentMove === 'BC') {
        let newState = moveDisk(towerB, towerC);
        if(!newState) return false;
        newTowerB = newState[0];
        newTowerC = newState[1];
    }
    if(currentMove === 'CA') {
        let newState = moveDisk(towerC, towerA);
        if(!newState) return false;
        newTowerC = newState[0];
        newTowerA = newState[1];
    }
    if(currentMove === 'CB') {
        let newState = moveDisk(towerC, towerB);
        if(!newState) return false;
        newTowerC = newState[0];
        newTowerB = newState[1];
    }

    let result;
    let index = moves.indexOf(currentMove);

    let newMoves = [...moves.slice(index), ...moves.slice(0, index)];

    console.log(newMoves);
    for(let i = 0; i < moves.length; i++) {
        result = towersOfHanoi(newTowerA, newTowerB, newTowerC, N, newMoves, newMoves[i]);
    }
    return result;
}

function moveDisk(tower1, tower2) {
    // What defines a legal move?
    if(tower1.length <= 0) return false;

    let newTower1 = [...tower1];
    let newTower2 = [...tower2];

    if(tower2.length > 0) {
        let pop1 = tower1[tower1.length-1];
        let pop2 = tower1[tower1.length-2];
        if(pop1 > pop2) return false;
    }

    let disk = newTower1.pop();
    newTower2.push(disk);

    return [newTower1, newTower2];
}
```

Attempt #1 in Java:
```
	static int[][] towersOfHanoi(int[][] towers) {
		if(checkTowers(towers)) {
			System.out.println("Found solution");
			return towers;
		}
		
		int towerA = 0;
		int towerB = 1;
		int towerC = 2;
		
		int[][] possibleMoves = 
				{{towerA, towerB}, {towerA, towerC},
				 {towerB, towerA}, {towerB, towerC},
				 {towerC, towerA}, {towerC, towerA}};
		
		ArrayList<int[]> validMoves = new ArrayList<int[]>();
		
		for(int i=0; i<possibleMoves.length; i++) {
			int tower1 = possibleMoves[i][0];
			int tower2 = possibleMoves[i][1];
			if(checkValid(tower1, tower2, towers)) {
				int[] move = {tower1, tower2};
				validMoves.add(move);
			}
		}
		
		for(int i=0; i<validMoves.size(); i++) {
			int[] move = validMoves.get(i);
			out.println("Move: "+move[0]+" "+move[1]);
			printTowers(towers);
			moveDisk(move[0], move[1], towers);
			towersOfHanoi(towers);
		}
		
		return towers;
	}
	
	// Move disk from towerA to towerB
	static void moveDisk(int towerA, int towerB, int[][] towers) {
		int disk = removeTop(towerA, towers);
		insertTop(towerB, disk, towers);
	}
	
	static int removeTop(int tower, int[][] towers) {
		int disk = 0;
		for(int i = towers[tower].length-1; i >= 0; i--) {
			if(towers[tower][i] > 0) {
				disk = towers[tower][i];
				towers[tower][i] = 0;
			}
		}
		return disk;
	}
	
	static void insertTop(int tower, int disk, int[][] towers) {		
		if(getTop(tower, towers) == 0) {
			towers[tower][0] = disk;
		} else {
			int index = -1;
			int i = 0;
			while(index < 0) {
				if(towers[tower][i] == 0) {
					index = i;
				}
				i++;
			}
			towers[tower][index] = disk;
		}
	}
	
	static boolean checkValid(int towerA, int towerB, int[][] towers) {
		boolean valid = true;
		
		// There is no disk in Tower A
		if(getTop(towerA, towers) == 0) valid = false;
		if(getTop(towerA, towers) < getTop(towerB, towers)) valid = false;
		
		return valid;
	}
	
	static int getTop(int tower, int towers[][]) {
		int top = towers[tower][0];
		
		for(int i=0; i<towers[tower].length; i++) {
			int current = towers[tower][i];
			if(current < top && current != 0) top = towers[tower][i];
		}
		return top;
	}
	
	static int[][] initTowers(int n) {
		// n represents the size of the largest disk;
		int[][] towers = new int[3][n];
		
		for(int i = 0; i < n; i++) {
			towers[0][i] = n-i;
		}
		
		return towers;
	}
	
	static boolean checkTowers(int[][] towers) {
		boolean result = true;
		
		for(int i=0; i<towers.length; i++) {
			for(int j=0; j<towers[i].length; j++) {
				if(i == 0 && towers[i][j] != 0) result = false;
				if(i == 1 && towers[i][j] != 0) result = false;
				if(i == 2) {
					if(towers[i][j] != towers[i].length - j) {
						result = false;
					}
				}
			}
		}
		
		return result;
	}
	
	static void printTowers(int[][] towers) {
		for(int j = 0; j < towers[0].length; j++) {
			for(int i = 0; i < towers.length; i++) {
				System.out.print("[ " + towers[i][j] + " ]");
			}
			System.out.println();
		}
	}
```

Attempt #2 in Java:
```
	static ArrayList<Stack<Integer>> towersOfHanoi(ArrayList<Stack<Integer>> towers) {
		if(checkTowers(towers)) {
			System.out.println("Found solution");
			return towers;
		}
		
		int towerA = 0;
		int towerB = 1;
		int towerC = 2;
		
		int[][] possibleMoves = 
				{{towerA, towerB}, {towerA, towerC},
				 {towerB, towerA}, {towerB, towerC},
				 {towerC, towerA}, {towerC, towerA}};
		
		ArrayList<int[]> validMoves = new ArrayList<int[]>();
		
		for(int i=0; i<possibleMoves.length; i++) {
			int tower1 = possibleMoves[i][0];
			int tower2 = possibleMoves[i][1];
			if(checkValid(tower1, tower2, towers)) {
				int[] move = {tower1, tower2};
				validMoves.add(move);
			}
		}
		
		for(int i=0; i<validMoves.size(); i++) {
			int[] move = validMoves.get(i);
			out.println("Move: "+move[0]+" "+move[1]);
			moveDisk(move[0], move[1], towers);
			printTowers(towers);
			towersOfHanoi(towers);
		}
		
		return towers;
	}
	
	// Move disk from towerA to towerB
	static void moveDisk(int towerA, int towerB, ArrayList<Stack<Integer>> towers) {
		Integer disk = towers.get(towerA).pop();
		towers.get(towerB).push(disk);
	}
	
	static boolean checkValid(int towerA, int towerB, ArrayList<Stack<Integer>> towers) {
		boolean valid = true;
		
		if(towers.get(towerA).empty()) valid = false;
		if(!towers.get(towerB).empty() && !towers.get(towerA).empty()) {
			if(towers.get(towerB).peek() < towers.get(towerA).peek()) valid = false;
		}
		
		return valid;
	}
	
	static boolean checkTowers(ArrayList<Stack<Integer>> towers) {
		boolean isSolution = true;
		
		if(!towers.get(0).empty()) isSolution = false;
		if(!towers.get(1).empty()) isSolution = false;
		// If the first and second tower are empty, all the disks are in the third tower
		return isSolution;
	}
	
	static ArrayList<Stack<Integer>> initTowers(int n) {
		// n represents the size of the largest disk;
		ArrayList<Stack<Integer>> towers = new ArrayList<Stack<Integer>>();
		towers.add(new Stack<Integer>());
		towers.add(new Stack<Integer>());
		towers.add(new Stack<Integer>());
		for(int i = 0; i < n; i++) {
			towers.get(0).push(n-i);
		}
		
		return towers;
	}
	
	static void printTowers(ArrayList<Stack<Integer>> towers) {
		for(int i = 0; i < towers.size(); i++) {
			Stack<Integer> temp = new Stack<Integer>();
			while(!towers.get(i).empty()) {
				temp.push(towers.get(i).pop());
			}
			out.print("Tower " + i + ": ");
			while(!temp.empty()) {
				int val = temp.pop();
				out.print("[ " + val + " ]");
				towers.get(i).push(val);
				System.out.print("\t");
			}

		}
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
- Sample output: ((())), (()()), (())(), ()(()), ()()()

for N = 1
()
- f(1) = ()

for N = 2
(())
()()
- f(2) = (f(1)) , f(1)(), ()(f(1))

for N = 3
((()))
(()())
()(())
(())()
()()()
- f(3) = (f(2)), f(2)(), ()(f(2))

for N = 4
(((())))
((()()))
((())())
((()))()
(()(()))
()((()))
(()())()
(())()()
()(())()
()()()()

Relation: 
- The permutation of n completely nested parentheses is:
    1. `()` nesting all permutations of n-1 parentheses
        - `(permutation(n-1))`
    2. `()` prepended to all permutations of n-1 parentheses
        - `()permutation(n-1)`
    3. `()` appended to all permutations of n-1 parentheses
        - `permutation(n-1)`
- If n = 1, the only permutation is '()'

Algorithm:
- Start with completely nested parentheses
- Take middle parentheses and move out left
- Then move out right, until it is not nested inside another parentheses
- Repeat for the other nested parentheses

What is a valid parenthesis pair?
- p = ( ) or ''
- p = \* ( \* ) \*
- \* means zero or more instances can occur

*A valid string of N parentheses pairs is one where there are N opening parentheses and N closing parentheses.*

Java:
```
	static ArrayList<String> parens(int n) {
		
		ArrayList<ArrayList<String>> permutations = new ArrayList<ArrayList<String>>();
		permutations.add(new ArrayList<String>());
		permutations.get(0).add("()");
		
		permutations.add(new ArrayList<String>());
		permutations.get(1).add("(())");
		permutations.get(1).add("()()");
		
		for(int i=2; i < n; i++) {
			ArrayList<String> newPerms = new ArrayList<String>();
			for(String s : permutations.get(i-1)) {
				String surrounded = "(" + s + ")";
				String prepended = "()" + s;
				String appended = s + "()";
				newPerms.add(surrounded);
				newPerms.add(prepended);
				newPerms.add(appended);
			}
			permutations.add(newPerms);
		}
		
		return permutations.get(n-1);		
	}
```
In JavaScript:
```
function parens(N) {
    let permutations = [];
    permutations.push(['()']);
    permutations.push(['(())', '()()']);

    for(let i = 2; i < N; i++) {
        let newPerms = [];

        let oldPerms = permutations[i-1];
        for(let j=0; j<oldPerms.length; j++) {
            newPerms.push(`(${oldPerms[j]})`);
            newPerms.push(`()${oldPerms[j]}`);
            newPerms.push(`${oldPerms[j]}())`);
        }
        permutations.push(newPerms);
    }

    return new Set(permutations[N-1]);
}
```

### 8.10 Paint Fill
> Implement the "paint fill" function that one might see on many image editing programs. That is, given a screen (represented by a two-dimensional array of colors), a point, and a new color, fill in the surrounding area until the color changes from the original color.

```
function init() {
    // Let's represent a color as a number from 0-255

    const width = 100;
    const height = 100;

    let image = new Array(width);

    for(let i = 0; i < height; i++) {
        image[i] = new Array(size);
    }

    const colorRange = 256;

    for(let i = 0; i < 100; i++) {
        for(let j = 0; j < 100; j++) {
            image[i][j] = getRandomInt(colorRange);
        }
    }

    const color = 10;
    let point = [Math.floor(width/2), Math.floor(height/2)];

    console.log(paintFill(image, point, color));
}

function paintFill(image, point, color) {
    // Base case

    if(point[0] < 0 || point [1] < 0 ||  point[0] >= image.length || point [1] >= image[0].length) {
        return;
    }

    let x = point[0];
    let y = point[1];

    if(image[x][y] !== color) {
        image[x][y] = color;
    }

    let newPoints = [[x,y-1], [x-1, y], [x+1, y], [x, y+1]];

    for(let i = 0; i < newPoints.length; i++) {
        paintFill(image, newPoints[i], color);
    }
}
// [Source](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random)

function getRandomInt(max) {
  return Math.floor(Math.random() * Math.floor(max));
}
```

### 8.11 Coins
> Given an infinite number of quarters (25 cents), dimes (10 cents), nickels (5 cents), and pennies (1 cent), write code to calculate the number of ways of representing n cents.

```
// Brute force implementation
function coins(n) {
    let total = coinsHelper(n, 0);
    console.log(`Total: ${total}`);
}

function coinsHelper(n, count) {
    if(n === count) {
        return 1;
    }
    if(count > n) {
        return 0;
    }

    return coinsHelper(n, count+25) + coinsHelper(n, count+10) + coinsHelper(n, count+5)  + coinsHelper(n, count+1);
}
```

### 8.12 Eight Queens - Incomplete
> Write an algorithm to print all ways of arranging eight queens on an 8x8 chess board so that none of them share the same row, column, or diagonal. In this case, "diagonal" means all diagonals, not just the two that bisect the board.

```
function init() {
    let board = new Array(8);
    for(let i = 0; i < board.length; i++) {
        board[i] = new Array(8);
    }
    for(let i = 0; i < board.length; i++) {
        for(let j = 0; j < board.length; j++) {
            board[i][j] = 0;
        }
    }
    for(let i = 0; i < board.length; i++) {
        board[0][i] = 1;
    }

    board = eightQueens(board);

}

function eightQueens(board) {
    // Algorithm
    // Base case
    // 1. If all queens are not violating any rules, return 1
}
```

### 8.13 Stack of Boxes - Incomplete
> You have a stack of n boxes, with widths w, heights h, and depths d. The boxes cannot be rotated and can only be stacked on top of one another if each box in the stack is strictly larger than the box above it in width, height, and depth. Implement a method to compute the height of the tallest possible stack. The height of a stack is the sum of the heights of each box.


```
class Box {
    constructor(width, height, depth) {
        this.width = width;
        this.height = height;
        this.depth = depth;
    }
}

function init() {
    let max = stackOfBoxes(n, boxes)
}
```

### 8.14 Boolean Evaluation - Incomplete
> Given a boolean expression consisting of the symbols 0 (false), 1 (true), & (AND), | (OR), and ^ (XOR), and a desired boolean result value `result`, implement a function to count the number of ways of parenthesizing the expression such that it evalutes to `result`.

```
function booleanEvaluation(expression, result) {

}

function parseExpression(expression) {

}
```