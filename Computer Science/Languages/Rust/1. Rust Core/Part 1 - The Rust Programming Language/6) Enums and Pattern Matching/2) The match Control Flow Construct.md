`match` allows you to compare a value against a series of patterns and then execute code based on which pattern matches. Patterns can be made up of:
* literal values
* variable names
* wildcards
* many other things
Chapter 18 covers all the different kinds of patterns and what they do.

The power of `match` comes from the expressiveness of the patterns and the fact that the compiler confirms that all possible cases are handled.

Using American coins as an example for pattern matching:

```rust
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```

This is similar to `if`, however each condition must evaluate to a Boolean value with if, but here it can be any type. Here the type of `coin` in this example is the `Coin` enum.

An arm has two parts: a pattern and some code. The first arm here has a pattern that is the value `Coin::Penny` and then the => operator that separates the pattern and the code to run. Each arm is separated from the next with a comma.

When the `match` expression executes, it compares the resultant value against the pattern of each arm in order. If a pattern matches the value, the code associated with that pattern is executed. If that pattern doesn't match the value, execution continues to the next arm.

We don't typically use curly brackets if the match arm code is short. If you want to run multiple lines of code in a match arm, you must use curly brackets, and the comma following the arm is then optional. For example:

```rust
fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => {
            println!("Lucky penny!");
            1
        },
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```

# Patterns that Bind to Values
Match arms can bind to the parts of the values that match the patterns. This is how we can extract values out of enum variants.

As an example let's change one of the enum variants to hold data inside it. Let's say that the quarter had different designs based on the US state it was made in

```rust
#[derive(Debug)] // so we can inspect the state
enum UsState {
    Alabama,
    Alaska,
    // --snip--
}

enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter(UsState),
}
```

In the match expression for the following, we add a variable called state to the pattern that matches the value of the variant `Coin::Quarter`. When a `Coin::Quarter` matches, the `state` variable will bind to the value of that quarter's state. We can then use `state` in the code arm:

```rust
fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => {
            println!("Lucky penny!");
            1
        },
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter(state) => {
            println!("The quarter is from {:?}!", state);
            25
        },
    }
}
```

If we were to call `value_in_cents(Coin::Quarter(UsState::Alaska))`, `coin` would be `Coin::Quarter(UsState::Alaska)`. It will compare each match arm and not match them until reaching `Coin::Quarter(state)`. Then, `state` will equal `UsState::Alaska`. Remember the print statement conventions (`{:?}` and `{:#?}`) from [[2) An Example Program Using Structs]].

# Matching Option `<T>`
Say we want to add `1` to an `Option<i32>` value that equalled `Some<5>`. The compiler stops us from just doing it as the `None` case is not accounted for. We can do it with enums:

```rust
fn plus_one(x: Option<32>) -> Option<32> {
    match x {
        None => None,
        Some(i) => Some(i + 1),
    }
}

let five = Some(5)
let six = plus_one(five);
let none = plus_one(None)
```

Combining `match` and enums is useful in many situations. This is in a lot of rust code:
1) `match` against an enum
2) bind a variable to the data inside
3) execute code based on it

# Matches Are Exhaustive
The arms' patterns must cover all possibilities. The following will not compile:

```rust
fn plus_one(x: Option<32>) -> Option<32> {
    match x {
        Some(i) => Some(i + 1),
    }
}
```

Rust knows that we didn't cover every possible case, and even knows which pattern we forgot.

# Catch-all Patterns and the `_` Placeholder
Using enums, we can also take special actions for a few particular values, but for all other values take one default action.

Imagine a game where if you role a 3 you add equipment, a 7 you remove it and on any other role you move that number of spaces. You would code it as follows:

```rust
let dice_role = 9;

match dice_roll {
    3 => add_item(),
    7 => remove_item(),
    other => move_player(other)
}

fn add_item() {}
fn remove_item() {}
fn move_player(num_spaces: u8) {}
```

This compiles, even though we haven't listed all the possible values a u8 can have, because the last pattern will match all values not specifically listed.

Note that we put this arm last because patterns are evaluated in order. If we put it first then the other arms would never run, so Rust will warn us if we add arms after a catch all.

Rust also has a pattern we can use when we want a catch-all but don't want to use the value in the catch-all pattern: `_`. This tells Rust we aren't going to use the value, so Rust won't warn us about an unused variable.

Changing the rules of the game so if you don't get a 3 or 7 then you must role again:

```rust
let dice_role = 9;

match dice_roll {
    3 => add_item(),
    7 => remove_item(),
    _ => reroll()
}

fn add_item() {}
fn remove_item() {}
fn reroll() {}
```

If we don't want anything at all to happen unless you roll a 3 or a 7 we can express that by using the unit value:

```rust
let dice_role = 9;

match dice_roll {
    3 => add_item(),
    7 => remove_item(),
    _ => ()
}

fn add_item() {}
fn remove_item() {}
```

Here we are telling Rust explicitly that we aren't going to use any other value that doesn't match a pattern in an earlier arm, and we don't want to run any code in this case.