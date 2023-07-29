In Rust, the idiomatic comment style starts a comment with two slashes, and the comment continues until the end of the line.
For comments that extend beyond a single line, you'll need to include // on each line like this:

```rust
// Some extremely long comment that understandable needs to
// be on multiple lines, or else...
```

Comments can also be placed at the end of lines containing code:

```rust
fn main() {
	// Some comment up on the top is most common
	let lucky_number = 7; // though you can do this too
}
```

Rust also has another kind of comment, documentation comments, which will be discussed in [[Publishing a Crate to Crates.io]]