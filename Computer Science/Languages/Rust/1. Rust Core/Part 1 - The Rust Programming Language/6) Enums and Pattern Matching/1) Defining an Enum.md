* Structs give you a way of grouping together related fields and data
	* Like a Rectangle with its width and height
* Enums give you a way of saying a value is one of a possible set of values.
	* We may want to say that `Rectangle` is one of a set of possible shapes that also includes `Circle` and `Triangle`.

Say we need to work with IP addresses.
Currently, two major standards are used for IP addresses: version four and version six.
* An IP address cannot be both at the same time
* These are the only possibilities for an IP address that our program will come across
* We can _enumerate_ all possible variants, which is where enumeration gets its name.
* Both are fundamentally IP addresses so should be treated as the same type in situations that apply

You can do an enum as follows:

```rust
enum IpAddressKind {
    V4,
    V6,
}
```

`IpAddrKind` is now a custom data type that we can use elsewhere in our code.

# Enum Values
We can create instances of each of the two variants of IpAddrKind like this:
```rust
let four = IpAddrKind::V4;
let six = IpAddrKind::V6;
```

 now both values `IpAddrKind::V4` and `IpAddrKind::V6` are of the same type: `IpAddrKind`. We can then, for instance, define a function that takes any `IpAddrKind`:
 ```rust
 fn route(ip_kind: IpAddrKind) {}
```
And we can call this function with either variant:
```terminal
route(IPAddrKind::V4);
route(IPAddrKind::V6);
```

Using enums has more advantages. You might be tempted to tackle this problem with structs:

```rust
enum IpAddrKind {
    V4,
    V6,
}

struct IpAddr {
    kind: IpAddrKind,
    address: String,
}

fn main() {
	let home = IpAddr {
	    kind: IpAddrKind::V4,
	    address: String::from("127.0.0.1"),
	};
	
	let loopback = IpAddr {
	    kind: IpAddrKind::V6,
	    address: String::from("::1"),
	};
}
```

Representing the same concept using just an enum is more concise:

```rust
enum IpAddr {
    V4(String),
    V6(String),
}

let home = IpAddr::V4(String::from("127.0.0.1"));

let loopback = IpAddr::V6(String::from("::1"));
```

We attach data to each variant of the enum directly, so there is no need for an extra struct.

The name of each enum variant that we define also becomes a function that constructs an instance of the enum.
That is, `IpAddr::V4()` is a function call that takes a `String` argument and returns an instance of the `IpAddr` type.

There’s another advantage to using an enum rather than a struct: each variant can have different types and amounts of associated data.

Version four IP addresses will always have four numeric components that will have values between 0 and 255. If we wanted to store `V4` addresses as four `u8` values but still express `V6` addresses as one `String` value, we wouldn’t be able to with a struct. Enums handle this case with ease:

```rust
enum IpAddr {
    V4(u8, u8, u8, u8),
    V6(String),
}

fn main() {
    let home = IpAddr::V4(127, 0, 0, 1);

    let loopback = IpAddr::V6(String::from("::1"));
}
```

The standard library already defines IpAddr. It does it as follows:

```rust
struct Ipv4Addr {
    // --snip--
}

struct Ipv6Addr {
    // --snip--
}

enum IpAddr {
    v4(Ipv4Addr)
    v6(Ipv6Addr)
}
```

This shows that you can put any kind of data inside of an enum variant, even another enum.

We can create and use our own definition even though the standard library contains one as we haven't brought the std one into our scope.

Let's look at an enum with a wide variety of types embedded in its variants:

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32)
}
```

This enum has four variants with different types:
* `Quit` has no data associated with it at all.
* `Move` has named fields, like a struct does.
* `Write` includes a single `String`.
* `ChangeColor` includes three `i32` values.

The following structs could hold the same data that the preceding enum variants hold:

```rust
struct QuitMessage; //unit struct
struct MoveMessage {
    x: i32,
    y: i32,
}
struct WriteMessage(String); // truple struct
struct ChangeColorMessage(i32, i32, i32) //tuple struct
```

But if we did this, we couldn't define a function to take any of these kinds of messages as easily as with the enum.

We can define methods on enums as we do with structs. The following is a method named `call` that we could define on our `Message` enum:

```rust
impl Message {
    fn call(&self) {
        // method body would be defined here
    }
}

let m = Message::Write(String::from("hello"));
m.call()
```

The body of the method would use `self` to get the value that we called the method on.

# The Option Enum and Its Advantages Over Null Values
`Option` is another enum defined by the standard library. The `Option` type encodes the very common scenario in which a value could be something or it could be nothing.

For example, if you request the first item in a list:
* In an empty list you would get nothing
* In a non-empty list you would get a value

Expressing this concept in terms of the type system means the compiler can check if you've handled all the cases you should be handling. This can prevent bugs.

Rust doesn’t have the null feature that many other languages have. In languages with null, variables can always be in one of two states: null or not-null.

The problem with null values is that if you try to use it as a not-null value, you'll get an error. It is extremely easy to make this kind of error.

The concept that null is trying to express is useful though: a null is a value that is currently invalid or absent for some reason.

The implementation is the problem. Rust therefore does not have nulls, but it does have an enum that can encode the concept of a value being present or absent.

This enum is `Option<T>`, and it is [defined by the standard library](https://doc.rust-lang.org/std/option/enum.Option.html) as follows:

```rust
enum Option<T> {
    None,
    Some(T),
}
```

It is so useful that it is included in the prelude; you don't need to bring it into scope explicitly. Its variants are also included in the prelude: you can use `Some` and `None` directly without the `Option::` prefix.

The `Option<T>` enum is still just a regular enum, and `Some(T)` and `None` are still variants of type `Option<T>`.

The `<T>` syntax is a generic type parameter. Generics are covered in more detail in Chapter 10. For now, know that `<T>` means that the `Some` variant of the `Option` enum can hold one piece of data of any type, and that each concrete type that gets used in place of `T` makes the overall `Option<T>` type a different type.

Some examples of using `Option` values to hold number types and string types:

```rust
let some_nmber = Some(5);
let some_char = Some('e');

let absent_number: Option<i32> = None;
```

The type of `some_number` is `Option<i32>`. The type of `some_char` is `Option<char>`, which is a different type. Rust can infer these types because we’ve specified a value inside the `Some` variant.

For `absent_number`, Rust requires us to annotate the overall `Option` type: the compiler can’t infer the type that the corresponding `Some` variant will hold by looking only at a `None` value. Here, we tell Rust that we mean for `absent_number` to be of type `Option<i32>`.

When we have a `Some` value, we know that a value is present and the value is held within the `Some`. When we have a `None` value, in some sense it means the same thing as null: we don’t have a valid value.

Why is having `Option<T>` any better than having null? Look at the following:

```rust
let x: i8 = 5;
let y: Option<i8> = Some(5);
let sum = x + y;
```

This panics. the error message means that Rust doesn't understand how to add an `i8` and an `Option<i8>`, because they are different types.

When we have a value of a type like `i8` in Rust, the compiler will ensure that we always have a valid value so we can proceed confidently without having to check for null before using that value. Only with `Option<i8>` (or what value we're working with) do we have to worry about possibly not having a value. The compiler will make sure we handle that case before using the value.

We need to convert an `Option<T>` to a `T` before we can perform `T` operations with it. This generally catches bugs where we assume something isn't null when it actually is.

In order to have a value that can possibly be null, you must explicitly opt in by making the type of that value `Option<T>`. Then, when you use that value, you are required to explicitly handle the case when the value is null.

Everywhere that a value has a type that isn’t an `Option<T>`, you _can_ safely assume that the value isn’t null. This was a deliberate design decision for Rust to limit null’s pervasiveness and increase the safety of Rust code.

Becoming familiar with the methods on `Option<T>`[its documentation](https://doc.rust-lang.org/std/option/enum.Option.html) will be extremely useful in your journey with Rust.

In general, in order to use an `Option<T>` value, you want to have code that will handle each variant.

You want some code that will run only when you have a `Some(T)` value, and this code is allowed to use the inner `T`. You want some other code to run only if you have a `None` value, and that code doesn’t have a `T` value available.

The `match` expression is a control flow construct that does just this when used with enums: it will run different code depending on which variant of the enum it has, and that code can use the data inside the matching value.