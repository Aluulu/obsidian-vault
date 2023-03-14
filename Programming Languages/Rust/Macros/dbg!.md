`dbg!` is a print macro that prints information for debugging. Unlike `println!` which just prints exactly what you tell it to, `dbg!` on the otherhand prints additional information useful for debugging.

dbg! is not intended for long-term debugging; instead [[debug!]] should be used in its place.

# Syntax

```Rust
dbg!("Text goes here")
dbg!(<a_variable_or_almost_anything_can_go_here>())
```

# Example

```Rust
let a = 2;
let b = dbg!(a * 2) + 1;
```

This example will print out the following assuming the file it is current in is called `main.rs`:

```Shell
[src/main.rs:2] a * 2 = 4
```

Using `dbg!` helps because it:
 - Prints the file it is in
 - Prints variables rather than the value so you can see exactly what it is interacting with
 - Prints the line number