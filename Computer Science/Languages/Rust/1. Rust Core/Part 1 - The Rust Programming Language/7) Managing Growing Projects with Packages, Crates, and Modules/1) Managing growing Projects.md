Organizing code becomes increasingly important as programs grow. You clarify where to find code that implements a particular feature and where to go to change how a feature works when you group code.

A package can contain multiple binary creates and optionally one library crate. As a package grows, you can extract parts into separate crates that become external dependencies. For very large projects comprising a set of interrelated packages that grow together, Cargo provides workspaces, which are in chapter 14.

Rust has a number of features that let you manage your code's organisation called the module system, which includes:
* Packages: A Cargo feature that lets you build, test, and share crates
* Crates: A tree of modules that produces a library or executable
* Modules and use: Let you control the organization, scope, and privacy of paths
* Paths: A way of naming an item, such as a struct, function, or module

# Packages and Crates
The first parts of the module system are packages and crates.

A crate is the smallest amount of code that the Rust compiler considers at a time. Whether you use `rustc` or `cargo` the compiler considers the file to be a crate. Crates can contain modules, and the modules may be defined in other files that get compiled with the crate.

A crate can come in one of two forms: a binary crate or a library crate.
* Binary crates are programs you can compile to an executable that you can run
	* examples include command-line programs or servers
* Library crates don't have a main function, and don't compile to an executable. Instead, they define functionality intended to be shared with multiple projects.
	* An example includes the rand library
	* When people say crate they usually mean library crate

The crate root is a source file that the Rust compiler starts from and makes up the root module of your crate.

A package is a bundle of one or more crates that provides a set of functionality. A package contains a `Cargo.toml` file that describes how to build those crates. Cargo is a package that contains:
* the binary crate for the command-line tool used to build code
* a library crate that the binary crate depends on
Other projects can depend on the Cargo library crate to use the same logic the Cargo command-line tool uses.

A package can contain as many binary crates as you like, but at most only one library crate. A package must contain at least one crate, whether that’s a library or binary crate.

Running `cargo new` gives us:
* A `Cargo.toml` file
* A `src` directory that contains `main.rs`

Cargo follows a convention that:
* `src/main.rs` is the crate root of a binary crate with the same name as the package.
* `src/lib.rs` is a library crate with the same name as the package, and src/lib.rs is its crate root.
* Cargo passes the crate root files to `rustc` to build the library or binary.
* If a package contains `src/main.rs` and `src/lib.rs`, it has two crates: a binary and a library, both with the same name as the package.
* A package can have multiple binary crates by placing files in the `src/bin` directory: each file will be a separate binary crate.