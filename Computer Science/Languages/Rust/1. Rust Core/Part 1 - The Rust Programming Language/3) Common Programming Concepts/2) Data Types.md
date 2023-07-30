This will look at two data type subsets: scalar and compound.

Every value in Rust is of a certain data type, which tells Rust what kind of data is being specified so it knows how to work with that data.

* Rust is a statically typed language, meaning that it must know the types of all variables at compile time.
* The compiler can usually infer what type we want to use based on the value and how we use it.
* In cases when many types are possible, such as when we must add a type annotation, for example:

```rust
let guess: u32 = "42".parse().expect("Not a number!");
```

If we don't add the `:u32` type annotation shown in the preceding code then we will get an error.

# Scalar Types
* A scalar type represents a single value.
* Rust has four primary scalar types: integers, floating-point numbers, Booleans, and characters.

* Signed integers: `i8`, `i16`, `i32`, `i64`, `i128` and `isize` (pointer size)
* Unsigned integers: `u8`, `u16`, `u32`, `u64`, `u128` and `usize` (pointer size)
* Floating point: `f32`, `f64`
* `char` Unicode scalar values like `'a'` (4 bytes each)
* `bool` either `true` or `false` (1 byte each)
* The unit type `()`, whose only value is an empty tuple: `()`

## Numeric Operations
Rust supports the basic mathematical operations you would expect for numbers.

```rust
fn main() {
    // addition
    let sum = 5 + 10;
	
	// subtraction
	let difference = 95.5 - 4.3;

	// multiplication
	let product = 4 * 30;
	
	// division
	let quotient = 56.7 / 32.2;
	// Integer division truncates toward zero to the nearest integer
	let truncated = -5 / 3; // Results in -1
	
	// remainder
	let remainder = 43 % 5;
}
```

## Integer Types
* In Rust, integers can be signed or unsigned (the type starts with i or u, respectively).
* A number needs a sign with it if it is signed, and is always positive if it is unsigned.
* They default to `i32` or `u32` based on context.

* Number literals can use `_` as a visual separator to make the number easier to read, such as `1_000` which means 1000.
* Number literals that can be multiple numeric types allow a type suffix, such as `57u8` to design the type.

 * `isize` and `usize` depend on the architecture of the computer your program is running on - 64 bits on a 64 bit machine and 32 bits on a 32 bit machine.
	* They are used primarily when indexing some sort of collection

### Signed integers
* `i` denotes a signed integer
* Signed integers: `i8`, `i16`, `i32`, `i64`, `i128` and `isize` (pointer size)
* Signed variants can store numbers from -($2^{n-1}$) to $2^{n-1}$ - 1 inclusive
	* For example, an i8 can store numbers from -($2^7$) to $2^7$ - 1 or -128 to 127

### Unsigned integers
* `u` denotes an unsigned integer
* Unsigned integers: `u8`, `u16`, `u32`, `u64`, `u128` and `usize` (pointer size)
* Unsigned variants can store numbers from 0 to $2^n$ - 1
	* For example, a u8 can store numbers from 0 to $2^8$ -1, or 0 to 255
	
### Integer Overflow
Say you have a u8 that can hold between 0 and 255. If you try to change the variable to a value outside that range, such as 256, *integer overflow* will occur. There are two resulting behaviours:
	* In debug mode, Rust causes your program to panic at runtime.
	* When compiling in release mode, Rust does not check for overflow. Rust will wrap the value around.
	* Relying on this is considered an error handle the possibility of an overflow with:
		* Wrap in all modes with the `wrapping_*` methods, e.g. `wrapping_add`
		* Return `None` and a boolean indicating an overflow with the `overflowing_*` methods
		* Saturate the value's minimum or maximum values with the `saturating_*` methods

## Floating-Point Types
Rust has two primitive types for floating-point numbers, which are numbers with decimal points. These are f32 and f64, which are 32 bits and 64 bits in size.

The default type is f64 as they are roughly as fast as f32 on modern CPUs, but are capable of more precision.

All floating points are signed.

```rust
let x = 2.0; // f64
let y: f32 = 3.0 //f32
```


## The Boolean Type
A Boolean type in Rust has two possible values: `true` and `false`.
They are one byte in size, and are specified using `bool`

```rust
fn main() {
	let t = true;
	let f: bool = false; // with explicit type annotation
}
```

## The Character Type
* Rust's `char` type is the language's most primitive alphabetic type.
* We specify `char` literals with single quotes, as opposed to string literals, which use double quotes.
* It represents a Unicode Scalar Value, meaning it can represent more than just ASCII, e.g.:
	* Accented letters
	* Chinese
	* emoji
	* zero-width spaces
* Unicode Scalar Values range from `U+0000` to `U+D7FF` and `U+10FFFF` to `U+10FFFF` inclusive

# Compound type
Compound types can group multiple values into one type. Rust has two primitive compound types: tuples and arrays.

* Arrays like [1, 2, 3]
* Tuples like (1, true)

## The Tuple Type
A tuple is a general way of grouping together a number of values with a variety of types into one compound type.
They have a fixed length: once declared they cannot grow or shrink in size.

Create them by writing a comma-separated list of values inside parenthesis. For example:

```rust
fn main() {
	let tup: (i32, f64, u8) = (500, 6.4, 1);
}
```

To get the individual values out of a tuple, we can use pattern matching to destructure a tuple value, for example:

```rust
fn main() {
	let tup = (500, 6.4, 1);
	let (x,y,z) = tup;
	println!("The value of y is: {y}");
}
```

We can also access a tuple element directly by using a period (`.`) followed by the index of the value we want to access:

```rust
fn main() {
	let x: (i32, f64, u8) = (500, 6.4, 1);
	let five_hundred = x.0;
	let six_point_four = x.1;
	let one = x.2;
}
```


* A tuple without any values has a special name, unit.
* It represents an empty value or an empty return type.
* Expressions implicitly return the unit value if they don't return any other value.

## The Array Type
An array is a single chunk of memory of a known, fixed size that can be allocated on the stack.

* Unlike a tuple, every element of an array must have the same type.
* Unlike arrays in some other languages, arrays in Rust have a fixed length.
* A vector is a similar collection type that is allowed to grow or shrink in size.
	* If unsure whether to use an array or a vector, you should probably chose a vector.

Arrays are useful when you want your data allocated on the stack rather than the heap (see chapter 4) or when you want to ensure you always have a fixed number of elements.

Arrays are more useful than vectors when you know the number of elements will not need to change, e.g. for the months in a year as it will always contain 12 elements:

```rust
let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
```

You write an array's type using square brackets with the type of each element, a semicolon, and then the number of elements in the array:

``` rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
```

You can also initialise an array to contain the same value for each element by specifying the initial value, followed by a semicolon, and then the length of the array in square brackets:

```rust
let a = [3; 5]; // Gives [3, 3, 3, 3, 3]
```

### Accessing Array Elements
You can access elements of an array using indexing:

```rust
fn main() {
	let a = [1, 2, 3, 4, 5];

	let first = a[0];
	let second = a[1];
}
```

### Invalid Array Element Access
In many low-level languages, this check is not done, and when you provide an incorrect index, invalid memory can be accessed. Rust will panic if you index past the end of an array during runtime due to checks. Rust protects you from this kind of error by immediately exiting instead of allowing the memory access and continuing.

Chapter 9 discusses more of Rust's error handling and how you can write readable, safe code that neither panics nor allows invalid memory access.