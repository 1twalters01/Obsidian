In his 1972 essay [“The Humble Programmer,”](https://www.cs.utexas.edu/~EWD/transcriptions/EWD03xx/EWD340.html) Edsger W. Dijkstra said that “Program testing can be a very effective way to show the presence of bugs, but it is hopelessly inadequate for showing their absence.” That doesn’t mean we shouldn’t try to test as much as we can!

Rust’s type system shoulders a huge part of this burden, but the type system cannot catch everything. As such, Rust includes support for writing automated software tests.

Tests are Rust functions that verify that the non-test code is functioning in the expected manner. The bodies of test functions typically perform these three actions:
1. Set up any needed data or state.
2. Run the code you want to test.
3. Assert the results are what you expect.

# The Anatomy of a Test Function


# Checking Results with the `assert!` Macro


# Testing Equality with the `assert_eq!` and `assert_ne!` Macros


# Adding Custom Failure Messages


# Checking for Panics with `should_panic`


# Using `Result<T, E>` in Tests
Our tests so far all panic when they fail. We can also write tests that use `Result<T, E>`:

```rust
#[cfg(test)]
mod tests {
    #[test]
    fn it_works() -> Result<(), String> {
        if 2 + 2 == 4 {
            Ok(())
        } else {
            Err(String::from("two plus two does not equal four"))
        }
    }
}
```

The `it_works` function now has the `Result<(), String>` return type. In the body of the function, rather than calling the `assert_eq!` macro, we return `Ok(())` when the test passes and an `Err` with a `String` inside when the test fails.

Writing tests so they return a `Result<T, E>` lets you use the question mark operator in the body of tests, which can be a convenient way to write tests that should fail if any operation within them returns an `Err` variant.

You can’t use the `#[should_panic]` annotation on tests that use `Result<T, E>`. To assert that an operation returns an `Err` variant, _don’t_ use the question mark operator on the `Result<T, E>` value. Instead, use `assert!(value.is_err())`.