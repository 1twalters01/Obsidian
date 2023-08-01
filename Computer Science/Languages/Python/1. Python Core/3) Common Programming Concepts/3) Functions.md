* The `def` keyword allows you to declare new functions.
* The statements that form the body of the function start at the next line, and must be indented.
* Python uses *snake_case* as the conventional style for function and variable names. Here, all letters are lowercase and underscores separate words.
* We can call any function we've defined by entering its name followed by a set of parentheses.
* The first statement of the function body can optionally be a string literal; this string literal is the function’s documentation string, or _docstring_.
* functions without a return statement return the value `None`.
```python
def fib(n):    # write Fibonacci series up to n
    """Print a Fibonacci series up to n."""
    a, b = 0, 1
    while a < n:
        print(a, end=' ')
            a, b = b, a+b
    print()

fib(2000)
print(fib(0))
```

The use case will determine which parameters to use in the function definition:
```python
def f(pos1, pos2, /, pos_or_kwd, *, kwd1, kwd2):
```

# Lambda Expressions
Small anonymous functions can be created with the lambda keyword.
Semantically, they are just syntactic sugar for a normal function definition.

```python
def make_incrementor(n):
    return lambda x: x + n
```