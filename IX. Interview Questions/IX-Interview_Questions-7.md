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