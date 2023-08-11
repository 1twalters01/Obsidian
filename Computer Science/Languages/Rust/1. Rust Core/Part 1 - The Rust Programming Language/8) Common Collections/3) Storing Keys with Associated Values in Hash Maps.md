The last of our common collections is the _hash map_. The type `HashMap<K, V>` stores a mapping of keys of type `K` to values of type `V` using a _hashing function_, which determines how it places these keys and values into memory.

Many programming languages support this kind of data structure, but they often use a different name, such as hash, map, object, hash table, dictionary, or associative array, just to name a few.

# Creating a New Hash Map
One way to create an empty hash map is using `new` and adding elements with `insert`.

As an example, to keep track of the scores of two teams whose names are _Blue_ and _Yellow_. where the Blue team starts with 10 points, and the Yellow team starts with 50, we do:

```rust
fn main() {
    use std::collections::HashMap;

    let mut scores = HashMap::new();

    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Yellow"), 50);
}
```

Note that we need to first `use` the `HashMap` from the collections portion of the standard library. They are used less so they are not included in the prelude. Hash maps also have less support from the standard library; there’s no built-in macro to construct them, for example.

Just like vectors, hash maps store their data on the heap. Like vectors, **hash maps are homogeneous**: all of the keys must have the same type as each other, and all of the values must have the same type.

# Accessing Values in a Hash Map
We can get a value out of the hash map by providing its key to the `get` method:

```rust
fn main() {
    use std::collections::HashMap;

    let mut scores = HashMap::new();

    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Yellow"), 50);

    let team_name = String::from("Blue");
    let score = scores.get(&team_name).copied().unwrap_or(0);
}
```

Here, `score` will have the value that’s associated with the Blue team, and the result will be `10`.
The `get` method returns an `Option<&V>`; if there’s no value for that key in the hash map, `get` will return `None`.
This program handles the `Option` by calling `copied` to get an `Option<i32>` rather than an `Option<&i32>`, then `unwrap_or` to set `score` to zero if `scores` doesn't have an entry for the key.

We can iterate over each key/value pair in a hash map in a similar manner as we do with vectors, using a `for` loop:
```rust
    use std::collections::HashMap;

    let mut scores = HashMap::new();

    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Yellow"), 50);

    for (key, value) in &scores {
        println!("{key}: {value}");
    }
```

This code will print each pair in an arbitrary order.

# Hash Maps and Ownership
For types that implement the `Copy` trait, like `i32`, the values are copied into the hash map. For owned values like `String`, the values will be moved and the hash map will be the owner of those values:

```rust
fn main() {
    use std::collections::HashMap;

    let field_name = String::from("Favorite color");
    let field_value = String::from("Blue");

    let mut map = HashMap::new();
    map.insert(field_name, field_value);
    // field_name and field_value are invalid at this point, try using them and
    // see what compiler error you get!
}
```

We aren’t able to use the variables `field_name` and `field_value` after they’ve been moved into the hash map with the call to `insert`.

If we insert references to values into the hash map, the values won’t be moved into the hash map. The values that the references point to must be valid for at least as long as the hash map is valid.
These issues are discussed in  [[3) Validating References with Lifetimes]].

# Updating a Hash Map
When you want to change the data in a hash map, you have to decide how to handle the case when a key already has a value assigned. You could:
* replace the old value with the new value, completely disregarding the old value.
* keep the old value and ignore the new value, only adding the new value if the key _doesn’t_ already have a value.
* combine the old value and the new value.

## Overwriting a Value
If we insert a key and a value into a hash map and then insert that same key with a different value, the value associated with that key will be replaced.

```rust
fn main() {
    use std::collections::HashMap;

    let mut scores = HashMap::new();

    scores.insert(String::from("Blue"), 10);
    scores.insert(String::from("Blue"), 25);

    println!("{:?}", scores);
}
```

This code will print `{"Blue": 25}`. The original value of `10` has been overwritten.


## Adding a Key and Value Only If a Key Isn't Present
In this case:
* If the key does exist in the hash map, the existing value should remain the way it is.
* If the key doesn’t exist, insert it and a value for it.

Hash maps have a special API for this called `entry` that takes the key you want to check as a parameter.
The return value of the `entry` method is an enum called `Entry` that represents a value that might or might not exist.

Let’s say we want to check whether the key for the Yellow team has a value associated with it. If it doesn’t, we want to insert the value 50, and the same for the Blue team. We would do:

```rust
fn main() {
    use std::collections::HashMap;

    let mut scores = HashMap::new();
    scores.insert(String::from("Blue"), 10);

    scores.entry(String::from("Yellow")).or_insert(50);
    scores.entry(String::from("Blue")).or_insert(50);

    println!("{:?}", scores);
}
```

## Updating a Value Based on the Old Value
Another common use case for hash maps is to look up a key’s value and then update it based on the old value.

Say we were counting how many times each word in a text appeared:
```rust
fn main() {
    use std::collections::HashMap;

    let text = "hello world wonderful world";

    let mut map = HashMap::new();

    for word in text.split_whitespace() {
        let count = map.entry(word).or_insert(0);
        *count += 1;
    }

    println!("{:?}", map);
}
```

* The `split_whitespace` method returns an iterator over sub-slices, separated by whitespace, of the value in `text`.
* The `or_insert` method returns a mutable reference (`&mut V`) to the value for the specified key.
* Here we store that mutable reference in the `count` variable, so in order to assign to that value, we must first **dereference** `count` using the asterisk (`*`).
	* The mutable reference goes out of scope at the end of the `for` loop, so all of these changes are safe and allowed by the borrowing rules.

# Hashing Functions
By default, `HashMap` uses a hashing function called [_SipHash_](https://en.wikipedia.org/wiki/SipHash) that can provide resistance to Denial of Service (DoS) attacks involving hash tables. This is not the fastest hashing algorithm available, but the trade-off for better security that comes with the drop in performance is worth it.

If you find that the default hash function is too slow for your purposes, you can switch to another function by specifying a different hasher. A _hasher_ is a type that implements the `BuildHasher` trait. Traits and how to implement them are in Chapter 10.

You don’t necessarily have to implement your own hasher from scratch; [crates.io](https://crates.io/) has libraries shared by other Rust users that provide hashers implementing many common hashing algorithms.

# Exercises
Here are some exercises you should now be equipped to solve:
- Given a list of integers, use a vector and return the median (when sorted, the value in the middle position) and mode (the value that occurs most often; a hash map will be helpful here) of the list.
- Convert strings to pig latin. The first consonant of each word is moved to the end of the word and “ay” is added, so “first” becomes “irst-fay.” Words that start with a vowel have “hay” added to the end instead (“apple” becomes “apple-hay”). Keep in mind the details about UTF-8 encoding!
- Using a hash map and vectors, create a text interface to allow a user to add employee names to a department in a company. For example, “Add Sally to Engineering” or “Add Amir to Sales.” Then let the user retrieve a list of all people in a department or all people in the company by department, sorted alphabetically.