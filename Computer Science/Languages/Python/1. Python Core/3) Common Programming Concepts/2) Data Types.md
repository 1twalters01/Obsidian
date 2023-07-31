# About Data Types
This will look at two data type subsets: scalar and compound.

Python is a dynamic programming language, meaning that you don't have to specify types. You can give type hints though they do not make any functional difference - they just cause error messages to show when writing code. They can actually slow code down slightly if you need to import them from the `types` module.

```python
a: int = 5
print(a)

b: int = 4.7 # Gives an error for 3rd party tools, but the code still works
print(b)
```

## The Types
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

## About Python's Dynamic Typing
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

# Scalar Types
## Numbers
Python supports `ints` and `floats`:

```python
a = 42 #int
b = 4.865 # float
```

* In addition to `int` and `float`, Python supports other types of numbers, such as`decimal` and `fraction`. 
* Python also has support for complex numbers, and uses the `j` or `J` suffix to indicate the imaginary part (e.g. `3+5j`).
* Python decides the type of number to use by itself.

* The standard Python implementation is written in C. This means that every Python object is simply a cleverly-disguised C structure, which contains not only its value, but other information as well.
### Numeric Operations
Python supports the basic mathematical operations you would expect for numbers.

```python
# addition
sum = 5 + 10
	
# subtraction
difference = 95.5 - 4.3

# multiplication
product = 4 * 30
	
# powers
2 ** 7  # 2 to the power of 7

# division
quotient = 17 / 3 # classic division returns a float (= 5.66666667)
quotient = 17 // 3 # floor division discards the fractional part (= 5)
	
# remainder
remainder = 43 % 5
```
## The Boolean Type
A Boolean has two possible values: `true` and `false`.

```python
t = True
```

## Text
In python, you can use either single (`''`), or double quotes (`""`) for text. There are some general standards though.
* Double quotes around strings used for interpolation or are natural language messages
* Single quotes for anything that behaves like an identifier
* Triple double quotes for docstrings

For example:

```python
LIGHT_MESSAGES = {
    'English': "There are %(number_of_lights)s lights.",
    'Pirate':  "Arr! Thar be %(number_of_lights)s lights."
}

def lights_message(language, number_of_lights):
    """Return a language-appropriate string reporting the light count."""
    return LIGHT_MESSAGES[language] % locals()

def is_pirate(message):
    """Return True if the given message sounds piratical."""
    return re.search(r"(?i)(arr|avast|yohoho)!", message) is not None
```

# Compound Types
Compound types can group multiple values into one type. The two most commonly used compound data types in Python are **lists** and **tuples**, though more exist.
## Lists
* A list is a collection of items that are enclosed in square brackets and separated by commas.
* Lists are mutable, which means that their elements can be modified after they are created.

```python
squares = [1, 4, 9, 16, 25]
```

## Tuples
* A tuple is similar to a list, but it is enclosed in parentheses
* Its elements cannot be modified after it is created.
```python
smoothie_ingredients = ("strawberry", "banana", "milk")
```

## Dictionaries
* A dictionary is a collection of key-value pairs, where each key is associated with a value.
* The keys and values are separated by a colon and enclosed in curly braces.
* Dictionaries are mutable, which means that the keys and values can be modified after the dictionary is created.

```python
test_scores = {
    Tyler: 100,
    Jules: 53,
    Kieran: 98
}
```

## Sets
* A Set is a collection of unique elements.
* They are mutable, which means that the elements can be added or removed after the set is created.
* They are enclosed in curly brackets `{}`.
	* Note that empty brackets `{}` denotes an empty dictionary, and not a set.

```python
my_languages = {Python, Rust, TypeScript, Kotlin, C, Swift}
my_languages = set([Python, Rust, TypeScript, Kotlin, C, Swift])
```

## FrozenSets
* A frozenset are a collection of unique elements.
* They are immutable.
* They are created using the following syntax: `frozenset(iterable)`

```python
mylist = ['apple', 'banana', 'cherry']  
x = frozenset(mylist)

x[1] = "strawberry" # This will fail
```