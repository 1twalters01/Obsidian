# Cargo
* Cargo is Rust's build system and package manager.
* Cargo handles a things such as:
	* Building your code
	* Downloading the libraries your code depends on
	* Building those libraries (dependencies)
* The rest of this guide assumes the use of Cargo.
* Cargo comes installed with the official rust installer. Check if it is installed using:
	* `cargo --version`
* To work on existing projects all you have to do is run:
	* `git clone example.org/someproject`
	* `cd some project`
	* `cargo build`

# Creating a project with Cargo
Run the folllowing on any operating system to create a project with Cargo:
`cargo new file_name`
`cd file_name`

The first command creates a new directory and project called `file_name`.

* Go to the directory and list the files.
* Cargo has generated two files and one directory for us: 
	* A `Cargo.toml` file
	* A `src` directory with a `main.rs` file inside
	* It also initialised a new Git repo along with a .gitignore file.
		* Git files won't be generated if you run `cargo new` within an existing Git repository
		* Override this behaviour by using `cargo new --vcs=git`

Open `Cargo.toml`. You should see the following:
```rust
[package]
name = "hello_cargo"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
```

This file is in the TOML format, which is Cargo's configuration format.

* The first line, [package], is a section heading that indicates that the following statements are configuring a package.
* The next three lines set the configuration information Cargo needs to compile your program
	* The name
	* The version
	* The edition of Rust to use
* The last line, `[dependencies]`, is the start of a section for you to list any of your project's dependencies.
* In Rust, packages of code are referred to as crates.

Go to `src/main.rs`. Cargo has generated a "hello world" program.

Cargo expects your source files to live inside the src directory. The top-level project directory is just for README files, license information, configuration files, and anything else not related to your code.

If you started a project that doesn't use Cargo, you can convert it to a project that does by:
* Moving the project code into a `src` directory
* Creating an appropriate `Cargo.toml` file

# Building and Running a Cargo Project
* Build your project by entering: `cargo build`
* This creates an exe file in target/debug/filename rather than your current directory.
	* Because the default build is a debug build, Cargo puts the binary in a directory named debug
		* Run the executable with the command
			* On windows using: `.\target\debug\hello_cargo.exe`
			* Everything else: `./target/debug/hello_cargo`
	* Running `cargo build` also makes Cargo create a new file at the top level: `Cargo.lock`. This keeps track of the exact versions of dependencies in your project.
		* You won't ever have to change this file manually

* We can also use `cargo run` to compile the code and then run the resultant executable all in one command
* `cargo run` is more convenient than the above.
	* If you don't change the code then it just runs the binary.

* Cargo also provides a command called `cargo check` which quickly checks your code to make sure it compiles but doesn't produce an executable.
* Often, `cargo check` is much faster than `cargo build` as it skips the step of producing an executable.
	* If you are continually checking your work while writing the code, using this will speed up the process

# Building for Release
When your project is finally ready for release, you can use `cargo build --release` to compile it with optimisations.

* This will create an executable in `target/release` instead of `target/debug`.
* The optimisations make your code run faster, but lengthens the time it takes for your program to compile.

