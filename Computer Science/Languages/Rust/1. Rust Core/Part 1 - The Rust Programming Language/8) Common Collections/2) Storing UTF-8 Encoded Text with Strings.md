
# What is a String?
Rust has only one string type in the core language, which is the string slice `str` that is usually seen in its borrowed form `&str`. _String slices_ are references to some UTF-8 encoded string data stored elsewhere. String literals, for example, are stored in the program’s binary and are therefore string slices.

The `String` type, which is provided by Rust’s standard library rather than coded into the core language, is a growable, mutable, owned, UTF-8 encoded string type.

Many of the same operations available with `Vec<T>` are available with `String` as well, because `String` is actually implemented as a wrapper around a vector of bytes with some extra guarantees, restrictions, and capabilities.

# Creating a New String
To create a new string, you do the following:
```rust
fn main() {
    let mut s = String::new();
}
```

Often, we’ll have some initial data that we want to start the string with. For that, we use the `to_string` method, which is available on any type that implements the `Display` trait:
```rust
fn main() {
    let data = "initial contents";

    let s = data.to_string();

    // the method also works on a literal directly:
    let s = "initial contents".to_string();
}
```

We can also use the function `String::from` to create a `String` from a string literal:
```rust
fn main() {
    let s = String::from("initial contents");
}
```

Remember that strings are UTF-8 encoded, so we can include any properly encoded data in them. All of the following are valid strings:
```rust
fn main() {
    let hello = String::from("السلام عليكم");
    let hello = String::from("Dobrý den");
    let hello = String::from("Hello");
    let hello = String::from("שָׁלוֹם");
    let hello = String::from("नमस्ते");
    let hello = String::from("こんにちは");
    let hello = String::from("안녕하세요");
    let hello = String::from("你好");
    let hello = String::from("Olá");
    let hello = String::from("Здравствуйте");
    let hello = String::from("Hola");
}
```

# Updating a String
* A `String` can grow in size and its contents can change, just like the contents of a `Vec<T>`, if you push more data into it.
* You can use the `+` operator or the `format!` macro to concatenate `String` values.

## Appending to a String with `push_str` and push
We can grow a `String` by using the `push_str` method to append a string slice:
```rust
fn main() {
    let mut s = String::from("foo");
    s.push_str("bar");
}
```

The `push_str` method takes a string slice because we don’t necessarily want to take ownership of the parameter. As an example of this:
```rust
fn main() {
    let mut s1 = String::from("foo");
    let s2 = "bar";
    s1.push_str(s2);
    println!("s2 is {s2}");
}
```

The `push` method takes a single character as a parameter and adds it to the `String`.
```rust
fn main() {
    let mut s = String::from("lo");
    s.push('l');
}
```

## Concatenation with the `+` Operator or the `format!` Macro
Often, you’ll want to combine two existing strings. One way to do so is to use the `+` operator:
```rust
fn main() {
    let s1 = String::from("Hello, ");
    let s2 = String::from("world!");
    let s3 = s1 + &s2; // note s1 has been moved here and can no longer be used
}
```

`s1` is no longer valid after the addition, and we used a reference to `s2`, because of the signature of the method that’s called when we use the `+` operator. The `+` operator uses the `add` method, whose signature looks something like this:
```rust
fn add(self, s: &str) -> String {
```

The type of `&s2` is `&String`, not `&str`, as specified in the second parameter to `add`. So why does it compile?

The reason is that the compiler can _coerce_ the `&String` argument into a `&str`. When we call the `add` method, Rust uses a _deref coercion_, which here turns `&s2` into `&s2[..]`. We’ll discuss deref coercion in more depth in Chapter 15. Because `add` does not take ownership of the `s` parameter, `s2` will still be a valid `String` after this operation.

Note that it takes ownership of the first string. It looks like it’s making a lot of copies but isn’t; the implementation is more efficient than copying.

### Adding Multiple Strings
If we need to concatenate multiple strings, the behaviour of the `+` operator gets unwieldy:
```rust
fn main() {
    let s1 = String::from("tic");
    let s2 = String::from("tac");
    let s3 = String::from("toe");

    let s = s1 + "-" + &s2 + "-" + &s3;
}
```

For more complicated string combining, we can instead use the `format!` macro:
```rust
fn main() {
    let s1 = String::from("tic");
    let s2 = String::from("tac");
    let s3 = String::from("toe");

    let s = format!("{s1}-{s2}-{s3}");
}
```

```rust
fn main() {
    let s1 = String::from("tic");
    let s2 = String::from("tac");
    let s3 = String::from("toe");

    let s = format!("{s1}-{s2}-{s3}");
}
```

# Indexing into Strings
In many other programming languages, accessing individual characters in a string by referencing them by index is a valid and common operation.

However, if you try to access parts of a `String` using indexing syntax in Rust, you’ll get an error:
```rust
fn main() {
    let s1 = String::from("hello");
    let h = s1[0];
}
```

```terminal
$ cargo run
   Compiling collections v0.1.0 (file:///projects/collections)
error[E0277]: the type `String` cannot be indexed by `{integer}`
 --> src/main.rs:3:13
  |
3 |     let h = s1[0];
  |             ^^^^^ `String` cannot be indexed by `{integer}`
  |
  = help: the trait `Index<{integer}>` is not implemented for `String`
  = help: the following other types implement trait `Index<Idx>`:
            <String as Index<RangeFrom<usize>>>
            <String as Index<RangeFull>>
            <String as Index<RangeInclusive<usize>>>
            <String as Index<RangeTo<usize>>>
            <String as Index<RangeToInclusive<usize>>>
            <String as Index<std::ops::Range<usize>>>

For more information about this error, try `rustc --explain E0277`.
error: could not compile `collections` due to previous error
```

Rust strings don’t support indexing. But why not?

To answer that question, we need to discuss how Rust stores strings in memory.

## Internal Representation
A `String` is a wrapper over a `Vec<u8>`. Consider:
```rust
fn main() {
    let hello = String::from("Hola");
}
```

In this case, `len` will be 4 - `Hola` is 4 bytes long in UTF-8 encoding.

The following looks at first like being 12 chars long, but it is actually 24:
```rust
	let hello = String::from("Здравствуйте");
```

It takes 24 bytes to encode “Здравствуйте” in UTF-8, because each Unicode scalar value in that string takes 2 bytes of storage.

Consider this invalid Rust code:
```rust
fn main() {
    let hello = "Здравствуйте";
    let answer = &hello[0];
}
```

When encoded in UTF-8, the first byte of `З` is `208` and the second is `151`, so it would seem that `answer` should in fact be `208`, but `208` is not a valid character on its own.

Returning `208` is likely not what a user would want if they asked for the first letter of this string; however, that’s the only data that Rust has at byte index 0.

The answer, to avoid returning an unexpected value and causing bugs that might not be discovered immediately, is to not compile this code at all. This prevents misunderstandings early in the development process.

## Bytes and Scalar Values and Grapheme Clusters
In UTF-8 is that there are actually three relevant ways to look at strings from Rust’s perspective: as bytes, scalar values, and grapheme clusters (the closest thing to what we would call _letters_).

If we look at the Hindi word “नमस्ते” written in the Devanagari script, it is stored as a vector of `u8` values that looks like this:
```terminal
[224, 164, 168, 224, 164, 174, 224, 164, 184, 224, 165, 141, 224, 164, 164, 224, 165, 135]
```

This is how computers ultimately store this value.

If we look at them as Unicode scalar values, which are what Rust’s `char` type is, those bytes look like this:
```terminal
['न', 'म', 'स', '्', 'त', 'े']
```

There are six `char` values here, but the fourth and sixth are not letters: they’re diacritics that don’t make sense on their own. Finally, if we look at them as grapheme clusters, we’d get what a person would call the four letters that make up the Hindi word:
```terminal
["न", "म", "स्", "ते"]
```

Rust does not know what one to choose, so we cannot index.

A final reason Rust doesn’t allow us to index into a `String` to get a character is that indexing operations are expected to always take constant time (O(1)).

It isn’t possible to guarantee that performance with a `String`, because Rust would have to walk through the contents from the beginning to the index to determine how many valid characters there were.

# Slicing Strings
Indexing into a string is often a bad idea because it’s not clear what the return type of the string-indexing operation should be: a byte value, a character, a grapheme cluster, or a string slice. If you really need to use indices to create string slices, therefore, Rust asks you to be more specific.

Rather than indexing using `[]` with a single number, you can use `[]` with a range to create a string slice containing particular bytes:
```rust
#![allow(unused)]
fn main() {
let hello = "Здравствуйте";

let s = &hello[0..4];
}
```

Here, `s` will be a `&str` that contains the first 4 bytes of the string. Earlier, we mentioned that each of these characters was 2 bytes, which means `s` will be `Зд`.

If we were to try to slice only part of a character’s bytes with something like `&hello[0..1]`, Rust would panic at runtime in the same way as if an invalid index were accessed in a vector:
```terminal
$ cargo run
   Compiling collections v0.1.0 (file:///projects/collections)
    Finished dev [unoptimized + debuginfo] target(s) in 0.43s
     Running `target/debug/collections`
thread 'main' panicked at 'byte index 1 is not a char boundary; it is inside 'З' (bytes 0..2) of `Здравствуйте`', src/main.rs:4:14
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

You should use ranges to create string slices with caution, because this can crash your program.

# Methods for Iterating Over Strings
The best way to operate on pieces of strings is to be explicit about whether you want characters or bytes.
For individual Unicode scalar values, use the `chars` method. Calling `chars` on “Зд” separates out and returns two values of type `char`, and you can iterate over the result to access each element:
```rust
#![allow(unused)]
fn main() {
for c in "Зд".chars() {
    println!("{c}");
}
}
```

This code will print the following:
```terminal
З
д
```

Alternatively, the `bytes` method returns each raw byte, which might be appropriate for your domain:
```rust
#![allow(unused)]
fn main() {
for b in "Зд".bytes() {
    println!("{b}");
}
}
```

This code will print the four bytes that make up this string:
```terminal
208
151
208
180
```

Be sure to remember that valid Unicode scalar values may be made up of more than 1 byte.

Getting grapheme clusters from strings as with the Devanagari script is complex, so this functionality is not provided by the standard library. Crates are available on [crates.io](https://crates.io/) if this is the functionality you need.

# Strings are Not So Simple

