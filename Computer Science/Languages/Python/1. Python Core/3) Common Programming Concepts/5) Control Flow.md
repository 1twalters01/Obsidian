The most common constructs that let you control the flow of execution of Python code are if expressions and loops, though other ones, like match, exist.
# If/Elif/Else
You do an if statement as follows:

```python
number = 3;
if number % 4 == 0 :
	print("number is divisible by 4")
elif number % 3 == 0:
	print("number is divisible by 3")
elif number % 2 == 0:
	print("number is divisible by 2")
else
	print("number is not divisible by 4, 3, or 2")
```

If you’re comparing the same value to several constants, or checking for specific types or attributes, you may also find the `match` statement useful.

# Loops
## For loops
Rather than always iterating over an arithmetic progression of numbers (like in Pascal), or giving the user the ability to define both the iteration step and halting condition (like with C), Python’s `for` statement iterates over the items of any sequence (a list or a string), in the order that they appear in the sequence. For example:

```python
words = ['cat', 'window', 'defenestrate']
for w in words:
    print(w, len(w))
```

Code that modifies a collection while iterating over that same collection can be tricky to get right. Instead, it is usually more straight-forward to loop over a copy of the collection or to create a new collection.

## Range
The built-in function `range` is used if you need to iterate over a sequence of numbers. It generates arithmetic progressions. The given end point is never part of the generated sequence. For example:

```python
for i in range(5):
     print(i) # Will print 0, 1, 2, 3 and 4
```

The range function's full syntax is `range(start, end, step)`:
```python
print(list(range(0, 10, 3))) # Will print the numbers 0, 3, 6, and 9
```

To iterate over the indices of a sequence, you can combine `range` and `len()`, though enumerate() is usually more convenient:
```python
a = ['Mary', 'had', 'a', 'little', 'lamb']
for i in range(len(a)):
    print(i, a[i])

for item, index in enumerate(a)
    print(item, index)
```

If you print something like `range(10)` you will get back an iterable object.
## While loops
While the condition for the while loop is `true`, the loop runs. When the condition ceases to be `true`, the program calls `break`, stopping the loop.

```python
a = 0
while a < 10
    print(a)
    a += 1
```

## Break, else and Continue Statements
The break statement breaks out of the innermost enclosing for or while loop.

A `for` or `while` loop can include an `else` clause.
* In a for loop, the `else` clause is executed after the loop reaches its final iteration.
* In a while loop, it’s executed after the loop’s condition becomes false.
*  The `else` clause is **not** executed if the loop was terminated by a `break` in either case.

For example, when searching for prime numbers:

```python
for n in range(2, 10):
    for x in range(2, n):
        if n % x == 0:
            print(n, 'equals', x, '*', n//x)
                break
    else:
    # loop fell through without finding a factor
    print(n, 'is a prime number')
```
The `continue` statement continues with the next iteration of the loop:

```python
for num in range(2, 10):
    if num % 2 == 0:
        print("Found an even number", num)
        continue
    print("Found an odd number", num)
```
## Pass
The `pass` statement does nothing. It can be used when a statement is required syntactically but the program requires no action. For example:
```python
while True:
    pass # Wait for ctrl+C to be pressed
```

This is commonly used for creating minimal classes:
```python
class MyEmptyClass:
    pass
```

It can also be used as a placeholder for a function or conditional body:
```python
def initlog(*args):
    pass   # Remember to implement this!
```

# Match Statements
The simplest form of a match statement compares a subject value against one or more literals. You can combine several literals in a single pattern using `|` (“or”). For example:
```python
def http_error(status):
    match status:
        case 400:
            return "Bad request"
        case 401 | 403 | 404:
            return "Not allowed"
        case 404:
            return "Not found"
        case 418:
            return "I'm a teapot"
        case _:
            return "Something's wrong with the internet"
```
`_` acts as a _wildcard_ and never fails to match.

Patterns can look like unpacking assignments, and can be used to bind variables:
```python
# point is an (x, y) tuple
match point:
    case (0, 0):
        print("Origin")
    case (0, y):
        print(f"Y={y}")
    case (x, 0):
        print(f"X={x}")
    case (x, y):
        print(f"X={x}, Y={y}")
    case _:
        raise ValueError("Not a point")
```

If you are using classes to structure your data you can use the class name followed by an argument list resembling a constructor:
```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

def where_is(point):
    match point:
        case Point(x=0, y=0):
            print("Origin")
        case Point(x=0, y=y):
            print(f"Y={y}")
        case Point(x=x, y=0):
            print(f"X={x}")
        case Point():
            print("Somewhere else")
        case _:
            print("Not a point")
```

We can add an `if` clause to a pattern, known as a “guard”. If the guard is false, `match` goes on to try the next case block. Note that value capture happens before the guard is evaluated:
```python
match point:
    case Point(x, y) if x == y:
        print(f"Y=X at {x}")
    case Point(x, y):
        print(f"Not on the diagonal")
```

Patterns may use named constants. These must be dotted names to prevent them from being interpreted as capture variable:
```python
from enum import Enum
class Color(Enum):
    RED = 'red'
    GREEN = 'green'
    BLUE = 'blue'

color = Color(input("Enter your choice of 'red', 'blue' or 'green': "))

match color:
    case Color.RED:
        print("I see red!")
    case Color.GREEN:
        print("Grass is green")
    case Color.BLUE:
        print("I'm feeling the blues :(")
```