When you choose to return a `Result` value, you give the calling code options.
However, in situations such as examples, prototype code, and tests, it’s more appropriate to write code that panics instead of returning a `Result`.

# Examples, Prototype Code, and Tests
When you’re writing an example to illustrate some concept, also including robust error-handling code can make the example less clear. In examples, it’s understood that a call to a method like `unwrap` that could panic is meant as a placeholder for the way you’d want your application to handle errors.

If a method call fails in a test, you’d want the whole test to fail, even if that method isn’t the functionality under test. Because `panic!` is how a test is marked as a failure, calling `unwrap` or `expect` is exactly what should happen.

# Cases in Which You Have More Information Than the Compiler
You should call `unwrap` or `expect` when you have some other logic that ensures the `Result` will have an `Ok` value, but the logic isn’t something the compiler understands.

You’ll still have a `Result` value that you need to handle: whatever operation you’re calling still has the possibility of failing in general, even though it’s logically impossible in your particular situation.

If you can ensure by manually inspecting the code that you’ll never have an `Err` variant, it’s perfectly acceptable to call `unwrap`, and even better to document the reason you think you’ll never have an `Err` variant in the `expect` text. Here’s an example:
```rust
fn main() {
    use std::net::IpAddr;

    let home: IpAddr = "127.0.0.1"
        .parse()
        .expect("Hardcoded IP address should be valid");
}
```

We can see that `127.0.0.1` is a valid IP address, so it’s acceptable to use `expect` here. However, having a hardcoded, valid string doesn’t change the return type of the `parse` method: we still get a `Result` value, and the compiler will still make us handle the `Result` as if the `Err` variant is a possibility because the compiler isn’t smart enough to see that this string is always a valid IP address.

Mentioning the assumption that this IP address is hardcoded will prompt us to change `expect` to better error handling code if in the future, we need to get the IP address from some other source instead.

# Guidelines for Error Handling
It’s advisable to have your code panic when it’s possible that your code could end up in a bad state.
In this context, a _bad state_ is when:
* some assumption, guarantee, contract, or invariant has been broken
	* Examples include when invalid values, contradictory values, or missing values are passed to your code
* plus one or more of the following:
	* The bad state is something that is unexpected, as opposed to something that will likely happen occasionally, like a user entering data in the wrong format.
	* Your code after this point needs to rely on not being in this bad state, rather than checking for the problem at every step.
	* There’s not a good way to encode this information in the types you use. See chapter 17 for more information.

In cases where continuing could be insecure or harmful, the best choice might be to call `panic!` and alert the person using your library to the bug in their code so they can fix it during development.
* Similarly, `panic!` is often appropriate if you’re calling external code that is out of your control and it returns an invalid state that you have no way of fixing.
* When your code performs an operation that could put a user at risk if it’s called using invalid values, your code should verify the values are valid first and panic if the values aren’t valid.
	* This is the main reason the standard library will call `panic!` if you attempt an out-of-bounds memory access: trying to access memory that doesn’t belong to the current data structure is a common security problem.
	* Functions often have _contracts_: their behaviour is only guaranteed if the inputs meet particular requirements.
		* Panicking when the contract is violated makes sense because a contract violation always indicates a caller-side bug. This is not a kind of error you want the calling code to have to explicitly handle.

Return a result if:
* Someone calls your code and passes in values that don’t make sense, it’s best to return an error if you can so the user of the library can decide what they want to do in that case.
* Failure is expected, it’s more appropriate to return a `Result` than to make a `panic!` call. Examples include:
	* A parser being given malformed data 
	* An HTTP request returning a status that indicates you have hit a rate limit.

Rust's type system protects you from a lot of things, for example:
* Using an unsigned integer type such as `u32`, which ensures the parameter is never negative.
* If you have a type rather than an `Option`, your program expects to have _something_ and not _nothing_. Your code doesn’t have to handle two cases for the `Some` and `None` variants.
	*  Code trying to pass nothing to your function won’t even compile, so your function doesn’t have to check for that case at runtime.

# Creating Custom Types for Validation
Let’s take the idea of using Rust’s type system to ensure we have a valid value one step further and look at creating a custom type for validation.

Let's say we needed a value to be between 1 and 100. Instead of checking every in every function that needed this condition, we can make a new type and put the validations in a function to create an instance of the type rather than repeating the validations everywhere:
```rust
pub struct Guess {
    value: i32,
}

impl Guess {
    pub fn new(value: i32) -> Guess {
        if value < 1 || value > 100 {
            panic!("Guess value must be between 1 and 100, got {}.", value);
        }

        Guess { value }
    }

    pub fn value(&self) -> i32 {
        self.value
    }
}
```

A function that has a parameter or returns only numbers between 1 and 100 could then declare in its signature that it takes or returns a `Guess` rather than an `i32` and wouldn’t need to do any additional checks in its body.