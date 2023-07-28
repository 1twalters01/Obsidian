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

## Floating-Point Types

## Numeric Operations

## The Boolean Type

## The Character Type


# Compound type
Compound types can group multiple values into one type. Rust has two primitive compound types: tuples and arrays.

## The Tuple Type

## The Array Type
### Accessing Array Elements

### Invalid Array Element Access
