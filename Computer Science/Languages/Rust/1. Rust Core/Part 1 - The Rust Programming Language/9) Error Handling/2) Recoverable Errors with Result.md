Most errors aren’t serious enough to require the program to stop entirely. Functions can fail in ways that are easily interpreted and responded to.

Recall that the Result enum in chapter 2 is defined as having two variants, `Ok` and `Err`, as follows:
```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

The `T` and `E` are generic type parameters.
* T represents the type of the value that will be returned in a success case within the `Ok` variant
* `E` represents the type of the value that will be returned in a failure case within the `Err` variant

Because `Result` has these generic type parameters, we can use the `Result` type and the functions defined on it in many different situations where the successful value and error value we want to return may differ.

Let's call a function that returns a `Result` value because it could fail:

```rust
use std::fs::File;

fn main() {
    let greeting_file_result = File::open("hello.txt");
}
```
The return type of `File::open` is a `Result<T, E>` - it might open the file or it might fail to open it
* The generic parameter `T` has been filled in by the implementation of `File::open` with the type of the success value, `std::fs::File`, which is a file handle.
	* In the case where `File::open` succeeds, the value in the variable `greeting_file_result` will be an instance of `Ok` that contains a file handle.
* The type of `E` used in the error value is `std::io::Error`.
	* In the case where it fails, the value in `greeting_file_result` will be an instance of `Err` that contains more information about the kind of error that happened.

To handle this, we do the following:
```rust
use std::fs::File;

fn main() {
    let greeting_file_result = File::open("hello.txt");

    let greeting_file = match greeting_file_result {
        Ok(file) => file,
        Err(error) => panic!("Problem opening the file: {:?}", error),
    };
}
```

The prelude is **the list of things that Rust automatically imports into every Rust program**. Note that, like the `Option` enum, the `Result` enum and its variants have been brought into scope by the prelude, so we don’t need to specify `Result::` before the `Ok` and `Err` variants in the `match` arms.

We then call a match that returns us the inner file from `Ok` or gets the error out of Err. In this case we chose to panic, though you may not necessarily do this.
# Matching on Different Errors
This code will panic no matter what how it fails. we may want to take different actions based on the type of error though. We can use **nested match statements** to do this.

If `File::open` failed because the file doesn’t exist, we want to create the file and return the handle to the new file. If `File::open` failed for any other reason—for example, because we didn’t have permission to open the file—we still want the code to `panic!` in the same way.

To do this you need to add an inner match statement.

```rust
use std::fs::File;
use std::io::ErrorKind;

fn main() {
    let greeting_file_result = File::open("hello.txt");

    let greeting_file = match greeting_file_result {
        Ok(file) => file,
        Err(error) => match error.kind() {
            ErrorKind::NotFound => match File::create("hello.txt") {
                Ok(fc) => fc,
                Err(e) => panic!("Problem creating the file: {:?}", e),
            },
            other_error => {
                panic!("Problem opening the file: {:?}", other_error);
            }
        },
    };
}
```

The type of the value that `File::open` returns inside the `Err` variant is `io::Error`, which is a struct provided by the standard library. This struct has a method `kind` that we can call to get an `io::ErrorKind` value.

The enum `io::ErrorKind` is provided by the standard library and has variants representing the different kinds of errors that might result from an `io` operation. The variant we want to use is `ErrorKind::NotFound`, which indicates the file we’re trying to open doesn’t exist yet. So we match on `greeting_file_result`, but we also have an inner match on `error.kind()`.

If it is returned, we try to create the file with `File::create`. However, because `File::create` could also fail, we need a second arm in the inner `match` expression. When the file can’t be created, a different error message is printed.

The second arm of the outer `match` stays the same, so the program panics on any error besides the missing file error.

# Alternatives to Using `match` with `Result<T, E>`
The `match` expression is very useful but also is a primitive. In Chapter 13, is about closures, which are used with many of the methods defined on `Result<T, E>`. These methods can be more concise than using `match` when handling `Result<T, E>` values in your code.

For example, here’s another way to write the same logic, this time using closures and the `unwrap_or_else` method:

```rust
use std::fs::File;
use std::io::ErrorKind;

fn main() {
    let greeting_file = File::open("hello.txt").unwrap_or_else(|error| {
        if error.kind() == ErrorKind::NotFound {
            File::create("hello.txt").unwrap_or_else(|error| {
                panic!("Problem creating the file: {:?}", error);
            })
        } else {
            panic!("Problem opening the file: {:?}", error);
        }
    });
}
```

Look up the `unwrap_or_else` method in the standard library documentation. Many more of these methods can clean up huge nested `match` expressions when you’re dealing with errors.

# Shortcuts for Panic on Error
## `unwrap`
The `Result<T, E>` type has many helper methods defined on it to do various, more specific tasks. The `unwrap` method is a shortcut method implemented just like the `match` expression we wrote. If the `Result` value is the `Ok` variant, `unwrap` will return the value inside the `Ok`. If the `Result` is the `Err` variant, `unwrap` will call the `panic!` macro for us. Here is an example of `unwrap` in action:
```rust
use std::fs::File;

fn main() {
    let greeting_file = File::open("hello.txt").unwrap();
}
```

If we run this code without a _hello.txt_ file, we’ll see an error message from the `panic!` call that the `unwrap` method makes:

```terminal
thread 'main' panicked at 'called `Result::unwrap()` on an `Err` value: Os {
code: 2, kind: NotFound, message: "No such file or directory" }',
src/main.rs:4:49
```

## `Expect`
Similarly, the `expect` method lets us also choose the `panic!` error message. Using `expect` instead of `unwrap` and providing good error messages can convey your intent and make tracking down the source of a panic easier. The syntax of `expect` looks like this:

```rust
use std::fs::File;

fn main() {
    let greeting_file = File::open("hello.txt")
        .expect("hello.txt should be included in this project");
}
```
We use `expect` in the same way as `unwrap`: to return the file handle or call the `panic!` macro. The error message used by `expect` in its call to `panic!` will be the parameter that we pass to `expect`, rather than the default `panic!` message that `unwrap` uses:
```terminal
thread 'main' panicked at 'hello.txt should be included in this project: Os {
code: 2, kind: NotFound, message: "No such file or directory" }',
src/main.rs:5:10
```

# Propagating Errors
When a function’s implementation calls something that might fail, instead of handling the error within the function itself, you can return the error to the calling code so that it can decide what to do. This is known as _propagating_ the error.

This gives more control to the calling code, where there might be more information or logic that dictates how the error should be handled than what you have available in the context of your code.

For example, here is a function that reads a username from a file. If the file doesn’t exist or can’t be read, this function will return those errors to the code that called the function:

```rust
use std::fs::File;
use std::io::{self, Read};

fn read_username_from_file() -> Result<String, io::Error> {
    let username_file_result = File::open("hello.txt");

    let mut username_file = match username_file_result {
        Ok(file) => file,
        Err(e) => return Err(e),
    };

    let mut username = String::new();

    match username_file.read_to_string(&mut username) {
        Ok(_) => Ok(username),
        Err(e) => Err(e),
    }
}
```

Look at the return type of the function first: `Result<String, io::Error>`.
On success we return a string.
We chose `io::Error` as the return type of this function because that happens to be the type of the error value returned from both of the operations we’re calling in this function’s body that might fail: the `File::open` function and the `read_to_string` method.

1) The body of the function starts by calling the `File::open` function.
2) We handle the `Result` value with a `match`
3) We need a match statement to handle this `Result` value
	* If `File::open` succeeds, the file handle in the pattern variable `file` becomes the value in the mutable variable `username_file` and the function continues.
	* In the `Err` case, instead of calling `panic!`, we use the `return` keyword to return early out of the function entirely and pass the error value from `File::open`, now in the pattern variable `e`, back to the calling code as this function’s error value.
4) If we have a file handle in `username_file`, the function then creates a new `String` in variable `username`
5) Then call the `read_to_string` method on the file handle in `username_file` to read the contents of the file into `username`
6) The `read_to_string` method also returns a `Result` because it might fail, even though `File::open` succeeded. So we need another `match` to handle that `Result`. We don’t need to explicitly say `return`, because this is the last expression in the function.
	* If `read_to_string` succeeds, then our function has succeeded, and we return the username from the file that’s now in `username` wrapped in an `Ok`.
	* If `read_to_string` fails, we return the error value.

This pattern of propagating errors is so common in Rust that Rust provides the question mark operator `?` to make this easier.
## A Shortcut for Propagating Errors: the ? Operator
The following has the same functionality but implements the `?` operator:
```rust
use std::fs::File;
use std::io::{self, Read};

fn read_username_from_file() -> Result<String, io::Error> {
    let mut username_file = File::open("hello.txt")?;
    let mut username = String::new();
    username_file.read_to_string(&mut username)?;
    Ok(username)
}
```

The `?` placed after a `Result` value is defined to work in almost the same way as the `match` expressions we defined to handle the `Result` values:
* If the value of the `Result` is an `Ok`, the value inside the `Ok` will get returned from this expression, and the program will continue.
* If the value is an `Err`, the `Err` will be returned from the whole function as if we had used the `return` keyword so the error value gets propagated to the calling code.

The difference between `match` and the `?` operator is that error values that have the `?` operator called on them go through the `from` function, defined in the `From` trait in the standard library, which is used to convert values from one type into another.
When the `?` operator calls the `from` function, the error type received is converted into the error type defined in the return type of the current function.

In this context, the `?` at the end of the `File::open` call will return the value inside an `Ok` to the variable `username_file`. If an error occurs, the `?` operator will return early out of the whole function and give any `Err` value to the calling code. The same thing applies to the `?` at the end of the `read_to_string` call.

We could even shorten this code further by chaining method calls immediately after the `?`:
```rust
use std::fs::File;
use std::io::{self, Read};

fn read_username_from_file() -> Result<String, io::Error> {
    let mut username = String::new();

    File::open("hello.txt")?.read_to_string(&mut username)?;

    Ok(username)
}
```

We could also just do:
```rust
use std::fs;
use std::io;

fn read_username_from_file() -> Result<String, io::Error> {
    fs::read_to_string("hello.txt")
}
```

## Where The `?` Operator Can Be Used
The `?` operator can only be used in functions whose return type is compatible with the value the `?` is used on. This is because the `?` operator is defined to perform an early return of a value out of the function, in the same manner as the `match` expression defined earlier.

We get an error if we use the `?` operator in a `main` function with a return type incompatible with the type of the value we use `?` on:
```rust
use std::fs::File;

fn main() {
    let greeting_file = File::open("hello.txt")?;
}
```

The `?` operator follows the `Result` value returned by `File::open`, but this `main` function has the return type of `()`, not `Result`. We thus get an error message:
```terminal
$ cargo run
   Compiling error-handling v0.1.0 (file:///projects/error-handling)
error[E0277]: the `?` operator can only be used in a function that returns `Result` or `Option` (or another type that implements `FromResidual`)
 --> src/main.rs:4:48
  |
3 | fn main() {
  | --------- this function should return `Result` or `Option` to accept `?`
4 |     let greeting_file = File::open("hello.txt")?;
  |                                                ^ cannot use the `?` operator in a function that returns `()`
  |
  = help: the trait `FromResidual<Result<Infallible, std::io::Error>>` is not implemented for `()`

For more information about this error, try `rustc --explain E0277`.
error: could not compile `error-handling` due to previous error
```

This error points out that we’re only allowed to use the `?` operator in a function that returns `Result`, `Option`, or another type that implements `FromResidual`.

To fix the error, you have two choices. One choice is to change the return type of your function to be compatible with the value you’re using the `?` operator on as long as you have no restrictions preventing that. The other technique is to use a `match` or one of the `Result<T, E>` methods to handle the `Result<T, E>` in whatever way is appropriate.

As with using `?` on `Result`, you can only use `?` on `Option` in a function that returns an `Option`.
*  if the value is `None`, the `None` will be returned early from the function at that point.
* If the value is `Some`, the value inside the `Some` is the resulting value of the expression and the function continues.

For example:
```rust
fn last_char_of_first_line(text: &str) -> Option<char> {
    text.lines().next()?.chars().last()
}
```

This is an `Option` because it’s possible that the first line is the empty string, for example if `text` starts with a blank line but has characters on other lines, as in `"\nhi"`.

This code:
1) Takes the `text` string slice argument and calls the `lines` method on it
	* This returns an iterator over the lines in the string.
2) This function wants to examine the first line so it calls `next` on the iterator to get the first value from the iterator.
	* If `text` is the empty string, this call to `next` will return `None`, in which case we use `?` to stop and return `None` from `last_char_of_first_line`.
	* If `text` is not the empty string, `next` will return a `Some` value containing a string slice of the first line in `text`.
3) If there are values we turn the string into an iterator of characters using `.chars()`
4) Extract the last character.

## Main function
So far, all the `main` functions we’ve used return `()`. The `main` function is special because it’s the entry and exit point of executable programs, and there are restrictions on what its return type can be for the programs to behave as expected.

`main` can also return a `Result<(), E>`. For example, the following will compile:
```rust
use std::error::Error;
use std::fs::File;

fn main() -> Result<(), Box<dyn Error>> {
    let greeting_file = File::open("hello.txt")?;

    Ok(())
}
```

The `Box<dyn Error>` type is a _trait object_, which will be talked about in chapter 17. When a `main` function returns a `Result<(), E>`, the executable will exit with a value of `0` if `main` returns `Ok(())` and will exit with a nonzero value if `main` returns an `Err` value.

Executables written in C return integers when they exit: programs that exit successfully return the integer `0`, and programs that error return some integer other than `0`. Rust also returns integers from executables to be compatible with this convention.

The `main` function may return any types that implement [the `std::process::Termination` trait](https://doc.rust-lang.org/std/process/trait.Termination.html), which contains a function `report` that returns an `ExitCode`.