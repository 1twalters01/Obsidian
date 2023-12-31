# Creating Files
* Python files always end with the `.py` extension.
* Modules (filenames) should have short, all-lowercase names, and they can contain underscores;
	 Use `hello_world.py` rather than `helloWorld.py`

# Example
Put the following into a file called *example.py*:

```python
for i in range(3):
    print("Hello World")
```

Then type `python example.py` or `python3 example.py` if you also have python 2.0 installed into a terminal.
This should print to the terminal 3 times.

# Example Analysis
* PEP 8 is the style guide for python code.
	* This is important as code is read more often than it is written, and formatting helps with this.
	* You can use Python Black to format your code

# Running
Python is an interpreted programming language, meaning that someone needs to have python installed to run files sent to them, unlike something like Rust or C.

To run a python file type `python path-to-script.py` or `python3 path-to-script.py` if both python 2 and python 3 are installed on the machine.