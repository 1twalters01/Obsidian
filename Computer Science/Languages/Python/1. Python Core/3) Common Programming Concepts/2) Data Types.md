This will look at two data type subsets: scalar and compound.

Python is a dynamic programming language, meaning that you don't have to specify types. You can give type hints though they do not make any functional difference - they just cause error messages to show when writing code. They can actually slow code down slightly if you need to import them from the `types` module.

```python
a: int = 5
print(a)

b: int = 4.7 # Gives an error for 3rd party tools, but the code still works
print(b)
```

# The Types
Python has the following primitive data types:
* Numeric Types: `int`, `float`, `complex`
* Boolean Type: `bool`
* Text Type: `str`
* Sequence Types: `list`, `tuple`, `range`
* Mapping Type: `dict`
* Set Types: set, `frozenset`
* Binary Types: bytes, `bytearray`, `memoryview`
* None Type: `NoneType`

The `NoneType` type has to be imported from the `types` module to be used.

# About Python's Dynamic Typing
The standard Python implementation is written in C. This means that every Python object is simply a cleverly-disguised C structure, which contains not only its value, but other information as well.

When we define an integer in Python, such as `x = 10000`, `x` is not just a "raw" integer. It's actually a pointer to a compound C structure, which contains several values.

A single integer in Python 3.4 actually contains four pieces:
* `ob_refcnt`, a reference count that helps Python silently handle memory allocation and deallocation
- `ob_type`, which encodes the type of the variable
- `ob_size`, which specifies the size of the following data members
- `ob_digit`, which contains the actual integer value that we expect the Python variable to represent.

This means that there is some overhead in storing an integer in Python as compared to an integer in a compiled language like C.

* A C integer is essentially a label for a position in memory whose bytes encode an integer value.
* A Python integer is a pointer to a position in memory containing all the Python object information, including the bytes that contain the integer value.
* This extra information in the Python integer structure is what allows Python to be coded so freely and dynamically.
* All this additional information in Python types comes at a cost to speed and memory.

# Numbers
Python supports `ints` and `floats`:

```python
a = 42 #int
b = 4.865 # float
```

* In addition to `int` and `float`, Python supports other types of numbers, such as`decimal` and `fraction`. 
* Python also has support for complex numbers, and uses the `j` or `J` suffix to indicate the imaginary part (e.g. `3+5j`).
* Python decides the type of number to use by itself.

* The standard Python implementation is written in C. This means that every Python object is simply a cleverly-disguised C structure, which contains not only its value, but other information as well.

# The Boolean Type
A Boolean has two possible values: `true` and `false`.

```python
t = True
```

# Text


## Lists


## Compound Types