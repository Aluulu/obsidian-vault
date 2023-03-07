
# Passing system commands

To pass commands to the system, use the following command:

```Rust
let <variable_name> = Command::new("<command_to_run>")
	.arg("<single_argument_here>")
	.arg("<another_argument_here>")
	.output()
	.expect("<error message goes here");

// Convert a std response from vec<u8> to String
// NOTE: Because we modify the item, we can't use a string slice
let <response_name> = String::from_utf8(output.stdout).unwrap();

<command_line_return_code> = <varaible_name>.status;
<command_line_output> = String::from_utf8_lossy(&<variable_name>.stdout);
<command_line_error_message> = String::from_utf8_lossy(&<variable_name>.stderr);
```

## Example

```Rust
// Check the location of LibreOffice using Flatpak arguments
let find_libreoffice = Command::new("/usr/bin/flatpak")
	.arg("info")
	.arg("--show-location")
	.arg("org.libreoffice.LibreOffice")
	.output()
	.expect("Failed to execute Flatpak command");

let libreoffice_location = String::from_utf8(find_libreoffice.stdout).unwrap();
println!("LibreOffice is installed at {}", libreoffice_location);

println!("The return code is: {}", find_flatpak.status);
println!("The ouput is: {}", String::from_utf8_lossy(&find_flatpak.stdout));
println!("The err mesg is: {}", String::from_utf8_lossy(&find_flatpak.stderr));
```