The equal sign (`=`) is used to assign a value to a variable:
```python
x = 5
```

If a variable is not “defined” (assigned a value), trying to use it will give you an error.

Nothing in Python enforces immutability in variables. There are immutable datatypes, like strings, however the pointers to these get overwritten whenever you assign a new thing to the variable that had the string pointer.

# Shadowing
* You can declare a new variable within the same name as a previous variable.
* The second variable is what the compiler will see when you use the name of the variable until it itself is shadowed or the scope ends.
```python
# global variable
a = 2
a = a + 1

def fn():
  # local variable
  a = 5
  print(a) # prints 5

fn()
print(a) # prints 3
```

Upon running this program we will see 5 and then 3