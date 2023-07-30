# Variables
Variables are immutable by default, however you can make your variables mutable.
You declare them with the `let` keyword

When a variable is immutable, once a value is bound to a name, you can't change that value. For example, this will give an error message:

```rust
fn main() {
    let x = 5;
    println!("The value of x is: {x}");
    x = 6;
    println!("The value of x is: {x}");
}
```

We can make code mutable by adding `mut` in front of the variable name:

```rust
fn main() {
    let mut x = 5;
    println!("The value of x is: {x}");
    x = 6;
    println!("The value of x is: {x}");
}
```

**We cannot allowed to mutate a variable's type with `mut`**
# Constants
* Like immutable variables, constants are values that are bound to a name and are not allowed to change
* You aren't allowed to use `mut` with constants - they aren't just immutable by default - they are always immutable
* You declare them with the `const` keyword and the type of the value **must** be annotated.
* Can only be set to a constant expression, not the result of a value that could only be computed at runtime.
* They can be declared in any scope, including the global scope
	* Especially useful for values that many parts of the code need to know about
* They are valid for the entire time a program runs, within the scope in which they were declared.

Rust naming convention is for constants to use all uppercase with underscores between words.
```rust
const THREE_HOURS_IN_SECONDS: u32 = 60*60*3;
```

# Shadowing
* You can declare a new variable within the same name as a previous variable.
* The second variable is what the compiler will see when you use the name of the variable until it itself is shadowed or the scope ends.

```rust
fn main() {
    let x = 5;
	let x = x + 1;
	
	{
	    let x = x*2;
	    println!("The value of x in the inner scope is; {x}");
	}
	
	println!("The value of x is: {x}");
}
```

Upon running this program we will see the following:

```
The value of x in the inner scope is: 12
The value of x is: 6
```

Shadowing is different from marking a variable as `mut` because we'll get a compile-time error if we accidentally try to reassign to this variable without using the `let` keyword.

**We are allowed to mutate a variable's type with this.** Say we wanted to get the number of spaces put in a variable and then turn that into a number. The following gives an error:

```rust
let mut spaces = "     " // 5 spaces;
spaces = spaces.len();
```

However, the following will not:

```rust
let spaces = "     " # 5 spaces;
let spaces = spaces.len();
```

# Freezing
When data is bound by the same name immutably, it also freezes. Frozen data can't be modified until the immutable binding goes out of scope:

```rust
fn main() {
    let mut _mutable_integer = 7i32;

    {
        // Shadowing by immutable '_mutable_integer'}
        let _mutable_integer = _mutable_integer;
        
        // Error! '_mutable_integer' is frozen in this scope
        _mutable_integer = 50;
    }
    // Now it is ok
    _mutable_integer = 3;
}
```