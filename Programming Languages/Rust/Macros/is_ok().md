The command in [[Rust]] does stuff


# Returns

is_okay returns two values, these are:
- Ok - This is `true`
- Err - This is `false`

This is important because some commands can interpret those returns and use them. For example this if-statement will do whatever is in the if-statement if the function returns Ok:

```Rust
// Take a string input and check if it is purely numeric
fn is_string_numeric(s: &str) -> bool {
	s.trim().parse::<f64>().is_ok()
}

fn main{
	if is_string_numeric(&second_number) {
		// If "is_okay()" returns ok, do stuff here
	}
}
```