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

