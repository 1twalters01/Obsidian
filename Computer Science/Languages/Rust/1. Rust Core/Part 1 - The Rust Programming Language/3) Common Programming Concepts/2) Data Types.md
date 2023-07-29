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
A scalar type represents a single value.
Rust has four primary scalar types: integers, floating-point numbers, Booleans, and characters.

## Integer Types
* (An integer is a number without a fractional component.
* In Rust, an integer can be signed (the type starts with i) or unsigned (the type starts with u)
	* A number needs a sign with it if it is a signed number, and is always positive if it is an unsigned number.

	* Signed variants can store numbers from -($2^{n-1}$) to $2^{n-1}$ - 1 inclusive
		* For example, an i8 can store numbers from -($2^7$) to $2^7$ - 1 or -128 to 127
	* Unsigned variants can store numbers from 0 to $2^n$ - 1
		* For example, a u8 can store numbers from 0 to $2^8$ -1, or 0 to 255
	* isize and usize depend on the architecture of the computer your program is running on - 64 bits on a 64 bit machine and 32 bits on a 32 bit machine.
		* This is used primarily when indexing some sort of collection

	* Number literals that can be multiple numeric types allow a type suffix, such as `57u8` to design the type
	* Number literals can use `_` as a visual seperator to make the number easier to read, such as `1_000` which means 1000

### Integer Overflow


## Floating-Point Types
Rust has two primitive types for floating-point numbers, which are numbers with decimal points. These are f32 and f64, which are 32 bits and 64 bits in size.

The default type is f64 as they are roughly as fast as f32 on modern CPUs, but are capable of more precision.

All floating points are signed.

```rust
let x = 2.0; // f64
let y: f32 = 3.0 //f32
```

## Numeric Operations
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

## The Boolean Type
A Boolean type in Rust has two possible values: `true` and `false`.
They are one byte in size.
They are specified using `bool`

```rust
fn main() {
	let t = true;
	let f: bool = false; // with explicit type annotation
}
```

## The Character Type


# Compound type
Compound types can group multiple values into one type. Rust has two primitive compound types: tuples and arrays.

## The Tuple Type

## The Array Type
### Accessing Array Elements

### Invalid Array Element Access
