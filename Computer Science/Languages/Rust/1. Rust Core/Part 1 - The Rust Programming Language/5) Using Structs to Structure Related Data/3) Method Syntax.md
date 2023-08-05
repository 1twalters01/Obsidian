Methods are similar to functions. They are declared with the same way and contain code that's run when the method is called from somewhere else. Unlike fuctions, methods:
* Are defined within the context of a struct (or an enum or a trait object)
* Their first parameter is always `self`, which represents the instance of the struct the method is being called on

# Defining Methods
Let's change the area function that has a Rectangle instance as a parameter and instead make an area method defined on the `Rectangle` struct:

```rust
#[derive(Debug)]
struct Rectangle{
    width: u32,
    height: u32,
}

impl Rectangle {
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
        rect1.area()
    );
```




# Methods with More Parameters





# Associated Functions




# Multiple impl Blocks
