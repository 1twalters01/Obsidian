Lifetimes are another kind of generic that we’ve already been using. Rather than ensuring that a type has the behaviour we want, lifetimes ensure that references are valid as long as we need them to be.

# Preventing Dangling Lifetimes with Lifetimes
The main aim of lifetimes is to prevent _dangling references_, which cause a program to reference data other than the data it’s intended to reference. Consider the following, which has an inner scope and an outer scope:
```rust
fn main() {
    let r;

    {
        let x = 5;
        r = &x;
    }

    println!("r: {}", r);
}
```

We get the following error:
```terminal
$ cargo run
   Compiling chapter10 v0.1.0 (file:///projects/chapter10)
error[E0597]: `x` does not live long enough
 --> src/main.rs:6:13
  |
6 |         r = &x;
  |             ^^ borrowed value does not live long enough
7 |     }
  |     - `x` dropped here while still borrowed
8 |
9 |     println!("r: {}", r);
  |                       - borrow later used here

For more information about this error, try `rustc --explain E0597`.
error: could not compile `chapter10` due to previous error
```

The variable `x` doesn’t “live long enough.” If Rust allowed this code to work, `r` would be referencing memory that was deallocated when `x` went out of scope, and anything we tried to do with `r` wouldn’t work correctly.

# The Borrow Checker
The Rust compiler has a _borrow checker_ that compares scopes to determine whether all borrows are valid.
```rust
fn main() {
    let r;                // ---------+-- 'a
                          //          |
    {                     //          |
        let x = 5;        // -+-- 'b  |
        r = &x;           //  |       |
    }                     // -+       |
                          //          |
    println!("r: {}", r); //          |
}                         // ---------+
```

Here, we’ve annotated the lifetime of `r` with `'a` and the lifetime of `x` with `'b`. As you can see, the inner `'b` block is much smaller than the outer `'a` lifetime block.

At compile time, Rust compares the size of the two lifetimes and sees that `r` has a lifetime of `'a` but that it refers to memory with a lifetime of `'b`. The program is rejected because `'b` is shorter than `'a`: the subject of the reference doesn’t live as long as the reference.


The following fixes the code so it doesn’t have a dangling reference and compiles without any errors:
```rust
fn main() {
    let x = 5;            // ----------+-- 'b
                          //           |
    let r = &x;           // --+-- 'a  |
                          //   |       |
    println!("r: {}", r); //   |       |
                          // --+       |
}                         // ----------+
```

Here, `x` has the lifetime `'b`, which in this case is larger than `'a`. This means `r` can reference `x` because Rust knows that the reference in `r` will always be valid while `x` is valid.

# Generic Lifetimes in Functions
Write a function that returns the longer of two string slices. This function will take two string slices and return a single string slice:
```rust
fn main() {
    let string1 = String::from("abcd");
    let string2 = "xyz";

    let result = longest(string1.as_str(), string2);
    println!("The longest string is {}", result);
}
```

Note that we want the function to take string slices, which are references, rather than strings, because we don’t want the `longest` function to take ownership of its parameters.

The implementation of the `longest` function shown below won't compile:
```rust
fn longest(x: &str, y: &str) -> &str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

Instead we get an error about lifetimes:
```terminal
$ cargo run
   Compiling chapter10 v0.1.0 (file:///projects/chapter10)
error[E0106]: missing lifetime specifier
 --> src/main.rs:9:33
  |
9 | fn longest(x: &str, y: &str) -> &str {
  |               ----     ----     ^ expected named lifetime parameter
  |
  = help: this function's return type contains a borrowed value, but the signature does not say whether it is borrowed from `x` or `y`
help: consider introducing a named lifetime parameter
  |
9 | fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
  |           ++++     ++          ++          ++

For more information about this error, try `rustc --explain E0106`.
error: could not compile `chapter10` due to previous error
```

Rust can’t tell whether the reference being returned refers to `x` or `y`. We don’t know either, because the `if` block in the body of this function returns a reference to `x` and the `else` block returns a reference to `y`.

We also don’t know the concrete lifetimes of the references that will be passed in, so we can’t look at the scope to determine whether the reference we return will always be valid. The borrow checker can’t determine this either, because it doesn’t know how the lifetimes of `x` and `y` relate to the lifetime of the return value.

To fix this error, we’ll add generic lifetime parameters that define the relationship between the references so the borrow checker can perform its analysis.

# Lifetime Annotation Syntax
Lifetime annotations don’t change how long any of the references live. Instead, they describe the relationships of the lifetimes of multiple references to each other without affecting the lifetimes.

Just as functions can accept any type when the signature specifies a generic type parameter, functions can accept references with any lifetime by specifying a generic lifetime parameter.

The names of lifetime parameters must start with an apostrophe (`'`) and are usually all lowercase and very short, like generic types. Most people use the name `'a` for the first lifetime annotation.

Here are some examples: a reference to an `i32` without a lifetime parameter, a reference to an `i32` that has a lifetime parameter named `'a`, and a mutable reference to an `i32` that also has the lifetime `'a`:

```rust
&i32        // a reference
&'a i32     // a reference with an explicit lifetime
&'a mut i32 // a mutable reference with an explicit lifetime
```

One lifetime annotation by itself doesn’t have much meaning, because the annotations are meant to tell Rust how generic lifetime parameters of multiple references relate to each other.

# Lifetime Annotations in Function Signatures
To use lifetime annotations in function signatures, we need to declare the generic _lifetime_ parameters inside angle brackets between the function name and the parameter list, just as we did with generic type parameters.

We want the signature to express the following constraint: the returned reference will be valid as long as both the parameters are valid. Naming the lifetime `'a` and then add it to each reference:
```rust
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

The function signature now tells Rust that for some lifetime `'a`, the function takes two parameters, both of which are string slices that live at least as long as lifetime `'a`. The lifetime of the reference returned by the `longest` function is the same as the smaller of the lifetimes of the values referred to by the function arguments.

When we specify the lifetime parameters in this function signature, we’re not changing the lifetimes of any values passed in or returned. Rather, we’re specifying that the borrow checker should reject any values that don’t adhere to these constraints.

The following thus compiles:
```rust
fn main() {
    let string1 = String::from("long string is long");

    {
        let string2 = String::from("xyz");
        let result = longest(string1.as_str(), string2.as_str());
        println!("The longest string is {}", result);
    }
}
```

And this does not:
```rust
fn main() {
    let string1 = String::from("long string is long");
    let result;
    {
        let string2 = String::from("xyz");
        result = longest(string1.as_str(), string2.as_str());
    }
    println!("The longest string is {}", result);
}
```

## Task
Try designing more experiments that vary the values and lifetimes of the references passed in to the `longest` function and how the returned reference is used. Make hypotheses about whether or not your experiments will pass the borrow checker before you compile; then check to see if you’re right!

# Thinking in Terms of Lifetimes
if we changed the implementation of the `longest` function to always return the first parameter rather than the longest string slice, we wouldn’t need to specify a lifetime on the `y` parameter.

The following code will compile:
```rust
fn longest<'a>(x: &'a str, y: &str) -> &'a str {
    x
}
```

We’ve specified a lifetime parameter `'a` for the parameter `x` and the return type, but not for the parameter `y`, because the lifetime of `y` does not have any relationship with the lifetime of `x` or the return value.

When returning a reference from a function, the lifetime parameter for the return type needs to match the lifetime parameter for one of the parameters.
If the reference returned does _not_ refer to one of the parameters, it must refer to a value created within this function. However, this would be a dangling reference because the value will go out of scope at the end of the function.

The following will not compile:
```rust
fn longest<'a>(x: &str, y: &str) -> &'a str {
    let result = String::from("really long string");
    result.as_str()
}
```

This implementation will fail to compile because the return value lifetime is not related to the lifetime of the parameters at all. `result` goes out of scope and gets cleaned up at the end of the `longest` function. We’re also trying to return a reference to `result` from the function.

The best fix would be to return an owned data type rather than a reference so the calling function is then responsible for cleaning up the value.

# Lifetime Annotations in Struct Definitions
We can define structs to hold references, but in that case we would need to add a lifetime annotation on every reference in the struct’s definition.

```rust
struct ImportantExcerpt<'a> {
    part: &'a str,
}

fn main() {
    let novel = String::from("Call me Ishmael. Some years ago...");
    let first_sentence = novel.split('.').next().expect("Could not find a '.'");
    let i = ImportantExcerpt {
        part: first_sentence,
    };
}
```

This annotation means an instance of `ImportantExcerpt` can’t outlive the reference it holds in its `part` field.

The `main` function here creates an instance of the `ImportantExcerpt` struct that holds a reference to the first sentence of the `String` owned by the variable `novel`. The data in `novel` exists before the `ImportantExcerpt` instance is created. In addition, `novel` doesn’t go out of scope until after the `ImportantExcerpt` goes out of scope, so the reference in the `ImportantExcerpt` instance is valid.

# Lifetime Elision
Consider the example from [[3) The Slice Type]]:
```rust
fn first_word(s: &str) -> &str {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }

    &s[..]
}
```

This function compiles without lifetime annotations for historical reasons. In Rust  1.0, the function signature would have been written like this:
```rust
fn first_word<'a>(s: &'a str) -> &'a str {
```

The Rust developers programmed these patterns into the compiler’s code so the borrow checker could infer the lifetimes in situations like these and wouldn’t need explicit annotations.

This piece of Rust history is relevant because it’s possible that more deterministic patterns will emerge and be added to the compiler. In the future, even fewer lifetime annotations might be required.

The patterns programmed into Rust’s analysis of references are called the _lifetime elision rules_. These aren’t rules for programmers to follow; they’re a set of particular cases that the compiler will consider, and if your code fits these cases, you don’t need to write the lifetimes explicitly.

The compiler uses three rules to figure out the lifetimes of the references when there aren’t explicit annotations:
1) The compiler assigns a lifetime parameter to each parameter that’s a reference.
	* A function with one parameter gets one lifetime parameter: `fn foo<'a>(x: &'a i32)`
	* A function with two parameters gets two separate lifetime parameters: `fn foo<'a, 'b>(x: &'a i32, y: &'b i32)`
2) If there is exactly one input lifetime parameter, that lifetime is assigned to all output lifetime parameters: `fn foo<'a>(x: &'a i32) -> &'a i32`.
3) If there are multiple input lifetime parameters, but one of them is `&self` or `&mut self` because this is a method, the lifetime of `self` is assigned to all output lifetime parameters.

## Example
### Example 1
The signature starts without any lifetimes associated with the references:
```rust
fn first_word(s: &str) -> &str {
```

The compiler applies the first rule, which specifies that each parameter gets its own lifetime. Calling it `'a` gives us:
```rust
fn first_word<'a>(s: &'a str) -> &str {
```

The second rule applies because there is exactly one input lifetime. The second rule specifies that the lifetime of the one input parameter gets assigned to the output lifetime, so the signature is now this:
```rust
fn first_word<'a>(s: &'a str) -> &'a str {
```
Now all the references in this function signature have lifetimes, and the compiler can continue its analysis without needing the programmer to annotate the lifetimes in this function signature.

### Example 2
Looking at our original, uncompilable `longest` function:
```rust
fn longest(x: &str, y: &str) -> &str {
```

Let’s apply the first rule - that each parameter gets its own lifetime:
```rust
fn longest<'a, 'b>(x: &'a str, y: &'b str) -> &str {
```

You can see that the second rule doesn’t apply because there is more than one input lifetime. The third rule doesn’t apply either, because `longest` is a function rather than a method, so none of the parameters are `self`.

After working through all three rules, we still haven’t figured out what the return type’s lifetime is. We hence get an error.

# Lifetime Annotations in Method Definitions


# The Static Lifetime
`'static`, is a special lifetime, which denotes that the affected reference _can_ live for the entire duration of the program. All string literals have the `'static` lifetime, which we can annotate as follows:
```rust
let s: &'static str = "I have a static lifetime.";
```

The text of this string is stored directly in the program’s binary, which is always available. Therefore, the lifetime of all string literals is `'static`.

You might see suggestions to use the `'static` lifetime in error messages. But before specifying `'static` as the lifetime for a reference, think about whether the reference you have actually lives the entire lifetime of your program or not, and whether you want it to.

Most of the time, an error message suggesting the `'static` lifetime results from attempting to create a dangling reference or a mismatch of the available lifetimes.

# Generic Type Parameters, Trait Bounds, and Lifetimes Together
The syntax of specifying generic type parameters, trait bounds, and lifetimes all in one function is:
```rust
use std::fmt::Display;

fn longest_with_an_announcement<'a, T>(
    x: &'a str,
    y: &'a str,
    ann: T,
) -> &'a str
where
    T: Display,
{
    println!("Announcement! {}", ann);
    if x.len() > y.len() {
        x
    } else {
        y
    }
}
```

This is the `longest` function that returns the longer of two string slices. But now it has an extra parameter named `ann` of the generic type `T`, which can be filled in by any type that implements the `Display` trait as specified by the `where` clause.

This extra parameter will be printed using `{}`, which is why the `Display` trait bound is necessary. Because lifetimes are a type of generic, the declarations of the lifetime parameter `'a` and the generic type parameter `T` go in the same list inside the angle brackets after the function name.

This could be used as follows:
```rust
fn main() {
    let string1 = String::from("abcd");
    let string2 = "xyz";

    let result = longest_with_an_announcement(
        string1.as_str(),
        string2,
        "Today is someone's birthday!",
    );
    println!("The longest string is {}", result);
}
```