All programs have to manage the way they use a computer's memory while running. Some have garbage collection that regularly looks for no longer used memory as the program runs. In other languages, the programmer must explicitly allocate and free the memory. Rust manages memory through a system of ownership with a set of rules that, if broken, will mean that the program won't compile. None of the features of ownership will slow down your program while it's running.

Ownership lets Rust make memory safety guarantees without needing a garbage collector, meaning it is important to understand how it works.

# Ownership rules
1) Each value in Rust has an Owner.
2) There can only be one owner at a time.
3) When the owner goes out of scope, the value will be dropped.

# The Stack and the Heap
In systems programming languages like Rust, whether a value is on the stack or the heap affects how the language behaves and why you have to make certain decisions. The stack and the heap are parts of memory available to your code to use at runtime, but they are structured in different ways.

The stack stores values in the order it gets them and removes it in the opposite order.
* This is referred to as last in, first out.
* Adding data is called *pushing onto the stack*
* Removing data is called *popping off the stack*.
* All data stored on the stack must have a known, fixed size.
* Data with an unknown size at compile time or a size that might change must be stored on the heap instead.

The heap is less organised: when you put data on the heap, you request a certain amount of space.

# Variable Scope
# The string Type
# Memory Allocation
# Variables and Data Interacting with Move
# Variables and Data Interacting with Clone
# Stack-Only Data: Copy
# Ownership and Functions
# Return Values and Scope