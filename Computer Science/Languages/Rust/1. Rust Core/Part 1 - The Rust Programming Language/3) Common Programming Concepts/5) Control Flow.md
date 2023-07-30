The most common constructs that let you control the flow of execution of Rust code are if expressions and loops, though other ones exist.

# If/Else If/Else
You do an if statement as follows:

```rust
fn main() {
    let number = 3;
	if number % 4 == 0 {
	    println!("number is divisible by 4");
	} else if number % 3 == 0 {
	    println!("number is divisible by 3");
	} else if number % 2 == 0 {
	    println!("number is divisible by 2");
	} else {
	    println!("number is not divisible by 4, 3, or 2");
	}
}
```

The condition *must* be a `bool` or an error will be thrown. Unlike languages like python, Rust will not automatically try to convert non-Boolean types to a Boolean. You must be explicit and always provide `if` with a Boolean as its condition.

```rust
fn main() {
    let number = 3;

    // Throws an error as number is an i32 and not a boolean
    if number {
        println!("number was three")
    }
}
```

## if in a let statement
As `if` is an expression, we can use it on the right side of a `let` statement to assign the outcome to a variable:

```rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { 6 };

    println!("The value of number is: {number}");
}
```

The value has the potential to be from each arm of the if statement; the results from each arm must be the same type for this to work. The following will thus throw an error:

```rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { "six" };
    println!("The value of number is: {number}");
}
```

`"six"` is not an integer like 5 so the code will panic.

# Loops
Rust has three kinds of loops: `loop`, `while`, and `for`.

## Loop
The loop keyword tells Rust to execute a block of code over and over again forever or until you explicitly tell it to stop. You would have to use ctrl-c in your terminal to break out of the following code:

```rust
fn main() {
    loop {
        println!("again!");
    }
}
```

* Use the `break` keyword within the loop to tell the program to break out of the loop.
* Use the `continue` keyword to skip over any remaining code in this iteration of the loop and go to the next iteration.

### Returning values from Loops
You may need to pass the result of an operation out of a loop and into the rest of your code. Do this by adding what you want returned after the break expression used to stop the loop.

```rust
fn main() {
    let mut counter = 0;
    let result = loop {
        counter += 1;
        if counter == 10{
            break counter * 2;
        }
    };

    println!("The result is {result}") // Will return 20
}
```

### Loop labels to Disambiguate Between Multiple Loops
If you have loops within loops, `break` and `continue` apply to the innermost loop at that point.

You can specify a *loop label* on a loop that you can then use with `break` or `continue` to specify that those keywords apply to the labelled loop instead of the innermost loop.
* Loop labels must use single quotes.

```rust
fn main() {
    let mut count = 0;
    'count_loop': loop {
        println!("count = {count}");
        
        let mut remaining == 10;
        loop {
            println!("remaining = {remaining}")
            if remaining == 9 {
                break; // will break the inner loop
            }
            if count == 2 {
                break 'count_loop' // will break the loop named count_loop
            }
            remaining -= 1;
        }
    }
    println!("End count = {count}");
}
```

```output
count = 0
remaining = 10
remaining = 9
count = 1
remaining = 10
remaining = 9
count = 2
remaining = 10
End count = 2
```

## While
While the condition for the while loop is `true`, the loop runs. When the condition ceases to be `true`, the program calls `break`, stopping the loop.
* It is possible to do this using a combination of `loop`, `if`, `else`, and `break`, but Rust has this built in. This gets rid of a lot of nesting.

An example of the while loop:

```rust
fn main() {
    let mut number = 3;

    while nubmer != 0 {
        println!("{number}!");
        nubmer -= 1;
    }

    println!("Blast off")
}
```

## Looping through a collection with for
You can choose to use the `while` construct to loop over the elements of a collection, such as an array. You could use a for loop:

```rust
fn main() {
    let a = [10, 20, 30, 40, 50];
	let mut index = 0;

    while index < 5 {
	    println!("the value is: {}", a[index]);
	    index += 1;
	}
}
```

* This approach is error prone - we could make the program panic if the index value or test condition is incorrect.
* It is also slow, because the compiler adds runtime code to perform the conditional check of whether the index is within the bounds of the array on every iteration through the loop.

Use a `for` loop instead:

```rust
fn main() {
    let a = [10, 20, 30, 40, 50];

    for element in a {
        println!("the value is {element}");
    }
}
```

Use `.rev` to go from the end to the beginning instead:

```rust
fn main() {
    for number in (1..4).rev() {
         println!("{number}")
    }
}
```