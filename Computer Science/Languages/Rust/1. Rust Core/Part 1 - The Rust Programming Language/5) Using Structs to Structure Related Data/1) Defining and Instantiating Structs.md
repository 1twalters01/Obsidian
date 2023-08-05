Structs are similar to tuples, in that both hold multiple related values. Unlike with tuples, in a struct you'll name each piece of data so it's clear what the values mean. As an example, to define a struct for a user, we do  following:

```rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}
```

To use the struct we do the following:

```rust
fn main() {
    let user1 = User {
        active: true,
        username: String::from("someusername123"),
        email: String::from("someone@example.com"),
        sign_in_count: 1,
    };
}
```

To get a specific value from a struct, we use dot notation. As an example, to change the user's email, we do:

```rust
fn main() {
    let mut user1 = User {
        active: true,
        username: String::from("someusername123"),
        email: String::from("someone@example.com"),
        sign_in_count: 1,
    };
    user1.email = String::from("anotheremail@example.com");
}

```

Note that the instance must be mutable. Rust doesn't allow us to mark only certain fields as mutable.

We can construct a new instance of the struct as the last expression in the function body to implicitly return that new instance.

```rust
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username: username,
        email: email,
        sign_in_count: 1,
    }
}
```
# Using the Field Init Shorthand
Since the parameter names and the struct field names are exactly the same, we can use the `field init shorthand` syntax to rewrite `build_user` to not repeat username and email:

```rust
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username,
        email,
        sign_in_count: 1,
    }
}
```

# Creating Instances form Other Instances with Struct Update Syntax
It's often useful to create a new instance of a struct that includes most of the values from another instance, but changes soem. You can do this with the `struct update syntax`.

```rust
fn main() {
    --snip--

    let user2 = User {
        active: user1.active,
        username: user1.username,
        email: String::from("another@example.com"),
        sign_in_count: user1.sign_in_count,
    }
```



# Using Typle Structs Without Named Fields to Create Different Types












# Unit-Like Structs Without Any Fields











# Ownership of Struct Data



















