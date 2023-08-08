Most other data types represent one specific value, but collections can contain multiple values. Unlike the built-in array and tuple types, the data these collections point to is stored on the heap, which means the amount of data does not need to be known at compile time and can grow or shrink as the program runs.

Each kind of collection has different capabilities and costs. There are three main collections in Rust:
* A _vector_ allows you to store a variable number of values next to each other.
* A _string_ is a collection of characters. We’ve mentioned the `String` type previously, but in this chapter we’ll talk about it in depth.
* A _hash map_ allows you to associate a value with a particular key. It’s a particular implementation of the more general data structure called a _map_.

To learn about the other kinds of collections provided by the standard library, see [the documentation](https://doc.rust-lang.org/std/collections/index.html).  They can be grouped into four major categories:
* Sequences: `Vec`, `VecDeque`, `LinkedList`
* Maps:  `HashMap`, `BTreeMap`
* Sets: `HashSet`, `BTreeSet`
* Misc: `BinaryHeap`

# Creating a New Vector
A vector `vec<T>` allow you to store more than one value in a single data structure that puts all the values next to each other in memory. Vectors can only store values of the same type. They are useful when you have a list of items, such as the lines of text in a file or the prices of items in a shopping cart.

To create a new empty vector, we call the `Vec::new` function:
```rust
fn main() {
    let v: Vec<i32> = Vec::new();
}
```

Since we aren't inserting any values into the vector we need to add type annotation. Vectors are implemented using generics, which are covered in chapter 10.

More often, you’ll create a `Vec<T>` with initial values and Rust will infer the type of value you want to store, so you rarely need to do this type annotation. Rust conveniently provides the `vec!` macro, which will create a new vector that holds the values you give it.

As an example for an `i32` vector:
```rust
fn main() {
    let v = vec![1, 2, 3];
}
```

Because we’ve given initial `i32` values, Rust can infer that the type of `v` is `Vec<i32>`, and the type annotation isn’t necessary.

# Updating a Vector
To create a vector and then add elements to it, we can use the `push` method:
```rust
fn main() {
    let mut v = Vec::new();

    v.push(5);
    v.push(6);
    v.push(7);
    v.push(8);
}
```

# Reading Elements of Vectors
There are two ways to reference a value stored in a vector: via indexing or using the `get` method.

Via indexing:
```rust
fn main() {
    let v = vec![1, 2, 3, 4, 5];

    let third: &i32 = &v[2];
    println!("The third element is {third}");
}
```

In the case of there not being a third element the program will panic.


Via get:
```rust
fn main() {
    let v = vec![1, 2, 3, 4, 5];

    let third: Option<&i32> = v.get(2);
    match third {
        Some(third) => println!("The third element is {third}"),
        None => println!("There is no third element."),
    }
}
```

Note some details here:
* We use the index value of `2` to get the third element because vectors are indexed by number, starting at zero
* Using `&` and `[]` gives us a reference to the element at the index value.
* When we use the `get` method with the index passed as an argument, we get an `Option<&T>` that we can use with `match`.

Recall the rule that states you can’t have mutable and immutable references in the same scope. That rule applies below, where we hold an immutable reference to the first element in a vector (where it will be printed) and try to add an element to the end. This program won’t work if we also try to refer to that element later in the function:
```rust
fn main() {
    let mut v = vec![1, 2, 3, 4, 5];

    let first = &v[0];

    v.push(6);

    println!("The first element is: {first}");
}
```

This error is due to the way vectors work: because vectors put the values next to each other in memory, adding a new element onto the end of the vector might require allocating new memory and copying the old elements to the new space, if there isn’t enough room to put all the elements next to each other where the vector is currently stored.

In that case, the reference to the first element would be pointing to deallocated memory. The borrowing rules prevent programs from ending up in that situation.

# Iterating Over the Values in a Vector
To access each element in a vector in turn, we would iterate through all of the elements. For example:

```rust
fn main() {
    let v = vec![100, 32, 57];
    for i in &v {
        println!("{i}");
    }
}
```

We can also iterate over mutable references to each element in a mutable vector in order to make changes to all the elements:
```rust
fn main() {
    let mut v = vec![100, 32, 57];
    for i in &mut v {
        *i += 50;
    }
}
```

To change the value that the mutable reference refers to, we have to use the `*` dereference operator to get to the value in `i` before we can use the `+=` operator. More about this operator will be spoken about in chapter 15.

Iterating over a vector, whether immutably or mutably, is safe because of the borrow checker's rules. If we attempted to insert or remove items in the `for` loop bodies we would get an error.

The reference to the vector that the `for` loop holds prevents simultaneous modification of the whole vector.

# Using an Enum to Store Multiple Types
Vectors can only store values that are the same type.

The variants of an enum are defined under the same enum type, so when we need one type to represent elements of different types, we can define and use an enum.

say we want to get values from a row in a spreadsheet in which some of the columns in the row contain integers, some floating-point numbers, and some strings. We can:
1) Define an enum whose variants will hold the different value types
	* all the enum variants will be considered the same type: that of the enum.
2) Then we can create a vector to hold that enum

```rust
fn main() {
    enum SpreadsheetCell {
        Int(i32),
        Float(f64),
        Text(String),
    }

    let row = vec![
        SpreadsheetCell::Int(3),
        SpreadsheetCell::Text(String::from("blue")),
        SpreadsheetCell::Float(10.12),
    ];
}
```

Rust needs to know what types will be in the vector at compile time so it knows exactly how much memory on the heap will be needed to store each element. We must also be explicit about what types are allowed in this vector. Using an enum plus a `match` expression means that Rust will ensure at compile time that every possible case is handled,

If Rust allowed a vector to hold any type, there would be a chance that one or more of the types would cause errors with the operations performed on the elements of the vector.

If you don’t know the exhaustive set of types a program will get at runtime to store in a vector, the enum technique won’t work. Instead, you can use a trait object, which will be covered in Chapter 17.

Be sure to review [the API documentation](https://doc.rust-lang.org/std/vec/struct.Vec.html) for all the many useful methods defined on `Vec<T>` by the standard library.

# Dropping a Vector Drops Its Elements
Like any other `struct`, a vector is freed when it goes out of scope:

```rust
{
    let v = vec![1, 2, 3, 4]

    // do stuff with v
} // <- v goes out of scope and is freed here
```

When the vector gets dropped, all of its contents are also dropped, meaning the integers it holds will be cleaned up. The borrow checker ensures that any references to contents of a vector are only used while the vector itself is valid.