_Slices_ let you reference a contiguous sequence of elements in a collection rather than the whole collection. A slice is a kind of reference, so it does not have ownership.

**Task:** write a function that takes a string of words separated by spaces and returns the first word it finds in that string. If the function doesn’t find a space in the string, the whole string must be one word, so the entire string should be returned.

The `first_word` function has a `&String` as a parameter. We don’t want ownership, so this is fine. But what should we return? We don’t really have a way to talk about _part_ of a string. However, we could return the index of the end of the word, indicated by a space.

```rust
fn first_word(s: &String) -> usize {
    //convert our `String` to an array of bytes using the `as_bytes` method
    let bytes = s.as_bytes(); 

    // create an iterator over the array of bytes using the `iter` method
    // The first element of the tuple returned from `enumerate` is the index
    // The second element is a reference to the element.
    for (i, &item) in bytes.iter().enumerate() { // Because we get a reference to the element from `.iter().enumerate()`, we use `&` in the pattern for the item.
        if item == b' ' { //byte literal syntax
            return i;
        }
    }

    s.len()
}
```

`i` and `s.len()` are only a meaningful number in the context of the `&String`.

```rust 
fn main() {
    let mut s = String::from("hello world");
    let word = first_word(&s); // word will get the value 5
    
    s.clear(); // this empties the String, making it equal to ""
    
    // word still has the value 5 here, but there's no more string that
    // we could meaningfully use the value 5 with. word is now totally invalid! }
```

Word can get out of sync. This gets worse if we try to track a second index as we have to track start and end indices.

Rust has a solution to this problem: string slices.

# String Slices
A _string slice_ is a reference to part of a `String`, and it looks like this:

```rust
fn main() {
    let s = String::from("hello world");

    let hello = &s[0..5];
    let world = &s[6..11];
}
```

Rather than a reference to the entire `String`, `hello` is a reference to a portion of the `String`, specified in the extra `[0..5]` bit. 

We create slices using a range within brackets by specifying `[starting_index..ending_index]`, where:
* `starting_index` is the first position in the slice
* `ending_index` is one more than the last position in the slice.

The slice data structure stores the starting position and the length of the slice, which corresponds to `ending_index` minus `starting_index`.

```rust
fn first_word(s: &String) -> &str {
    let bytes = s.as_bytes();
    
    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }
    
    &s[..] }
```

We are returning a string which makes more sense than returning a number. Now, if we try to clear s, we will get an error, letting us know that we did something wrong.:

```rust
fn main() {
    let mut s = String::from("hello world");

    let word = first_word(&s);

    s.clear(); // error!

    println!("the first word is: {}", word);
}
```

Recall from the borrowing rules that if we have an immutable reference to something, we cannot also take a mutable reference.

Because `clear` needs to truncate the `String`, it needs to get a mutable reference. The `println!` after the call to `clear` uses the reference in `word`, so the immutable reference must still be active at that point. Rust disallows the mutable reference in `clear` and the immutable reference in `word` from existing at the same time, and compilation fails.

## String Literals as Slices
Recall that we talked about string literals being stored inside the binary.

```rust
#![allow(unused)]
fn main() {
    let s = "Hello, world!";
}
```

The type of `s` here is `&str`: it’s a slice pointing to that specific point of the binary. This is also why string literals are immutable; `&str` is an immutable reference.

## String Slices as Parameters
Knowing that you can take slices of literals and `String` values leads us to one more improvement on `first_word`, and that’s its signature:

```rust
fn first_word(s: &str) -> &str {
```

If we have a string slice, we can pass that directly. If we have a `String`, we can pass a slice of the `String` or a reference to the `String`.

Defining a function to take a string slice instead of a reference to a `String` makes our API more general and useful without losing any functionality:

```rust
fn main() {
    let my_string = String::from("hello world");
    
    // `first_word` works on slices of `String`s, whether partial or whole
    let word = first_word(&my_string[0..6]);
    let word = first_word(&my_string[..]);
   
    // `first_word` also works on references to `String`s, which are equivalent
    // to whole slices of `String`s
    let word = first_word(&my_string); let my_string_literal = "hello world";
    
    // `first_word` works on slices of string literals, whether partial or whole
    let word = first_word(&my_string_literal[0..6]);
    let word = first_word(&my_string_literal[..]);
    
    // Because string literals *are* string slices already,
    // this works too, without the slice syntax!
    let word = first_word(my_string_literal); }
```
# Other Slices
String slices are specific to strings. But there’s a more general slice type too.

Consider this array:

```rust
let a = [1, 2, 3, 4, 5];
```

We might want to refer to part of an array. We’d do so like this:

```rust
let a = [1, 2, 3, 4, 5];

let slice = &a[1..3];

assert_eq!(slice, &[2, 3]);
```

This slice has the type `&[i32]`. It works the same way as string slices do, by storing a reference to the first element and a length.
# Summary
* The concepts of ownership, borrowing, and slices ensure memory safety in Rust programs at compile time.