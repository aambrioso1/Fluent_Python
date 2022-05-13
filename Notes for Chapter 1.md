##### Chapter 1:  The Pythonic Data Model

# Code Examples

# 01-data-model

* Illustrates the power of the special dunder methods __getitem__ and __len__
* Illustrates the use of collections.nametuple.   Nametuple can be used to construnct class of objects that are
a collection of attributes.

The class implemented in the example [code]("\example-code\01-data-model\frenchdeck.py") creates a class to represent a deck of cards.    By implementing the dunder methods, the deck can be indexed and sliced.   Its length can computed with len() and random cards can be selected using the random.choice function to draw a random card.

### Example Output

```
>>> from frenchdeck import *
>>> suicide_king = Card('hearts', 'K')
>>> suicide_king
Card(rank='hearts', suit='K')
>>> deck = FrenchDeck()
>>> len(deck)
52
>>> for i in range(len(deck)):
...     deck[i]
...
Card(rank='2', suit='spades')
Card(rank='3', suit='spades')
Card(rank='4', suit='spades')
Card(rank='5', suit='spades')
Card(rank='6', suit='spades')
Card(rank='7', suit='spades')
Card(rank='8', suit='spades')
Card(rank='9', suit='spades')
Card(rank='10', suit='spades')
Card(rank='J', suit='spades')
...
>>> deck[-1]
Card(rank='A', suit='hearts')
>>> deck[0]
Card(rank='2', suit='spades')
>>> from random import choice
>>> choice(deck)
Card(rank='10', suit='hearts')
>>> choice(deck)
Card(rank='9', suit='clubs')
>>> for i in range(5):
...     choice(deck)
...
Card(rank='9', suit='spades')
Card(rank='4', suit='spades')
Card(rank='Q', suit='hearts')
Card(rank='A', suit='hearts')
Card(rank='A', suit='hearts')
>>> deck[:3]
[Card(rank='2', suit='spades'), Card(rank='3', suit='spades'), Card(rank='4', suit='spades')]
>>> deck[12::13]
[Card(rank='A', suit='spades'), Card(rank='A', suit='diamonds'), Card(rank='A', suit='clubs'), Card(rank='A', suit='hearts')]

>>> for card in reversed(deck):
...     print(card)
...
Card(rank='A', suit='hearts')
Card(rank='K', suit='hearts')
Card(rank='Q', suit='hearts')
Card(rank='J', suit='hearts')
Card(rank='10', suit='hearts')
Card(rank='9', suit='hearts')
Card(rank='8', suit='hearts')
Card(rank='7', suit='hearts')
Card(rank='6', suit='hearts')
Card(rank='5', suit='hearts')
Card(rank='4', suit='hearts')
...

>>> for card in reversed(deck):
...     print(card)
...
Card(rank='A', suit='hearts')
Card(rank='K', suit='hearts')
Card(rank='Q', suit='hearts')
Card(rank='J', suit='hearts')
...
Card(rank='6', suit='spades')
Card(rank='5', suit='spades')
Card(rank='4', suit='spades')
Card(rank='3', suit='spades')
Card(rank='2', suit='spades')

>>> Card('Q', 'hearts') in deck
True
>>> Card('7', 'Emeralds') in deck
False
>>> Card('10', 'clubs') in deck
True
>>> Card('11', 'clubs') in deck
False


>>> for card in sorted(deck, key=spades_high):
...     print(card)
...
Card(rank='2', suit='clubs')
Card(rank='2', suit='diamonds')
Card(rank='2', suit='hearts')
Card(rank='2', suit='spades')
Card(rank='3', suit='clubs')
Card(rank='3', suit='diamonds')
...
Card(rank='K', suit='spades')
Card(rank='A', suit='clubs')
Card(rank='A', suit='diamonds')
Card(rank='A', suit='hearts')
Card(rank='A', suit='spades')
```