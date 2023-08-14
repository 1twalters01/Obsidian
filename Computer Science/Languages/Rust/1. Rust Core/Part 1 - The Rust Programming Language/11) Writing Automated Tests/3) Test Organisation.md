The Rust community thinks about tests in terms of two main categories: unit tests and integration tests. _Unit tests_ are small and more focused, testing one module in isolation at a time, and can test private interfaces. _Integration tests_ are entirely external to your library and use your code in the same way any other external code would, using only the public interface and potentially exercising multiple modules per test.

Writing both kinds of tests is important to ensure that the pieces of your library are doing what you expect them to, separately and together.

# Unit Tests
You’ll put unit tests in the `src` directory in each file with the code that they’re testing. The convention is to create a module named `tests` in each file to contain the test functions and to annotate the module with `cfg(test)`.

## The Tests Module and `#[cfg(test)]`
The `#[cfg(test)]` annotation on the tests module tells Rust to compile and run the test code only when you run `cargo test`, not when you run `cargo build`.

This saves compile time when you only want to build the library and saves space in the resulting compiled artefact because the tests are not included.

Recall that when we generated the new `adder` project in the first section of this chapter, Cargo generated this code for us:
```rust
#[cfg(test)]
mod tests {
    #[test]
    fn it_works() {
        let result = 2 + 2;
        assert_eq!(result, 4);
    }
}
```

The attribute `cfg` stands for _configuration_ and tells Rust that the following item should only be included given a certain configuration option. In this case, the configuration option is `test`, which is provided by Rust for compiling and running tests.

By using the `cfg` attribute, Cargo compiles our test code only if we actively run the tests with `cargo test`. This includes any helper functions that might be within this module, in addition to the functions annotated with `#[test]`.

## Testing Private Functions

# Integration Tests
In Rust, integration tests are entirely external to your library. They use your library in the same way any other code would, which means they can only call functions that are part of your library’s public API. Their purpose is to test whether many parts of your library work together correctly.

To create integration tests, you first need a _tests_ directory.

## The _tests_ Directory


## Submodules in Integration Tests


## Integration Tests for Binary Crates
