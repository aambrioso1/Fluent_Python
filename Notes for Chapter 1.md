##### Chapter 1:  The Pythonic Data Model

# Code Examples

# 01-data-model

The class implemented in the first example creates a class to represent a deck of cards.    By implementing the dunder methods __len__ and __getitem__ the FrenchDeck class behaves like a standard Python sequence.   The deck can be indexed and sliced, its length can computed with len(), and random cards can be selected using the random.choice function to draw a random card.

Note that as a rule the code should not make calls to special methods.   It is usually better to use the related built-in function.

```
import collections

Card = collections.namedtuple('Card', ['rank', 'suit'])
class FrenchDeck:
    ranks = [str(n) for n in range(2, 11)] + list('JQKA')
    suits = 'spades diamonds clubs hearts'.split()

    def __init__(self):
        self._cards = [Card(rank, suit) for suit in self.suits
                                        for rank in self.ranks]

    def __len__(self):
        return len(self._cards)

    def __getitem__(self, position):
        return self._cards[position]

suit_values = dict(spades=3, hearts=2, diamonds=1, clubs=0)

def spades_high(card):
    rank_value = FrenchDeck.ranks.index(card.rank) # Note that index(element) is a built-in function
    return rank_value * len(suit_values) + suit_values[card.suit]
```
* Illustrates the power of the special dunder methods __getitem__ and __len__
* Illustrates the use of collections.nametuple.   Nametuple can be used to construct classes of objects that are
a collection of attributes.


Note that none of the special methods is directly called within the class.   "The Python interpreter is the only frequent caller of most special methods" [FP, p. 10]

The __repr__ special method is called to get the string representation of the object.

```
>>> v0, v1, v2
(Vector(0, 0), Vector(2, 4), Vector(5, 12))
```

Note the use of %r.   This makes sure the string representation will be executable.   Without it the representation would be Vector('3', '4') and arguments would be invalid string values.  The string representation should look like a call to the constructor of the class.

The __str__ constructor is returns a string suitable for display by the end users.   The author recommend this SO question for more information about the difference between __str__ and __repr__ :
"Difference between __str__ and __repr__ in Python"](http://bit.ly/1Vm7j1N)

Python documentation of the truth value testing rules in Python can be found here:
["Built-in Types"](http://docs.python.org/3/library/stdtypes.html/#truth)

The [Data Model](http://docs.python.org/3/reference/datamodel.html) chapter of the Python Language Reference includes a list of special methods.   FP has an overview on pp. 13-14.

Perhaps worth looking at ["The Art of the Metaobject Protocol"](https://www.amazon.com/Art-Metaobject-Protocol-Gregor-Kiczales/dp/0262610744)


### Example output for FrenchDeck class

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


```
from math import hypot

class Vector:

    def __init__(self, x=0, y=0):
        self.x = x
        self.y = y

    def __repr__(self):
        return 'Vector(%r, %r)' % (self.x, self.y)

    def __abs__(self):
        return hypot(self.x, self.y)

    def __bool__(self):
        return bool(abs(self))

    def __add__(self, other):
        x = self.x + other.x
        y = self.y + other.y
        return Vector(x, y)

    def __mul__(self, scalar):
        return Vector(self.x * scalar, self.y * scalar)
```

### Example output for Vector class

```
>>> from vector2d import *
>>> v0 = Vector()
>>> v1 = Vector(3,4)
>>> v2 = Vector(5, 12)
>>> v0, v1, v2
(Vector(0, 0), Vector(3, 4), Vector(5, 12))
>>> abs(v2)
13.0
>>> v1 + v2
Vector(8, 16)
>>> v0 + v2
Vector(5, 12)
>>> 4 * v2
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unsupported operand type(s) for *: 'int' and 'Vector'
>>> v2 * 4
Vector(20, 48)
>>> v3 = v1 * (1 / abs(v1))
>>> abs(v3)
1.0
>>> bool(v0)
False
>>> bool(v1)
True
```

