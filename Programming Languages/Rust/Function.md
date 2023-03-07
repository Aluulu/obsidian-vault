Functions is a combination of code that is intended to be ran multiple times. In [[Rust]], functions have very specific rules they must follow.

## Syntax

The standard for writing function names are written using snake_case and lowercase. This is denoted in the official rust book:

> Rust code uses _snake case_ as the conventional style for function and variable names, in which all letters are lowercase and underscores separate words

\-https://doc.rust-lang.org/book/ch03-03-how-functions-work.html

```Rust
fn <function_name>(<argument_name>: <function_ data_type>) -> <return_data_type> {
	// Function contents
}
```

### Simple example

```Rust
fn main() {
    println!("Hello, world!");

    another_function();
}

fn another_function() {
    println!("Another function.");
}
```

### Detailed example

```Rust
fn add_two_numbers(x: i32, y: i32) -> i32 {
	let z: i32;
	z = x + y;
	return z;
}
```

## Function returns

To return variables from a function, you can do so using the `return` command like so:

```Rust
fn answer_to_life_the_universe_and_everything() -> i32 {
    return 42;
}
```

The important part is `-> data_type` as that denotes that it is expecting a datatype of `i32` as a return value.

### Data types

Data types that can be returned include but are not limited to:

\- https://doc.rust-lang.org/reference/types.html


| Data Type | Rust code          |
| --------- | ------------------ |
| Integer   | i16 / u16          |
| String    | String / str/ char | 
| Boolean   | bool               |
| Float     | f32                |

### Return multiple values

You can return multiple values by closing the return and expected return data types in parentheses. The syntax is as follows:

```Rust
fn person_data() -> (String, i8, String) {
	let name:String = String::from("Sam");
	let age:i8 = 18;
	let hair_colour:String = String::from("Blonde");
	return (name, age, hair_colour)
}

fn main() {
	let (name, age, hair_colour) = person_data();
	println!("{} is {} years old and has {} hair", name, age, hair_colour);
}
```

## Function arguments

If you want to pass arguments to a function, you can do so using the following syntax:

```Rust
fn add_numbers(x: i32, y: i32) -> i32 {
    return x + y
}
```

## Function docstrings

> [!important] Important
> See [[rustdoc]] to see how to access the documentation

Function docstrings are documentation that explains everything you will need to know about a function. 

Function docstrings use three backspaces `///` to denote documentation. These three backspaces are used in [[rustdoc]], these comments get compiled into the documentation.
It is important to note that it can support Markdown.

> [!important] Important
> You must document before the item as Rust takes the documentation before the item and applies it to the item after it.
> https://doc.rust-lang.org/rustdoc/index.html#outer-and-inner-documentation

Here is an example for how a docstring can be written:

```Rust
/// Checks if the provided string is purely numeric
///
/// Arguments:
///     string_to_check (String): The String that will be checked
///
/// Returns:
///     true (Boolean): Returns true if all characters are numeric
///     false (Boolean): Returns false if any characters are numeric
fn is_string_numeric(string_to_check: String) -> bool {
	for character in string_to_check.chars() {
		if !character.is_numeric() {
			return false;
		}
	}
	return true;
}
```