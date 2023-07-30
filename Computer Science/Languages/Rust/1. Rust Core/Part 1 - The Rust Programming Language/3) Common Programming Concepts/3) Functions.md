# Introduction
One of the most important functions in Rust is the `main` function, which is the entry point of many programs.

The `fn` keyword allows you to declare new functions.
Rust uses *snake_case* as the conventional style for function and variable names. Here, all letters are lowercase and underscores separate words.
We can call any function we've defined by entering its name followed by a set of parentheses.

```rust
fn main() {
    println!("Hello world");

    another_function();
}

fn another_function() {
    println!("Another function")
}
```

Rust doesn't care where you define your functions, only that they're defined somewhere in a scope that can be seen by the caller.

# Parameters
We can define functions to have parameters or arguments.
* Parameters are the special variables that are part of the function's signature.
* Arguments are the concrete values you pass into functions.

```rust
fn main() {
    print_labeled_measurements(5, 'cm');
}

fn print_labeled_measurements(value: i32, unit_label: char) {
    println!("The measurement is: {value}{unit_label}");
}
```

In function signatures, you must declare the type of each parameter.
The compiler is able to give more helpful error messages if it knows what types the function expects.

# Statements and Expressions
Rust is an expression-based language 
* Statements - instructions that perform some action and do not return a value
* Expressions - evaluate to a resultant value e.g. 5+2 which evaluates to 7


```rust
fn main() { // Function definitions are statements
    let y = 6; // a statement
}
```

Statements do not return values so you can't assign a `let` statement to another variable. The following code results in an error:

```rust
fn main() {
	let x = (let y = 6);
}
```

The `let y = 6` statement doesn't return so there isn't anything for `x` to bind to. This is different from what happens in other languages, like C and Python, where the assignment returns the value of the assignment. In these languages, you can write `x = y = 6` and have the value 6, which is not the case in Rust.

* Calling a function is an expression.
* Calling a macro is an expression.
* A new scope block is an expression.

In the case of a new scope block:

```rust
fn main() {
    let y = {
        let x = 3;
        x + 1
    };

    println!("The value of y is: {y}");
}
```

Note that the `x + 1` line doesn't have a semicolon at the end.
**If you add a semicolon to the end of an expression, you turn it into a statement, and it will then not return a value.**

# Functions with Return Values
Functions can return values to code that calls them. We don't name return values, but we must declare their type after an arrow (`->`).

In Rust, the return value of the function is synonymous with the value of the final expression in the block of the body of a function.
	* You can early return from a function by using the `return` keyword and specifying a value as well. Here you would have the semicolon.

```rust
fn five() -> i32 {
    // This is perfectly valid in Rust
    5 // No semicolon so it is an expression and not a statement
}

fn plus_one(x: i32) -> i32 {
    return x + 1;
}

fn main() {
    let x = five();
    println!("The value of x is: {x}")
    let y = plus_one(x);
    println!("The value of x + 1 is: {y}")
}
```

