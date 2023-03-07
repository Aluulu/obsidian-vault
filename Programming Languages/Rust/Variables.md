Variables are named containers that store data values inside of them. In [[Rust]], variables are declared with specific rules that must be followed:

## Syntax

To declare a variable, you must first use the `let` command to indicate a variable is being declared.

```Rust
let <variable_name>: <data_type> = <variable_contents>;
```

### Example

```Rust
let x: i32 = 5;
```

### Declaring empty variables

To declare an empty variable, there are multiple ways to do this


## Scope exits and variables

If you declare a variable inside of scope, aka two parenthesis `{}`, the variable will be cleared once that scope exits. An example of this happening is:

```Rust
fn add_two_numbers() {
	let x: i32 = 5;
	if x == 5 {
		let y: i32 = 5; // y is declared and assigned a value
	} // y is deleted
	let z = x + y;
}
```

To fix this, you must declare the variable in the same scope you wish to use it in so that the variable exists to that item. 

To fix our example above using the logic above, it would look like this:

```Rust
fn add_two_numbers() {
	let x: i32 = 5;
	let y: i32 // y is declared
	if x == 5 {
		y = 5; // y is assigned the value
	} // y is no longer deleted
	let z = x + y;
}
```

The reason this works is because the `y` variable was declared in the same parenthesis as the command it was used in. It was only assigned a value inside a different parenthesis, which doesn't affect if the variable is deleted or not. Once the if-statement's end parenthesis had been reached, nothing was deleted.

### Common mistake

A common mistake is declaring the variable again with the value inside another scope. For example, using the example above:

```Rust
fn add_two_numbers() {
	let x: i32 = 5;
	let y: i32 // y is declared once
	if x == 5 {
		let y = 5; // y is declared again and assigned a value
	} // y is now deleted because it was declared inside the scope
	let z = x + y;
}
```

The issue with this code was using `let` to declare the variable again rather than just assigning the value. Removing let from `let y = 5` fixes this issue.