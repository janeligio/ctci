# IX. Interview Questions - Chapter 7 | Object-Oriented Design

### 7.1 Deck of Cards
> Design the data structures for a generic deck of cards. Explain how you would subclass the data structures to implement blackjack.

``` 
class Card {
    constructor(value, suit, face) {
        this.value = value;
        this.suit = suit;
        
        if(value === 1) {
            this.name = 'ace';
        } else if(face) {
            switch(face) {
                case 'jack':
                    this.name = 'jack';
                    break;
                case 'queen':
                    this.name = 'queen';
                    break;
                case 'king':
                    this.name = 'king';
                    break;
                default:
                    break;
            }
        } else {
            this.name = valueToName(value);
        }
    }

    get name() {
        let name = this.name;
        let suit = this.suit;
        return `${name} of ${suit}`;
    }

    valueToName(value) {
        let ret;
        switch(value) {
            case 1:
                ret = 'one';
                break;
            case 2:
                ret = 'one';
                break;
            case 3:
                ret = 'one';
                break;
            case 4:
                ret = 'one';
                break;
            case 5:
                ret = 'one';
                break;
            case 6:
                ret = 'one';
                break;
            case 7:
                ret = 'one';
                break;
            case 8:
                ret = 'one';
                break;
            case 9:
                ret = 'one';
                break;
        }
    }
}

class DeckOfCards {
    constructor() {
        this.deck = init();
    }

    // Randomize order of cards
    shuffle() {
        // To implement
    }

    // Draw a number of cards from top of deck
    drawCards(numCards) {
        let cardsDrawn = [];
        for(let i = 0; i < numCards; i++) {
            if(this.deck.length === 0) break;
            else cardsDrawn.push(this.deck.pop());
        }
    }

    // Initialize deck of cards in order as if it was a fresh deck
    init() {
        const suits = ['hearts', 'spades', 'clubs', 'diamonds'];
        const nonFaceCards = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
        const faceCards = ['jack', 'queen', 'king'];
        const FACE_CARD_VALUE = 10;

        let cards = [];

        for(const suit in suits) {
            // Insert non-face cards
            for(let value in nonFaceCards) {
                cards.push(new Card(value, suit));
            }
            // Insert face cards
            for(let value in faceCards) {
                cards.push(new Card(FACE_CARD_VALUE, suit, value));
            }
        }

        return cards;
    }
}
```

- To implement these to classes into a blackjack game, I would create another BlackJackGame class. It would have an instance of a DeckOfCards in which its necessary functions would already exist. That is, drawing cards and shuffling cards. The values of cards can be obtained from Card objects in the DeckOfCards. 

### 7.2 Call Center
> Imagine you have a call center with three levels of employees: respondent, manager, and director. An incoming telephone call must be first allocated to a respondent who is free. If the respondent can't handle the call, he or she must escalate the call to a manager. If the manager is not free or not able to handle it, then the call should be escalated to a director. Design the classes and data structures for this problem. Implement a method dispatchCall() which assigns a call to the first available employee.

```
class CallCenter {
    constructor() {
        this.respondents = [];
        this.managers = [];
        this.directors = [];
        this.dispatchedEmployees = [];
    }

    dispatchCall() {
        const RESPONDENT = 'Respondent';
        const MANAGER = 'Manager';
        const DIRECTOR = 'Director';

        if(this.respondents.length > 0) {
            assignCall(RESPONDENT);
        } else if(this.managers.length > 0) {
            assignCall(MANAGER);
        } else if(this.directors.length > 0) {
            assignCall(DIRECTOR);
        } else {
            console.log("No available employees.");
        }
    }

    assignCall(typeEmployee) {
        switch(typeEmployee) {
            case 'Respondent':
                this.dispatchedEmployees.push(this.respondents.pop());
                break;
            case 'Manager':
                this.dispatchedEmployees.push(this.managers.pop());
                break;
            case 'Director':
                this.dispatchedEmployees.push(this.directors.pop());
                break;
            default: 
                break;
        }
    }
}

class Employee {
    constructor(level, id) {
        this.level = level;
        this.id = id;
    }
}
```

### 7.3 Jukebox
> Design a musical jukebox using object-orient principles.

```
class Jukebox {
    constructor(songs) {
        this.songs = [...songs];
        this.currentSelection = 0;
        this.queue = [];
        this.currentSong = null;
    }
    displaySelection() {
        console.log(this.songs[this.currentSelection]);
    }
    nextSelection() {
        if(this.currentSelection === this.songs.length - 1) {
            this.currentSelection = 0;
        } else {
            this.currentSelection = this.currentSelection + 1;
        }
    }
    previousSelection() {
        if(this.currentSelection === 0) {
            this.currentSelection = this.songs.length - 1;
        } else {
            this.currentSelection = this.currentSelection - 1;
        }
    }
    queueSong() {
        this.queue.push(this.songs[this.currentSelection]);
    }
    playSong() {
        if(this.queue > 0) {
            this.currentSong = this.queue[0];
            this.queue.shift();
            this.currentSong.play();
        } else {
            console.log("No songs in queue.");
        }
    }
}
```

### 7.4 Parking Lot
> Design a parking lot using object-orient principles.

```
class ParkingLot {
    constructor(numSpots) {
        this.spots = new Array(numSpots);
        this.numOccupied = 0;
        for(let i = 0; i < numSpots; i++) {
            this.spots[i] = new ParkingSpot(i+1);
        }
    }

    parkCar(car, spot) {
        if(this.numOccupied === this.spots.length) {
            console.log("Lot full.");
        } else if(this.spots[spot].isOccupied()) {
            let id = this.spots[spot].getId();
            console.log("Spot #${id} taken.");
        } else {
            this.spots[spot]
                .toggleOccupied()
                .assignCar(car);
            this.numOccupied = this.numOccupied + 1;
        }
    }
    unparkCar(spot) {
        if(!this.spots[spot].isOccupied()) {
            let id = this.spots[spot].getId();
            console.log(`"No car is parked here in spot #${id}`)
        } else {
            this.spots[spot].toggleOccupied().assignCar(null);
        }
    }
}

class ParkingSpot {
    constructor(id) {
        this.id = id;
        this.occupied = false;
        this.car = null;
    }
    toggleOccupied() {
        this.occupied = !this.occupied;
    }
    assignCar(car) {
        this.car = car;
    }
    isOccupied() {
        return this.occupied;
    }
    getId() {
        return this.id;
    }
}

class Car {
    constructor(make, model, color, year, plateNumber) {
        this.make = make;
        this.model = model;
        this.color = color;
        this.year = year;
        this.plateNumber = plateNumber;
    }
}
```

### 7.5 Online Book Reader - Incomplete
> Design the data structures for an online book reader system.

### 7.6 Jigsaw - Incomplete
> Implement an NxN jigsaw puzzle. Design the data structures and explain an algorithm to solve the puzzle. You can assume that you have a `fitsWith method which, when passed two puzzle edges, returns true if the two edges belong together.

class JigsawPuzzle {
    constructor(n) {
        this.puzzle = new Array(n);
        for(let i = 0; i < n; i++) {
            this.puzzle[i] = new Array(n);
        }
        this.solved = false;
    }
    placePiece() {

    }
    removePiece() {

    }
}

class PuzzlePiece {
    constructor() {

    }
}

### 7.7 Chat Server
> Explain how you would design a chat server. In particular, provide details about the various backend components, classes, and methods. What would be the hardest problems to solve?

**Backend**
- A server that the client receives all its data from
- Connected to a database server that stores all the data
    1. User
        - unique id
        - username
        - email
        - password
        - friends list
        - profile picture
    2. Messsage
        - sender id
        - recipient(s') id
        - conversation id (conversation it belongs to)
        - message
        - date
    3. Conversation
        - conversation id
        - members' ids
        - messages
- Actions
    - Authenticate user given a username/email and password
    - Fetch conversations
        - Within each conversation, fetch messages
    - Search user
    - Send message
    - Start conversation
    - Delete conversation

**Frontend**
1. User authentication
    - user must register for an account
    - user must then use its credentials to authenticate
    - user is then granted access to its conversations
    - user can log out atnd invalidate session
2. Actions
    - Start conversation
        - Search for a user and start conversation by sending message
    - Accept a conversation
    - Send message
    - Add friends
    - View conversations
    - Delete conversations

### 7.8 Othello
> Othello is played as follows: Each Othello piece is white on one side and black on the other. When a piece is surrounded by its opponents on both the left and right sides, or both the top and bottom, it is said to be captured and its color is flipped. On your turn, you must capture at least one of your opponent's pieces. The game ends when either user has no more valid moves. The win is assigned to the person with the most pieces. Implement the object-oriented design for Othello.

*Design*

1. Piece
    - Member variables
        1. Color (black/white)
    - Actions
        1. Flip: change to opposite color

2. The Game
    - Member variables
        1. Number of white pieces
        2. Number of black pieces
        3. Boolean - is game finished
        4. Turn - the color whose turn it is
    - Actions
        1. Start game
            - Allow ability to flip pieces
        2. Flip a specific piece
            - Check if valid flip
                - only valid if the piece to flip is opposite color
                - only valid if a flip results in a capture
            - If valid flip
                - Correctly increment/decrement number of white/black pieces
                - Change turns to the opposite color
            - If not valid, then deny move
            - If no valid moves are available, end game
            - Once game ends, check total of black and white pieces
            - Congratulate color who had more pieces

### 7.9 Circular Array - Incomplete
> Implement a `CircularArray` class that supports an array-like data structure which can be efficiently rotated. If possible, the class should use a generic type (also called a template), and should support iteration via the standard `for (Obj o : circularArray)` notation. 
```
class CircularArray {
    constructor(length) {
        this.arr = new Array(length);
        this.length = length;
    }
    get(index) {
        if(index > this.length || index < 0) {
            throw error;
        } else {
            return this.arr[index];
        }
    }
    set(index) {
        if(index < 0) {
            throw error;
        } else if(index > this.length) {
            const newLength = this.length * 2;
            let newArray = new Array(newLength);
            for(let i = 0; i < )
        }
    }
}
```

### 7.10 Minesweeper
> Design and implement a text-based Minesweeper gamae. Minesweeper is the class single-player computer game where an NxN grid has B mines (or bombs) hidden across the grid. The remaining cells are either blank or have a number behind them. The numbers reflect the number of bombs in the surrounding eight cells. The user then uncovers a cell. If it is a bomb, the player loses. If it is a number, the number is exposed. If it is a blank cell, this cell and all adjacent black cells (up to and including the surrounding numeric cells) are exposed. The player wins when all non-bomb cells are exposed. The player can also flag certain places as potential bombs. This doesn't affect game play, other than to block the user from accidentally clicking a cell that is thought to have a bomb. (Tip for the reader: if you're not familiar with this game, please play a few rounds online first.)

```
class Minesweeper {
    const NOT_STARTED = 0;
    const STARTED = 1;

    constructor(N, B) {
        this.board = init(N, B);
        this.gameState = NOT_STARTED;
    }

    startGame() {
        this.gameState = STARTED;
    }

    endGame() {
        this.gameState = NOT_STARTED;
    }

    pickCell(x, y) {
        const cellVal = this.board[x][y].get();
        if(cellVal === 'B') {
            // cell was a mine, end game;
            endGame();
        } else if(cellVal !== '') {
            // If it is a cell with a number
            this.board[x][y].reveal();
        } else {
            revealSurroundingCells(x, y);
        }
    }

    revealCell(x, y) {
        this.board[x][y].reveal();
    }

    revealSurroundingCells(x, y) {
        // Algorithm:
        // Run breadth-first search from an indicated cell location,
        // when a cell is encountered and it is not a bomb, mark as visited and reveal cell
        // If a cell is a bomb, return
        // Run until contiguous cells are revealed
    }

    init(N, B) {
        let board = new Array(N);
        for(let i = 0; i < N; i++) {
            board[i].new Array(N);
        }
        board = placeCells(board);
        board = placeBombs(B); // 
        board = 
    }

    placeCells(board) {
        let newBoard = board;
        for(let i = 0; i < newBoard.length; i++) {
            for(let j = 0; j < newBoard[i].length; j++) {
                newboard[i][j] = new Cell('');
            }
        }
        return newBoard;
    }

    placeBombs(B) {
        // Randomly place B bombs
    }

    placeValues(board) {
        let newBoard = [...board];
        for(let i = 0; i < newBoard.length; i++) {
            for(let j = 0; j < newBoard[i].length; j++) {
                let cell = newBoard[i][j].get();
                if(cell !== 'B') {
                    // If not a bomb
                    newBoard[i][j].set(numAdjacentBombs(newBoard, i, j));
                }
            }
        }
    }

    numAdjacentBombs(board, x, y) {
        // Get number of bombs of a cell given its coordinates and the board
    }
}
```
```
class Cell {
    constructor(value) {
        this.value = value;
        this.hidden = true;
    }

    get() {
        return this.value;
    }

    set() {
        this.value = value;
    }

    reveal() {
        this.hidden = false;
    }
}
```

### 7.11 File System - Incomplete
> Explain the data structures and algorithms that you would use to design an in-memory file system. Illustrate with an example in code where possible.

### 7.12 Hash Table
> Design and implement a hash table which uses chaining (linked lists) to handle collisions.

```
class HashTable {
    constructor(size) {
        this.arr = new Array(size) {
        this.length = size;
        }
    }

    add(obj) {
        let index = hash(obj);
        if(this.arr[index] === undefined) {
            this.arr[index] = new LinkedList(obj);
        } else {
            this.arr[index].add(obj);
        }
    }

    get(obj) {
        let index = hash(obj);
        let ret = this.arr[index].get(obj);
        if(this.arr[index].length === 0) {
            this.arr[index] = null; // Remove linked list if that linked list is now empty
        }
        return ret;
    }

    hash(obj) {
        obj.hashCode() % this.length;
    }
}
```
```
class LinkedList {
    constructor(initialValue) {
        this.length = 1;
        this.head = new Node(initialValue);
    }

    add(value) {
        let newNode = new Node(value);
        if(this.head === null) {
            this.head = newNode;
        } else {
            // Insert at end of linked list
            let currentNode = this.head;
            while(currentNode.next !== null) {
                currentNode = currentNode.next;
            }
            currentNode.next = newNode;
        }
    }

    get(value) {
        let ret;
        if(this.head === null) {
            console.log("Error");
        } else if(this.head.val() === value) {
            ret = this.head.val();
            this.head = this.head.next;
        } else {
            let currentNode = this.head;
            while(currentNode !== null) {
                if(currentNode.next.val() === value) {
                    ret = currentNode.next.val();
                    currentNode.next = currentNode.next.next;
                    return ret;
                } else {
                    currentNode = currentNode.next;
                }
            }
        }
    }
}
```
```
class Node {
    constructor(value) {
        this.value = value;
        this.next = null;
    }
    
    val() {
        return this.value;
    }
}
```
```
// Source: https://werxltd.com/wp/2010/05/13/javascript-implementation-of-javas-string-hashcode-method/

String.prototype.hashCode = function(){
	var hash = 0;
	if (this.length == 0) return hash;
	for (i = 0; i < this.length; i++) {
		char = this.charCodeAt(i);
		hash = ((hash<<5)-hash)+char;
		hash = hash & hash; // Convert to 32bit integer
	}
	return hash;
}
```