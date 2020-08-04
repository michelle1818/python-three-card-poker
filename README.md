# Three Card Poker Game
A simplified version of the Three Card Poker game written in Python. This implementation relies on Python language features including object-oriented programming, mutable compound data types (such as lists and dictionaries), and moderately complex control flows to support various parts of the game logic.

## Card.py
### Classes
The file contains implementations of the following three classes representing entities found in any card game:
- Card: implements an abstraction of a playing card using two data attributes: rank and suit. The rank is an integer between 0 and 12, and is mapped to a string representation using the ranks list. The suit is an integer between 0 and 3, and is mapped to a string via the suits list. Note that both ranks and suits are class variables, which means they are available to all instances of the Card class as Card.ranks and Card.suits (see the code of the __str__() method for an example).
- Deck: implements an abstraction of the standard 52 (13 ranks × 4 suits) deck of playing cards. The deck is represented as a list cards of Card objects. It includes the implementation of the constructor (the __init__() method), the __str__() method, and a number of place-holders for other methods whose functionality is described via their doc strings.
- Hand: implements an abstraction of a hand in a card game. The Hand class is a subclass of Deck, which means that it inherits its cards attribute, and all its methods. It further refines the Deck class by adding the name attribute, which is a string holding the name of the person to whom the hand was dealt.

### Functions
- Card class:
    - get_suit(), a getter method for the suit attribute of the Card class. Hint: use the code of the get_rank() method as an example. 
- Deck class:
    - shuffle(): randomly shuffles the list stored in self.cards in place using the random.shuffle() method of the Python’s random library.
    - pop_cards(): takes a positive integer n as argument and removes the last n Card objects from the end of the self.cards list. Returns a new list instance comprised of the removed Card objects.
    - add_cards(): takes a list cards of Card objects as argument, and extends self.cards with a deep copy of the cards parameter. You can use the copy.deepcopy() method of the Python’s copy library to generate a deep copy of a list. Note that we need a deep rather than a shallow copy since the list elements are by themselves mutable objects.
    - clear_cards(): clears the content of self.cards. Be careful to NOT remove the entire self.cards attribute, which will happen e.g., if you use del self.cards.
    - move_cards(): takes an instance hand of the Hand object and an integer n as arguments, and uses the add_cards() and pop_cards() methods to move n cards from self to hand.
- Hand class:
    - get_name(): a getter method for the name attribute of the Hand class.


## PokerHand.py

### Comparing the Hands
The rules for comparing two hands in the game of Three-Hand Poker:
1. If the hand rankings are not equal, then the hand with a higher ranking wins.
2. If the hand rankings are the same, then the following tie-breaking procedure is applied:
    1. If the hands are either both Straight Flush or both Straight, then the highest rank of any card in one hand is compared to the highest rank of any card in the other hand. For the Straight Flush hands that include an Ace, the highest rank is 3 (not Ace!). Otherwise, the highest rank is the rank of a card with the highest rank. If the highest ranks are distinct, then the hand with a higher ranked card wins. Otherwise, it is a tie.
Examples: ”9 of Diamonds, 10 of Diamonds, Jack of Diamonds” beats ”7 of Spades, 8 of Spades, 9 of Spades”, ”Queen of Hearts, King of Diamonds, Ace of Clubs” beats ”Ace of Hearts, 2 of Diamonds, 3 of Spades”, and ”7 of Spades, 8 of Spades, 9 of Spades” and ”7 of Clubs, 8 of Clubs, 9 of Clubs” is a tie.
    2. If the hands are both Three of a Kind, then the rank of an arbitrary card in one hand is compared to the rank of an arbitrary card in the other hand, and the hand with a higher ranked card wins. Note that a tie is impossible in this case.
    3. If the hands are either both Flush or both Nothing, then each hand is sorted in the reverse order of their ranks, and the outcome is computed by comparing the resulting rank triples to each other in the lexicographical order. That is, the highest ranks in both hands are first compared to each other, and if they are distinct, then the hand with a higher ranked card wins. Otherwise, the second highest ranks are compared to each other, and so force. If all respective ranks in both hands are equal, it is a tie.
Examples: ”Ace of Diamonds”, ”10 of Diamonds”, ”2 of Diamonds” beats ”King of Spades”, ”Jack of Spades”, ”10 of Spades”.
    4. If both hands are Pairs, then the respective ranks of the paired cards are first compared to each other. If they are distinct, then the hand with a higher ranked card wins. Otherwise, the outcome is determined by comparing the ranks of the highest ranked cards in both hands.
Examples: ”3 of Diamonds, 3 of Clubs, 2 of Spades” beats ”2 of Clubs, 2 of Hearts, Ace of Spades”, ”2 of Clubs, 2 of Hearts, Ace of Spades” beats ”2 of Diamonds, 2 of Spades, King of Hearts”, ”3 of Diamonds, 3 of Clubs, 2 of Spades” and ”3 of Clubs, 3 of Spades, 2 of Hearts” is a tie.

## Poker.py

## Playing the Game 
This game is a simplified variant of Three-Card Poker in which a single player is playing against the dealer, and no bonus bets or bonus pay-offs are used. The game proceeds in a series of rounds. At the beginning of each round the player and the dealer are dealt three card each face-down. The player then places an ante bet without looking into his/her cards. After placing the ante bet, the player consults his/her hand, and decides whether to play or fold.

If the player folds, the ante bet is forfeited, and the game proceeds to the next round If the player plays, he/she places an additional play bet, which is equal to the ante bet. Both hands are then revealed. The dealer’s hand must be at least Queen-High to qualify for playing. If the dealer’s hand does not qualify, the player is paid 1:1 on the ante bet, and nothing on the play bet, which is then returned to the player. If the dealer’s hand qualifies, then the hands are compared to each other according to the rules listed in Question 3. If the player’s hand wins, the player is paid 1:1 on both the ante and the play bets. If the player’s hand loses, then the player forfeits both the ante and the play bets. If it is a tie, both ante and play bets are returned to the player without any extra pay. A good online resource to try an interactive game play is https://www.table-games-online.com/3-card-poker/ (make sure the pair-plus bet is set to 0).

The function play_round() in Poker.py plays a single round of Three-Card Poker. It takes the following as arguments:
- dealer_hand: an instance of ThreeCardPokerHand representing the dealer’s hand dealt at the beginning of the round.
- player_hand: an instance of ThreeCardPokerHand representing the player’s hand dealt at the beginning of the round.
- cash: a positive integer holding the amount of cash available to the player at the beginning of the round.
- get_ante: a function to be called to obtain the player’s ante bet. It takes cash as argument and returns a positive integer.
- is_playing: a function to be called to determine whether the player plays or folds. It takes player_hand as argument and returns True if the player plays and False otherwise.

play_round() starts by invoking get_ante() to get the player’s ante. It then calls is_playing() to determine if the player plays or folds. If the player folds, no further steps are taken, and the function returns. If the player plays, the dealer’s hand is compared against the minimum playing hand (available in min_dealer_hand) to determine if the dealer’s hand qualifies for playing. If it does, the player’s hand is compared against the dealer’s to determine the winner, or if it is a tie.

play_round() returns a tuple (ante, outcome) where the ante is the amount returned by get_ante(), and outcome is -1 if the player folds, 1 if the player plays and the dealer does not qualify, 2 if the dealer qualifies, and the player plays and wins, and -2 if the dealer qualifies, and the player plays and loses.

