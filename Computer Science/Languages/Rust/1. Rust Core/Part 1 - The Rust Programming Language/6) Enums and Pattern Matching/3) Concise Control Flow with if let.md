The `if let` syntax lets you combine `if` and `let` into a less verbose way to handle values that match one pattern while ignoring the rest. Consider the following:

```rust
let config_max = Some(3u8);
match config_max {
   Some(max) => println!("The maximum is {}", max),
   _ => (),
}
```

If the value is `Some` we print out the value by binding it to a variable. To satisfy the `match` expression, we have to add `_ => ()` after processing just one variant.

We can skip the boilerplate by using `if let`. The following behaves the same as the `match` statement above:

```rust
let config_max = Some(3u8);
if let Some(max) = config_max {
    println!("The maximum is {}", max)
}
```

if (let Some(var) = Some(3u8) in question) then do the following

Choosing between `match` and `if let` depends on whether gaining conciseness is an appropriate trade-off for losing exhaustive checking. Think of it as syntax sugar.

We can include an else with this. 

Say we wanted to count all non-quarter coins while also announcing the state of the quarters then we could do either one of the following:

```rust
let mut count = 0;
match coin {
    Coin::Quarter(state) => println!("Quarter is from {:?}", state),
    _ => count += 1,
}
```

```rust
let mut count = 0;
if let Coin::Quarter(state) = coin {
    println!("Quarter is from {:?}", state);
} else {
    count += 1;
}
```