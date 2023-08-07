# Module Cheat Sheet
How modules work is as follows:
* **Start from the crate root:** When compiling a crate, the compiler first looks in the crate root file (usually `src/lib.rs` for a library crate or `src/main.rs` for a binary crate) for code to compile
* **Declaring modules:** In the crate root file, you can declare new modules. For example, you declare a "garden" module with `mod garden;`. The compiler will look for the module's code in these places:
	* Inline, within curly brackets that replace the semicolon following `mod garden`
	* In the file `src/garden.rs`
	* In the file `src/garden/mod.rs`
* **Declaring submodules:** In any file other than the crate root, you can declare submodules. For example, you might declare `mod vegetables;` in `src/garden.rs`. The compiler will look for the submodule's code within the directory named for the parent module in these places:
	* Inline, directly following `mod vegetables`, within curly brackets instead of the semicolon
	* In the file `src/garden/vegetables.rs`
	* In the file `src/garden/vegetables/mod.rs`
* **Paths to code in modules:** Once a module is part of your crate, you can refer to code in that module from anywhere else in that same crate, as long as the privacy rules allow, using the path to the code.  For example, an `Asparagus` type in the garden vegetables module would be found at `crate::garden::vegetables::Asparagus`.
* **Private vs public:** Code within a module is private from its parent modules by default. To make a module public, declare it with `pub mod` instead of `mod`. To make items within a public module public as well, use `pub` before their declarations.
* **The `use` keyword:** Within a scope, the `use` keyword creates shortcuts to items to reduce repetition of long paths. In any scope that can refer to `crate::garden::vegetables::Asparagus`, you can create a shortcut with `use crate::garden::vegetables::Asparagus;` and from then on you only need to write `Asparagus` to make use of that type in the scope.

## Example
Create new crate called `backyard` that contains the following:

backyard
├── Cargo.lock
├── Cargo.toml
└── src
    ├── garden
    │   └── vegetables.rs
    ├── garden.rs
    └── main.rs

```rust
use crate::garden::vegetables::Asparagus;

pub mod garden;

fn main() {
    let plant = Asparagus {};
    println!("I'm growing {:?}!", plant);
}
```

The `pub mod garden;` line tells the compiler to include the code it finds in `rc/gardens.rs`. Put the following inside of `src/garden.rs`:
```rust
pub mod vegetables;
```

Here, `pub mod vegetables;` means the code in `src/garden/vegetables.rs` is included too. That code is:

```rust
#[derive(Debug)]
pub struct Asparagus {}
```

# Grouping Related Code in Modules
Modules let us:
* Organise code within a crate for readability and easy reuse
* Control the privacy of items as code is private by default

As an example, let's write a library crate that provides the functionality of a restaurant. We’ll define the signatures of functions but leave their bodies empty to concentrate on the organization of the code, rather than the implementation of a restaurant.

Create a new library named `restaurant` by running `cargo new restaurant --lib`. Then write the following into `src/lib.rs`:

```rust
mod front_of_house {
    mod hosting {
        fn add_to_waitlist() {}
        
        fn seat_at_table() {}
    }
    
    mod serving {
        fn take_order() {}
        
        fn serve_order() {}
        
        fn take_payment() {}
    }
}
```

* We define a module with the `mod` keyword followed by the name of the module (in this case, `front_of_house`).
* The body of the module then goes inside curly brackets.
* Inside modules, we can place other modules, as in this case with the modules `hosting` and `serving`.
* Modules can also hold definitions for other items, such as structs, enums, constants, traits, and functions.

`src/main.rs` and `src/lib.rs` are called crate roots because the contents of either of these two files form a module named `crate` at the root of the crate’s module structure, known as the _module tree_.

crate
 └── front_of_house
     ├── hosting
     │   ├── add_to_waitlist
     │   └── seat_at_table
     └── serving
         ├── take_order
         ├── serve_order
         └── take_payment

The module tree might remind you of the filesystem’s directory tree on your computer; this is a very apt comparison. Just like directories in a filesystem, you use modules to organize your code. And just like files in a directory, we need a way to find our modules.