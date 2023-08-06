Let's write a program that calculates the area of a rectangle.

```rust
fn main() {
    let width1 = 30;
    let height1 = 50;

    println!(
        "The area of the rectangle is {} square pixels.",
        area(width1, height1)
    );
}

fn area(width: u32, height: u32) -> u32 {
    width * height
}
```
This code works, but it is not clear that our width and height parameters are related. Imagine a very complex program - it would not be good if others had to wade through code to find our variables or to see if they are related.

# Refactoring with Tuples
Another factor using tuples lets us add structure and just pass one argument into our area function:

```rust
fn main() {
    let rect1 = (30, 50)

    println!(
        "The area of the rectangle is {} square pixels.",
        area(rect1)
    );
}

fn area (dimensions: (u32, u32)) -> u32 {
    dimensions.0 * dimensions.1
}
```

This is better as tuples let us add some structure, but it is less clear. Typles don't name their elements, so we have to index into the parts of the tuple.

Mixing up the width and height doesn't matter for the area, but it would if we needed to draw the rectangle on a screen. We should use structs.


# Refactoring with Structs: Adding More Meaning
Look at the following:

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let rect1 = Rectangle {
         width: 30,
         height: 50,
    };

    println!(
        "The area of the rectangle is {} square pixels.",
        area(&rect1)
    );
}

fn area(rectangle: &Rectangle) -> u32 {
    rectangle.width * rectangle.height
}
```

Now our area function is defined with one parameter, which we've named rectangle, whose type is an immutable borrow of a struct `Rectangle` instance.
* Recall that we want to borrow the struct instead of taking ownership of it
* We can now use descriptive naming for the width and height instead of 0 and 1

# Adding Useful Functionality with Derived Traits
We mauy want to print an instance of Rectangle while debugging our code. The following will give us an error:

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!("rect1 is {}", rect1);
}
```

The `println!` macro can do many kinds of formatting, and by default, the curly brackets tell `println!` to use formatting known as `Display`: output intended for direct end user consumption.

The primitive types we've seen implement `Display` by default because there's only one way you'd want to show a `1` or any other primitive type. But with structs, the way `println!` should format the output is less clear because there are more display possibilities:
* Do you want commas?
* Do you want to print thte curly brackets?
* Should all the fields be shown?
* etc.

As such, Rust doesn't try to guess and just throws an error.
We can make our own custom print message, or use one of the following defaults:
* println!("rect1 is {:?}", rect1);
* println!("rect1 is {:#?}", rect1);

We also have to add the Debug trait to our struct or manually implement `Debug` like so:

```rust
#[derive(Debug)]
struct Rectangle{
    width: u32,
    height: u32,
}
```

Using `{:?}`:
`rect1 is Rectangle { width: 30, height: 50 }`

Using `{:#?}` gives
```
rect1 is Rectangle {
    width: 30,
    height: 50,
}
  ```

Another way to print out a value using the Debug format is to use the `dbg!` macro, which takes ownership of the value. This also needs the debug trait on our struct. Say we wanted to add a scaling factor to our width and print debug information about it as well as print debug information about the rectangle:

```rust
#[derive(Debug)]
struct Rectangle{
    width: u32,
    height: u32,
}

fn main() {
    let scale = 2;
    let rect1 = Rectangle {
        width: dbg!(30*scale),
        height: 50,
    };
}

dbg!(&rect1);
```

Remember to make rect1 a reference as we don't want to give ownership of it over to dbg.
