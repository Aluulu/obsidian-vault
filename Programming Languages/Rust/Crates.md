Crates are a [[Rust]] functionality that act as a module to provide libraries and or executables. Typically crates are libraries that allow your program additional functionality, in the same way as Python Libraries.

## Installation

Crates are installed automatically with the installation of Rust. To install Rust, follow the installation instructions found on the Rust page:
[[Rust#Installation]]

## Install a crate

To install a crate, simply type the following command:
```Shell
cargo add <crate_name>
```

This will download the crate to your project's directory, and add the crate to your project's `Cargo.toml` file

## Using a crate

To use a crate in your project, simple add the line to your Rust code
```Rust
use <crate_name>
```

## Example

To download and use a crate like [syn](https://crates.io/crates/syn), do the following:

In your project's directory run this command:
```Shell
cargo add syn
```

Then, in your project's file you wish to use the crate in, type the following (typically at the top of the file):
```Rust
use syn;
```