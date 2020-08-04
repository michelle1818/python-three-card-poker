# Three Card Poker Game
A simplified version of the Three Card Poker game written in Python 

## Card.py

## PokerHand.py

### Comparing the Hands
Below are the rules for comparing two hands in the game of Three-Hand Poker:
1. If the hand rankings are not equal, then the hand with a higher ranking wins.
2. If the hand rankings are the same, then the following tie-breaking procedure is applied:
(a) If the hands are either both Straight Flush or both Straight, then the highest rank of any card in one hand is compared to the highest rank of any card in the other hand. For the Straight Flush hands that include an Ace, the highest rank is 3 (not Ace!). Otherwise, the highest rank is the rank of a card with the highest rank. If the highest ranks are distinct, then the hand with a higher ranked card wins. Otherwise, it is a tie.
Examples: ”9 of Diamonds, 10 of Diamonds, Jack of Diamonds” beats ”7 of Spades, 8 of Spades, 9 of Spades”, ”Queen of Hearts, King of Diamonds, Ace of Clubs” beats ”Ace of Hearts, 2 of Diamonds, 3 of Spades”, and ”7 of Spades, 8 of Spades, 9 of Spades” and ”7 of Clubs, 8 of Clubs, 9 of Clubs” is a tie.
(b) If the hands are both Three of a Kind, then the rank of an arbitrary card in one hand is compared to the rank of an arbitrary card in the other hand, and the hand with a higher ranked card wins. Note that a tie is impossible in this case.
(c) If the hands are either both Flush or both Nothing, then each hand is sorted in the reverse order of their ranks, and the outcome is computed by comparing the resulting rank triples to each other in the lexicographical order. That is, the highest ranks in both hands are first compared to each other, and if they are distinct, then the hand with a higher ranked card wins. Otherwise, the second highest ranks are compared to each other, and so force. If all respective ranks in both hands are equal, it is a tie.
Examples: ”Ace of Diamonds”, ”10 of Diamonds”, ”2 of Diamonds” beats ”King of Spades”, ”Jack of Spades”, ”10 of Spades”.
(d) If both hands are Pairs, then the respective ranks of the paired cards are first compared to each other. If they are distinct, then the hand with a higher ranked card wins. Otherwise, the outcome is determined by comparing the ranks of the highest ranked cards in both hands.
Examples: ”3 of Diamonds, 3 of Clubs, 2 of Spades” beats ”2 of Clubs, 2 of Hearts, Ace of Spades”, ”2 of Clubs, 2 of Hearts, Ace of Spades” beats ”2 of Diamonds, 2 of Spades, King of Hearts”, ”3 of Diamonds, 3 of Clubs, 2 of Spades” and ”3 of Clubs, 3 of Spades, 2 of Hearts” is a tie.

## Poker.py

## Playing the Game 
You will implement a simplified variant of Three-Card Poker in which a single player is playing against the dealer, and no bonus bets or bonus pay-offs are used. The game proceeds in a series of rounds. At the beginning of each round the player and the dealer are dealt three card each face-down. The player then places an ante bet without looking into his/her cards. After placing the ante bet, the player consults his/her hand, and decides whether to play or fold.
If the player folds, the ante bet is forfeited, and the game proceeds to the next round If the player plays, he/she places an additional play bet, which is equal to the ante bet. Both hands are then revealed. The dealer’s hand must be at least Queen-High to qualify for playing. If the dealer’s hand does not qualify, the player is paid 1:1 on the ante bet, and nothing on the play bet, which is then returned to the player. If the dealer’s hand qualifies, then the hands are compared to each other according to the rules listed in Question 3. If the player’s hand wins, the player is paid 1:1 on both the ante and the play bets. If the player’s hand loses, then the player forfeits both the ante and the play bets. If it is a tie, both ante and play bets are returned to the player without any extra pay. A good online resource to try an interactive game play is https://www.table-games-online.com/3-card-poker/ (make sure the pair-plus bet is set to 0).

The function play_round() in Poker.py plays a single round of Three-Card Poker. It takes the following as arguments:
- dealer_hand: an instance of ThreeCardPokerHand representing the dealer’s hand dealt at the beginning of the round.
- player_hand: an instance of ThreeCardPokerHand representing the player’s hand dealt at the beginning of the round.
- cash: a positive integer holding the amount of cash available to the player at the beginning of the round.
- get_ante: a function to be called to obtain the player’s ante bet. It takes cash as argument and returns a positive integer.
- is_playing: a function to be called to determine whether the player plays or folds. It takes player_hand as argument and returns True if the player plays and False otherwise.

play_round() starts by invoking get_ante() to get the player’s ante. It then calls is_playing() to determine if the player plays or folds. If the player folds, no further steps are taken, and the function returns. If the player plays, the dealer’s hand is compared against the minimum playing hand (available in min_dealer_hand) to determine if the dealer’s hand qualifies for playing. If it does, the player’s hand is compared against the dealer’s to determine the winner, or if it is a tie.

play_round() returns a tuple (ante, outcome) where the ante is the amount returned by get_ante(), and outcome is -1 if the player folds, 1 if the player plays and the dealer does not qualify, 2 if the dealer qualifies, and the player plays and wins, and -2 if the dealer qualifies, and the player plays and loses.

