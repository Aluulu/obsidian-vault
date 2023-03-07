Cargo is [[Rust]]’s build system and package manager. Programmers use this tool to manage their Rust projects because Cargo handles a lot of tasks for you, such as building your code, downloading the [[Crates]] your code depends on, and building those libraries.

## Checking cargo version

You can check the currently installed version of cargo using the following command:

```Shell
cargo --version
```

## Cargo.toml

Cargo.toml is a text file that contains your project's configuration file. Here you can change the project's general settings and dependencies. 

Find out how to add dependencies in [[Rust#Add dependencies]].

## Build and execute Rust programs

> [!important] Important
> Detailed instructions on how to build and execute your program is found here:
> [[Rust#Build and execute your project (debug)]]
> [[Rust#Build your project (release)]]

To build and execute your program, run these commands:

```Shell
cargo build
```

```Shell
cargo run
```
