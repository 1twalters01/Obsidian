Writing out the paths to call functions can feel inconvenient and repetitive. We can implify this by creating a shortcut with the keyword `use`:

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

use crate::front_of_house::hosting;

pub fn eat_at restaurant() {
    hosting::add_to_waitlist();
}
```

Adding `use` and a path in a scope is similar to creating a symbolic link in the file system.
By adding `use crate::front_of_house::hosting` in the crate root, `hosting` is now a valid name in that scope, just as though the hosting module had been defined in the crate root.
Privacy is checked when paths are brought into scope with `use`.

Note that `use` only creates the shortcut for the particular scope where the `use` occurs.
