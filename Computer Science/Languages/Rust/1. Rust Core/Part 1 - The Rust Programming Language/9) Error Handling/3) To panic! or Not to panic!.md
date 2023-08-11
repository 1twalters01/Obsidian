

# Examples, Prototype Code, and Tests


# Cases in Which You Have More Information Than the Compiler


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
	* Panicking when the contract is violated makes sense because a contract violation always indicates a caller-side bug
		* This is not a kind of error you want the calling code to have to explicitly handle.

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
