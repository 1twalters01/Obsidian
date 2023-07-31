In Python, comments start with a `#`. The comment will continue until the next line.
Python does not really have a syntax for multiline comments.
To add a multiline comment you could insert a `#` for each line.

```python
# One comment here
# Another comment here
```

Or, not quite as intended, you can use a multiline string.

Python will ignore string literals that are not assigned to a variable, so you can add a multiline string (triple quotes) in your code, and place your comment inside it,
like this:

```python
"""
Insert comments
And more
And even more
"""
```

 The triple quotes are called docstrings. Docstrings are strings used to document a Python module, class, function or method, so programmers can understand what it does without having to read the details of the implementation.
 
 They are discussed in [[Publishing a package]].