layout: post
title: Strict Python
date: '2018-03-29 00:00:00'

-------------------------------------------------------------------------------------------------------------------------

# Strict Python

Python is fun, but did you ever get the feeling after looking at other people code (or your past self) that maybe it's to much fun?

That is the feeling I get (almost) each time I'm looking at legacy code. But does Python expressivnes must become hindrance
to the future maintainer? Over the last few month, I started formulate for myself what make the code I'm reading harder to understand, 
and I feel like other Pythonistas might enjoy it as well. The core assumption is that your goal as a programmer is to **reduce the
amount of context**, or put in other words, **reduce the complexity of the mental model** needed to understand what is going on at 
any given point in the program. This may apply to programming in general, but even more to dynamically typed languages. 

### Dictionaries are not the best type to pass around
As much as I love using dictionaries, I really feel they are overused in most Python code I read. When a dictionary becomes a the 
langua franca of your program, i.e. functions are passing all-encompassing dictionaries from one another, know that your future 
self/maintainer is going to have a hard time understanding what each dictionary contains (even though you gave it a great name!).

The next time you consider passing a dictionary around think whether you can just pass the actual values, like:
Let's see some examples (from real code):

```python
def call(person):
    print(f"Calling {person['name']} number {person['number']}")  
```

Instead do:

```python
def call(name, number):
    print(f"Calling {name} number {number}")
```

Or use nametuple:
```python
Person = nametuple('Person', 'name number')
```

Or better yet utilize the brand new Python3.7 dataclass:

```python
@dataclass
class Person:
    name: str
    number: str
```

And if you are at it, maybe use Python's type annotations as well:
```
def call(person: Person):
```

Now this may seem silly to you, but imagine a small program with 100~ functions, that passes a dictionary with 10~ items.  
