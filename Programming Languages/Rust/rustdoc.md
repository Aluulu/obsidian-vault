The standard Rust distribution ships with a tool called rustdoc. Its job is to generate documentation for Rust projects. On a fundamental level, Rustdoc takes as an argument either a crate root or a Markdown file, and produces HTML, CSS, and JavaScript.

## Run Rustdoc

To run Rustdoc on your code, type the following code:

```Shell
rustdoc /path/to/file.rs --crate-name <name_of_docs>
```

Running this will create a new directory named `doc` that holds a website inside of it. The reason we use `--crate-name` is because it will automatically use the name of the file as the name of the website, instead of something helpful.

### Example

```Shell
rustdoc src/main.rs --crate-name doc
```

## Show documentation for functions

In order for the documentation to appear for a function, the function must be public. To do this, simply add `pub` to the start of a function like so:

```Rust
pub fn add_numbers(x: i32, y: i32) -> i32 {
    return x + y
}
```

> [!important] Important
> If you change any documentation, you must rerun the rustdoc command so that it is regenerated.
> [[rustdoc#Run Rustdoc]]

## Inner and outer documentation

### Inner documentation

Inner documentation is a single lined comment that describes what the program does. It uses two forward-slashes followed by a exclamation mark like so:

```Rust
//! This create adds two numbers together

fn main {
    // Code that adds two numbers together
}
```

### Outer documentation

> [!note] Note
> Function docstrings are explained in the function docstring section
> [[Function#Function docstrings]]

Outer documentation documents the next item, describing what the item does, its values and returns etc. This uses three forward-slashes followed by the documentation like so:

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