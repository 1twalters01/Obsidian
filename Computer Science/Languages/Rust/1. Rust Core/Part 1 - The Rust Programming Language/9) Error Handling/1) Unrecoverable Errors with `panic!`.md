Rust groups errors into two major categories: _recoverable_ and _unrecoverable_ errors.
* For a recoverable error, such as a _file not found_ error, we most likely just want to report the problem to the user and retry the operation.
* Unrecoverable errors are always symptoms of bugs, like trying to access a location beyond the end of an array, and so we want to immediately stop the program.

Most languages don’t distinguish between these two kinds of errors and handle both in the same way, using mechanisms such as exceptions.

Rust doesn’t have exceptions. Instead, it has the type `Result<T, E>` for recoverable errors and the `panic!` macro that stops execution when the program encounters an unrecoverable error.

# Unwinding the Stack or Aborting in Response to a Panic
By default, when a panic occurs, the program starts _unwinding_, which means Rust walks back up the stack and cleans up the data from each function it encounters.

This walking back and cleanup is a lot of work. Rust, therefore, allows you to choose the alternative of immediately _aborting_, which ends the program without cleaning up. Memory that the program was using will then need to be cleaned up by the operating system.

If in your project you need to make the resulting binary as small as possible, you can switch from unwinding to aborting upon a panic by adding `panic = 'abort'` to the appropriate `[profile]` sections in your _Cargo.toml_ file.

For example, if you want to abort on panic in release mode, add this:

```toml
[profile.release]
panic = 'abort'
```

# `panic!`
The `panic!` macro is for when bad things happen in your code and you can't do anything about it. There are two ways to cause a panic:
* By taking an action that causes our code to panic (such as accessing an array past the end)
* By explicitly calling the `panic!` macro.

In both cases, we cause a panic in our program. By default, these panics will print a failure message, unwind, clean up the stack, and quit.

Via an environment variable, you can also have Rust display the call stack when a panic occurs to make it easier to track down the source of the panic.

Call panic as follows:
```rust
fn main() {
    panic!("crash and burn");
}
```

When you run the program, you’ll see something like this:
```terminal
$ cargo run
   Compiling panic v0.1.0 (file:///projects/panic)
    Finished dev [unoptimized + debuginfo] target(s) in 0.25s
     Running `target/debug/panic`
thread 'main' panicked at 'crash and burn', src/main.rs:2:5
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

* The first error line shows our panic message 
* The second error line shows the place in our source code where the panic occurred: `src/main.rs:2:5`
	* This is the second line, fifth character 

Sometimes it is called by a function that has a panic macro nested inside of it. Here, we can use the backtrace of the functions the `panic!` call came from to figure out the part of our code that is causing the problem.

# Using a `panic!` Backtrace
Let’s look at another example to see what it’s like when a `panic!` call comes from a library because of a bug in our code instead of from our code calling the macro directly:

```rust
fn main() {
    let v = vec![1, 2, 3];

    v[99];
}
```

Using `[]` is supposed to return an element, but if you pass an invalid index, there’s no element that Rust could return here that would be correct.

In C, attempting to read beyond the end of a data structure is undefined behaviour
* You might get whatever is at the location in memory that would correspond to that element in the data structure, even though the memory doesn’t belong to that structure.
* This is called a _buffer overread_
* This can lead to security vulnerabilities if an attacker is able to manipulate the index in such a way as to read data they shouldn’t be allowed to that is stored after the data structure.

To protect your program from this sort of vulnerability, if you try to read an element at an index that doesn’t exist, Rust will stop execution and refuse to continue.

```terminal
$ cargo run
   Compiling panic v0.1.0 (file:///projects/panic)
    Finished dev [unoptimized + debuginfo] target(s) in 0.27s
     Running `target/debug/panic`
thread 'main' panicked at 'index out of bounds: the len is 3 but the index is 99', src/main.rs:4:5
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

This error points at line 4 of our `main.rs` where we attempt to access index 99.
The next note line tells us that we can set the `RUST_BACKTRACE` environment variable to get a backtrace of exactly what happened to cause the error.
A _backtrace_ is a list of all the functions that have been called to get to this point.

Enter `RUST_BACKTRACE=1 cargo run`

```terminal
$ RUST_BACKTRACE=1 cargo run
thread 'main' panicked at 'index out of bounds: the len is 3 but the index is 99', src/main.rs:4:5
stack backtrace:
   0: rust_begin_unwind
             at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library/std/src/panicking.rs:584:5
   1: core::panicking::panic_fmt
             at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library/core/src/panicking.rs:142:14
   2: core::panicking::panic_bounds_check
             at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library/core/src/panicking.rs:84:5
   3: <usize as core::slice::index::SliceIndex<[T]>>::index
             at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library/core/src/slice/index.rs:242:10
   4: core::slice::index::<impl core::ops::index::Index<I> for [T]>::index
             at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library/core/src/slice/index.rs:18:9
   5: <alloc::vec::Vec<T,A> as core::ops::index::Index<I>>::index
             at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library/alloc/src/vec/mod.rs:2591:9
   6: panic::main
             at ./src/main.rs:4:5
   7: core::ops::function::FnOnce::call_once
             at /rustc/e092d0b6b43f2de967af0887873151bb1c0b18d3/library/core/src/ops/function.rs:248:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.

```

Use this information to work out what is making your code panic.

The exact output you see might be different depending on your operating system and Rust version. In order to get backtraces with this information, debug symbols must be enabled. Debug symbols are enabled by default when using `cargo build` or `cargo run` without the `--release` flag, as we have here.