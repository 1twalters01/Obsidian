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

The following works:

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist((){}
    }
}

use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
}
```

But this does not compile:

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist((){}
    }
}

use crate::front_of_house::hosting;

mod customer {
    pub fn eat_at_restaurant() {
        hosting::add_to_waitlist();
    }
}
```

To make it compile then either move the `use` within the customer module too, or reference the shortcut in the parent module with `super::hosting` within the child `customer` module.

# Creating Idiomatic Paths


# Providing New Names with the as Keyword
We can bring two types of the same name into the same scope with `use ... as`:

```rust
use std::fmt::Result;
use std::io::Result as IOResult

fn function1() -> Result {
    --snip--
}

fn function2() -> IOResult<()> {
    --snip--
}
```

# Re-exporting Names with `pub use`
When we bring a name into scope with the `use` keyword, the name available in the new scope is private. To enable the code that calls our code to refer to that name as if it had been in that code's scope, we can combine `pub` and `use`. See below:

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

pub use crate::front_of_house::hosting;

pub fn eat_at_restaurant() {
    hosting::add_to_waitlist();
}
```

Re-exporting is useful when the internal structure of your code is different from how programmers calling your code would think about the domain.

For example, in this restaurant metaphor, the people running the restaurant think about “front of house” and “back of house.” But customers visiting a restaurant probably won’t think about the parts of the restaurant in those terms. With pub use, we can write our code with one structure but expose a different structure.

Doing so makes our library well organized for programmers working on the library and programmers calling the library.

# Using External Packages
Use the same syntax to bring in external packages. In the case of Rng:

```rust
use rand::Rng;

fn main() {
    let secret_number = rand::thread_rng().gen_range(1..=100);
}
```

# Using Nested Paths to Clean Up Large use Lists
If we're using multiple items defined in the same crate or same module, listing each item on its own line can take up a lot of vertical space:

```rust
--snip--
use std::cmp::Ordering;
use std::io;
--snip--
```

Instead, we can do the following:

```rust
use std:{cmp::Ordering, io};
```

If we need to bring in the thing and something else defined in it then we can do the same thing but include self:

```rust
use std::io;
use std::io::Write;
```

is identical to:

```rust
use std::io::{self, Write};
```

This line brings `std::io` and `std::io::Write` into scope.

# The Glob Operator
If we want to bring **all** public items defined in a path into scope, we can specify that path followed by the glob (`*`) operator:

```rust
use std::collections::*;
```

It is often used when testing to bring everything under test into the `tests` module.
