When modules get large, you might want to move their definitions to a separate file to make the code easier to navigate.

We can remove the mod name and just put the code in a different file with the same name:

```rust
```

Becomes:

src/main.rs
```rust
```

src/front_of_house.rs
```rust
```

Which can be broken down even more to be:

src/main.rs
```rust
```

src/front_of_house.rs
```rust

src/front_of_house/hosting.rs
```rust
```

# Alternative File Paths
Rust also supports an older style of file path. For a module named `front_of_house` declareed in the crate root, the compiler will look for the module's code in:
1) src/front_of_house.rs
2) src/front_of_house/mod.rs (older style, still supported path)

For a module named `hosting` that is a submodule of `front_of_house`, the compiler will look for the module's code in:
1) src/front_of_house/hosting.rs
2) src/front_of_house/hosting/mod.rs

If you use both styles for the same module, you'll get a compiler error. Using a mix of both styles for different modules in the same project is allowed, but might be confusing for people navigating your project.

The main downside to having many mod.rs files is that it can get confusing if you open them in your editor at the same time.
