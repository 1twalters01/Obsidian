All programs have to manage the way they use a computer's memory while running. Some have garbage collection that regularly looks for no longer used memory as the program runs. In other languages, the programmer must explicitly allocate and free the memory. Rust manages memory through a system of ownership with a set of rules that, if broken, will mean that the program won't compile. None of the features of ownership will slow down your program while it's running.

Ownership lets Rust make memory safety guarantees without needing a garbage collector, meaning it is important to understand how it works.

# Ownership rules
1) Each value in Rust has an Owner.
2) There can only be one owner at a time.
3) When the owner goes out of scope, the value will be dropped.

# The Stack and the Heap
In systems programming languages like Rust, whether a value is on the stack or the heap affects how the language behaves and why you have to make certain decisions. The stack and the heap are parts of memory available to your code to use at runtime, but they are structured in different ways.

* The stack stores values in the order it gets them and removes it in the opposite order.
	* This is referred to as last in, first out.
* Adding data is called *pushing onto the stack*.
* Removing data is called *popping off the stack*.
* All data stored on the stack must have a known, fixed size.
* Data with an unknown size at compile time or a size that might change must be stored on the heap instead.

The heap is less organised: when you put data on the heap, you request a certain amount of space.
* The memory allocator finds an empty spot in the heap that is big enough, marks it as being in use, and returns a _pointer_, which is the address of that location.
* This process is called _allocating on the heap_ and is sometimes abbreviated as just _allocating_
	* Pushing values onto the stack is not considered allocating.
	* Because the pointer to the heap is a known, fixed size, you can store the pointer on the stack, but when you want the actual data, you must follow the pointer.
	* Think of being seated at a restaurant. When you enter, you state the number of people in your group, and the host finds an empty table that fits everyone and leads you there. If someone in your group comes late, they can ask where you’ve been seated to find you.

Pushing to the stack is faster than allocating on the heap because the allocator never has to search for a place to store new data; that location is always at the top of the stack.
Comparatively, allocating space on the heap requires more work because the allocator must first find a big enough space to hold the data and then perform bookkeeping to prepare for the next allocation.

Accessing data in the heap is slower than accessing data on the stack because you have to follow a pointer to get there.
* Continuing the analogy, consider a server at a restaurant taking orders from many tables. It’s most efficient to get all the orders at one table before moving on to the next table.
* Taking an order from table A, then an order from table B, then one from A again, and then one from B again would be a much slower process.

When your code calls a function, the values passed into the function (including, potentially, pointers to data on the heap) and the function’s local variables get pushed onto the stack. When the function is over, those values get popped off the stack.

Keeping track of what parts of code are using what data on the heap, minimizing the amount of duplicate data on the heap, and cleaning up unused data on the heap so you don’t run out of space are all problems that ownership addresses.

Once you understand ownership, you won’t need to think about the stack and the heap very often, but knowing that the main purpose of ownership is to manage heap data can help explain why it works the way it does.

# Variable Scope
A scope is the range within a program for which an item is valid. A variable is valid from the point at which it’s declared until the end of the current _scope_. Take the following variable string literal:
```rust
fn main() {
    {                      // s is not valid here, it’s not yet declared
        let s = "hello";   // s is valid from this point forward 
        // do stuff with s
    }                      // this scope is now over, and s is no longer valid
}
```

The relationship between scopes and when variables are valid is similar to that in other programming languages.

# The string Type
To illustrate the rules of ownership, we need a data type that is more complex than those we covered in [[2) Data Types]]. The types covered previously are:
* Of a known size
* Can be stored on the stack and popped off the stack when their scope is over
* Can be quickly and trivially copied to make a new, independent instance if another part of code needs to use the same value in a different scope

We want to look at data that is stored on the heap and explore how Rust knows when to clean up that data, and the `String` type is a great example. We’ll concentrate on the parts of `String` that relate to ownership. These aspects also apply to other complex data types, whether they are provided by the standard library or created by you.

We’ve already seen string literals (&str), where a string value is hardcoded into our program. String literals are convenient, but they aren’t suitable for every situation in which we may want to use text.
* One reason is that they’re immutable
* Not every string value can be known when we write our code
	* for example, what if we want to take user input and store it?

For these situations, Rust has a second string type, `String`.
* This type manages data allocated on the heap and as such is able to store an amount of text that is unknown to us at compile time.
* You can create a `String` from a string literal using the `from` function, like so:
```rust
fn main() {
    let s = String::from("hello");
}
```

This kind of string _can_ be mutated:
```rust
fn main() {
    let mut s = String::from("hello");
    s.push_str(", world!"); // push_str() appends a literal to a String
    println!("{}", s); // This will print `hello, world!`
}
```

Why can `String` be mutated but literals cannot? The difference is in how these two types deal with memory.

# Memory Allocation
String literals are fast and efficient because we know the contents at compile time, so the text is hardcoded directly into the final executable.
* These properties only come from the string literal’s immutability.
* We can’t put a blob of memory into the binary for each piece of text whose size is unknown at compile time and whose size might change while running the program.

With the `String` type, in order to support a mutable, growable piece of text, we need to allocate an amount of memory on the heap, unknown at compile time, to hold the contents. This means:
* The memory must be requested from the memory allocator at runtime.
* We need a way of returning this memory to the allocator when we’re done with our `String`.

That first part is done by us: when we call `String::from`, its implementation requests the memory it needs. This is pretty much universal in programming languages.

The second part is different. In languages with a _garbage collector (GC)_, the GC keeps track of and cleans up memory that isn’t being used anymore, and we don’t need to think about it.

In most languages without a GC, it’s our responsibility to identify when memory is no longer being used and to call code to explicitly free it, just as we did to request it. Doing this correctly has historically been a difficult programming problem.
* If we forget, we’ll waste memory.
* If we do it too early, we’ll have an invalid variable.
* If we do it twice, that’s a bug too. We need to pair exactly one `allocate` with exactly one `free`.

With Rust, the memory is automatically returned once the variable that owns it goes out of scope.

```rust
{
    let s = String::from("hello");  // s is valid from this point forward
    
    // do stuff with s
}                                   // this scope is now over, and s is no longer valid
```

There is a natural point at which we can return the memory our `String` needs to the allocator: when `s` goes out of scope.  When a variable goes out of scope, Rust calls a special function called drop.

This pattern has a profound impact on the way Rust code is written. It may seem simple right now, but the behaviour of code can be unexpected in more complicated situations when we want to have multiple variables use the data we’ve allocated on the heap.

# Variables and Data Interacting with Move
Multiple variables can interact with the same data in different ways in Rust. In the case of:
```rust
fn main() {
    let x = 5;
    let y = x;
}
```

This binds the value `5` to `x`; then makes a copy of the value in `x` and binds it to `y`. We now have two variables, `x` and `y`. This happens, because the integers are simple values with a known, fixed size. These two `5` values are pushed onto the stack.

Now let’s look at the `String` version:

```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1;
}
```

This looks very similar, so we might assume that the way it works would be the same: that is, the second line would make a copy of the value in `s1` and bind it to `s2`. But this isn’t what happens.

A `String` is a group of data stored on the stack. This data is made up of three parts:
*  A pointer to the memory that holds the contents of the string
* A length - how much memory, in bytes, the contents of the `String` are currently using.
* A capacity -  the total amount of memory, in bytes, that the `String` has received from the allocator.

![[Pasted image 20230804204732.png]]
When we assign `s1` to `s2`, the `String` data is copied, meaning we copy the pointer, the length, and the capacity that are on the stack. We do not copy the data on the heap that the pointer refers to.

Earlier, we said that when a variable goes out of scope, Rust automatically calls the `drop` function and cleans up the heap memory for that variable. This is a problem: when `s2` and `s1` go out of scope, they will both try to free the same memory. This is known as a _double free_ error and is one of the memory safety bugs we mentioned previously. Freeing memory twice can lead to memory corruption, which can potentially lead to security vulnerabilities.

To ensure memory safety, after the line `let s2 = s1;`, Rust considers `s1` as no longer valid. Therefore, Rust doesn’t need to free anything when `s1` goes out of scope.

```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1;
    
    println!("{}, world!", s1); // error
}
```

The concept of copying the pointer, length, and capacity without copying the data probably sounds like making a shallow copy. But because Rust also invalidates the first variable, instead of being called a shallow copy, it’s known as a _move_.

With only `s2` valid, when it goes out of scope it alone will free the memory, and we’re done.
Rust will never automatically create “deep” copies of your data. Therefore, any _automatic_ copying can be assumed to be inexpensive in terms of runtime performance.

# Variables and Data Interacting with Clone
If we _do_ want to deeply copy the heap data of the `String`, not just the stack data, we can use a common method called `clone`.

Here’s an example of the `clone` method in action:

```rust
fn main() {
    let s1 = String::from("hello");
    let s2 = s1.clone();

    println!("s1 = {}, s2 = {}", s1, s2);
}
```

When you see a call to `clone`, you know that some arbitrary code is being executed and that code may be expensive. It’s a visual indicator that something different is going on.

# Stack-Only Data: Copy

We did not have to call clone on the example before, but `x` was still valid and wasn’t moved into `y`.

Types such as integers that have a known size at compile time are stored entirely on the stack, so copies of the actual values are quick to make. That means there’s no reason we would want to prevent `x` from being valid after we create the variable `y`. There’s no difference between deep and shallow copying here, so calling `clone` wouldn’t do anything different from the usual shallow copying, and we can leave it out.

Rust has a special annotation called the `Copy` trait that we can place on types that are stored on the stack, as integers are (we’ll talk more about traits in Chapter 10). If a type implements the `Copy` trait, variables that use it do not move, but instead are trivially copied, making them still valid after assignment to another variable.

Rust won’t let us annotate a type with `Copy` if the type, or any of its parts, has implemented the `Drop` trait.

As a general rule, any group of simple scalar values can implement `Copy`, and nothing that requires allocation or is some form of resource can implement `Copy`. Here are some of the types that implement `Copy`:
* All the integer types, such as `u32`.
* The Boolean type, `bool`, with values `true` and `false`.
* All the floating-point types, such as `f64`.
* The character type, `char`.
* Tuples, if they only contain types that also implement `Copy`. For example, `(i32, i32)` implements `Copy`, but `(i32, String)` does not.

# Ownership and Functions
The mechanics of passing a value to a function are similar to those when assigning a value to a variable. Passing a variable to a function will move or copy, just as assignment does. For example:

```rust
fn main() {
    let s = String::from("hello");  // s comes into scope

    takes_ownership(s);             // s's value moves into the function...
                                    // ... and so is no longer valid here

    let x = 5;                      // x comes into scope

    makes_copy(x);                  // x would move into the function,
                                    // but i32 is Copy, so it's okay to still
                                    // use x afterward

} // Here, x goes out of scope, then s. But because s's value was moved, nothing
  // special happens.

fn takes_ownership(some_string: String) { // some_string comes into scope
    println!("{}", some_string);
} // Here, some_string goes out of scope and `drop` is called. The backing
  // memory is freed.

fn makes_copy(some_integer: i32) { // some_integer comes into scope
    println!("{}", some_integer);
} // Here, some_integer goes out of scope. Nothing special happens.
```

If we tried to use `s` after the call to `takes_ownership`, Rust would throw a compile-time error. These static checks protect us from mistakes.

# Return Values and Scope
Returning values can also transfer ownership. For example:

```rust
fn main() {
    let s1 = gives_ownership();         // gives_ownership moves its return
                                        // value into s1

    let s2 = String::from("hello");     // s2 comes into scope

    let s3 = takes_and_gives_back(s2);  // s2 is moved into
                                        // takes_and_gives_back, which also
                                        // moves its return value into s3
} // Here, s3 goes out of scope and is dropped. s2 was moved, so nothing
  // happens. s1 goes out of scope and is dropped.

fn gives_ownership() -> String {             // gives_ownership will move its
                                             // return value into the function
                                             // that calls it

    let some_string = String::from("yours"); // some_string comes into scope

    some_string                              // some_string is returned and
                                             // moves out to the calling
                                             // function
}

// This function takes a String and returns one
fn takes_and_gives_back(a_string: String) -> String { // a_string comes into scope
    a_string  // a_string is returned and moves out to the calling function
}
```

The ownership of a variable follows the same pattern every time: assigning a value to another variable moves it. When a variable that includes data on the heap goes out of scope, the value will be cleaned up by `drop` unless ownership of the data has been moved to another variable.

Rust does let us return multiple values using a tuple.

```rust
fn main() {
    let s1 = String::from("hello");

    let (s2, len) = calculate_length(s1);

    println!("The length of '{}' is {}.", s2, len);
}

fn calculate_length(s: String) -> (String, usize) {
    let length = s.len(); // len() returns the length of a String

    (s, length)
}
```

This is a lot of work for a concept that should be common. What if we want to let a function use a value but not take ownership? Rust has a feature for using a value without transferring ownership, called _references_.






