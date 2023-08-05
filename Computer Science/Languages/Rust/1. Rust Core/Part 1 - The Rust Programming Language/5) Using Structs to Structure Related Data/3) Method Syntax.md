Methods are similar to functions. They are declared with the same way and contain code that's run when the method is called from somewhere else. Unlike fuctions, methods:
* Are defined within the context of a struct (or an enum or a trait object)
* Their first parameter is always `self`, which represents the instance of the struct the method is being called on
They let us define all associated functions for a struct together and not have to make others search our code for them.
# Defining Methods
Let's change the area function that has a Rectangle instance as a parameter and instead make an area method defined on the `Rectangle` struct:

```rust
#[derive(Debug)]
struct Rectangle{
    width: u32,
    height: u32,
}

impl Rectangle { //Everything within this block is associated with rectangle
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

fn main() {
    let rect1 = Rectangle{
        width: 30,
        height: 50,
    };

    println!(
        "The area of the rectangle is {} square pixels.",
        rect1.area() // Call the method that we defined inside the impl
    );
}
```

In the signature for `area`, we use `&self` instead of `rectangle: &Rectangle`. Methods must have a parameter named `self` of type `Self` for their first parameter. The `&self` is actually short for `self: &Self`. Note that we still need to use the `&` in front of the `self` shorthand to indicate that this method borrows the `Self` instance, just as we did in `rectangle: &Rectangle`. 

Methods can take ownership of `self`, borrow `self` immutably, as we’ve done here, or borrow `self` mutably, just as they can any other parameter. If we wanted to change the instance that we’ve called the method on as part of what the method does, we’d use `&mut self` as the first parameter.

Having a method that takes ownership of the instance by using just `self` as the first parameter is rare; this technique is usually used when the method transforms `self` into something else and you want to prevent the caller from using the original instance after the transformation.

Often, we give a method the same name as a field and make it return the value in the field. Methods like this are called _getters_, and Rust does not implement them automatically for struct fields as some other languages do. They are useful as you can make the field private but the method public, and thus enable read-only access to that field as part of the type’s public API.

# Methods with More Parameters
Say we wanted to make an impl that can do the following:
```rust
fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };
    
    let rect2 = Rectangle {
        width: 10,
        height: 40,
    };

    let rect2 = Rectangle {
        width: 60,
        height: 45,
    };

    println!("Can rect1 hold rect2? {}", rect1.can_hold(&rect2));
    println!("Can rect1 hold rect3? {}", rect1.can_hold(&rect3));
}
```

The expected output would look like this:
```terminal
Can rect1 hold rect2? true
Can rect1 hold rect3? false
```

We would do something like this:
```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn Area(&self) -> u32 {
        self.width * self.height
    }

    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
    }
}
```

Methods can take multiple parameters that we add to the signature after the `self` parameter, and those parameters work just like parameters in functions.

# Associated Functions
All functions defined within an `impl` block are called _associated functions_ because they’re associated with the type named after the `impl`. We can define associated functions that don’t have `self` as their first parameter (and thus are not methods) because they don’t need an instance of the type to work with. The `String::from` function that’s defined on the `String` type is one of these.

Associated functions that aren’t methods are often used for constructors that will return a new instance of the struct. These are often called `new`. This is not built in to structs in Rust.

For example, we could choose to provide an associated function named `square` that would have one dimension parameter and use that as both width and height, thus making it easier to create a square `Rectangle` rather than having to specify the same value twice:

```rust
impl Rectangle {
    fn square(size: u32) -> Self {
        Self { width: size, height: size, }
            width: size,
            height: size
        }    
    }
```

The `Self` keywords in the return type and in the body of the function are aliases for the type that appears after the `impl` keyword, which in this case is `Rectangle`.

To call this associated function, we use the `::` syntax with the struct name, for example;
`let sq = Rectangle::square(3);`

This function is namespaced by the struct: the `::` syntax is used for both associated functions and namespaces created by modules. Modules are discussed in Chapter 7.

# Multiple impl Blocks
