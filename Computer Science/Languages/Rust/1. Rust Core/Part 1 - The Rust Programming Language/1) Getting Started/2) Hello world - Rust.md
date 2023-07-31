# Creating Files
* Rust files always end with the `.rs` extension.
* Use an underscore to separate multiple words in your file name:
	 Use `hello_world.rs` rather than `helloWorld.rs`

# Example
Put the following into a file called *main.rs*:
```rust
fn main() {
    for i in 0..3 {
        println!("Hello world");
    }
}
```

Then type `rustc main.rs` and then `.\main.exe` into a terminal.
This should print to the terminal.

# Example Analysis
* When your project has a `src/main.rs` file, then it's built into an executable.
* The main function is the first code that runs in every executable Rust program.
* The first line declares a function named main that has no parameters and returns nothing.
	* Parameters go inside the parentheses `()`.
	* The function body is wrapped in `{}`.
		* It is good style to place the opening curly bracket on the same line as the function declaration, adding one space in between
		* If you want to stick to a standard style across Rust projects, you can use an automatic formatter tool called `rustfmt` to format your code in a particular style

* Rust style is to indent with four spaces, not a tab

* `println!` calls a Rust macro. If it called a function instead, it would be entered as `println` (without the `!`).
	* Macros don't always follow the same rules as functions.
	* Rust macros are detailed in [[6) Macros]]
* End lines with a semicolon (`;`)

# Compiling and running
Rust is an ahead of time compiled language, meaning you can compile a program and give the exe to someone else, and they can run it without having rust installed.

If you give someone a dynamic language file like `.rb`, `.py`, etc. file, they need to have the language installed, but they only need one command to compile and run your program. Everything is a trade-off in language design.

* Before running a Rust program, you must compile it using the Rust compiler
	* Enter `rustc main.rs` to compile it
	* On Linux, macOS and PowerShell on Windows, you can see the files in a directory by entering the `ls` command in your shell
	* In the Windows CMD type `dir /B`
* Just compiling with `rustc` is fine for simple programs, but complex ones will use the Cargo tool.
