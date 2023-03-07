Rust is a [[Programming Languages]] developed by Mozilla that is fast, efficent, and safe.

## Installation

Run this command to download and install rust:

```Shell
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

## Create a project

You can create a project using [[Cargo]], Rust's build system and package manager. To create one in the current directory, run the following command:

```Shell
cargo new <project_name>
```


## Build and execute your project (debug)

> [!tip] Tip
> Building your project for debugging is faster as debugging doesn't compile for optimisations

To build your project to debug, run the following command in the top directory of your project:

```Shell
cargo build
```

Your executable will be placed in `target/debug/<project_name>` rather than in the current directory because it is a not a release build.

To execute it, type the following command:

```Shell
cargo run
```

## Build your project (release)

> [!tip] Tip
> Building your project for release compiles the program with as many optimisation as possible. Building for release takes longer, this is why there are two versions, one for development and one for release builds.

To build your project for release, run the following command:

```Shell
cargo build --release
```

The executable will be placed in `target/release`

To execute the release build of the program with compiler optimisations, type the following command:

```Shell
cargo run --release
```

## Add dependencies

To add dependencies, edit cargo.toml and add the name and version you would like to use. An example of this is as follows:

```toml
[dependencies]
time = "0.1.12"
regex = "0.1.41"
```

## Directories
-   bin - The bin directory contains executables of crates that were installed via `cargo install` or `rustup`. To be able to make these binaries accessible, add the path of the directory to your `$PATH` environment variable.
    
-   [[Git]] - Git sources are stored here:
    
    -   git/db - When a crate depends on a git repository, Cargo clones the repo as a bare repo into this directory and updates it if necessary.
        
    -   git/checkouts - If a git source is used, the required commit of the repo is checked out from the bare repo inside `git/db` into this directory. This provides the compiler with the actual files contained in the repo of the commit specified for that dependency. Multiple checkouts of different commits of the same repo are possible.
        
-   registry - Packages and metadata of crate registries (such as [crates.io](https://crates.io/)) are located here.
    
    -   registry/index - The index is a bare git repository which contains the metadata (versions, dependencies etc) of all available crates of a registry.
        
    -   registry/cache - Downloaded dependencies are stored in the cache. The crates are compressed gzip archives named with a `.crate` extension.
        
    -   registry/src - If a downloaded `.crate` archive is required by a package, it is unpacked into `registry/src` folder where rustc will find the `.rs` files.