# cargo

> Manage Rust projects and their module dependencies (crates).
> Some subcommands such as `cargo build` have their own usage documentation.
> More information: <https://doc.rust-lang.org/cargo>.

- Search for crates:

`cargo search {{search_string}}`

- Install a crate:

`cargo install {{crate_name}}`

- List installed crates:

`cargo install --list`

- Create a new binary or library Rust project in the current directory:

`cargo init --{{bin|lib}}`

- Create a new binary or library Rust project in the specified directory:

`cargo new {{path/to/directory}} --{{bin|lib}}`

- Build the Rust project in the current directory:

`cargo build`

- Build the rust project in the current directory using the nightly compiler:

`cargo +nightly build`

- Build using a specific number of threads (default is the number of CPU cores):

`cargo build --jobs {{number_of_threads}}`



# Oprations ............................................................................................
- dev-dependencies documentation cannot be generated. Workaround: add it as regular dependency
- examples of deps can be run directly, crates seem to be self sufficient
- binary files for the cargo-edit commands (such as cargo-add, cargo-rm, and cargo-upgrade) are installed in the bin directory under the default Cargo home directory.

The default Cargo home directory depends on your operating system:
- On Linux and macOS: $HOME/.cargo
- On Windows: %USERPROFILE%\.cargo

```bash
cargo add serde --features derive  # list features via crate's Cargo.toml

cargo tree
cargo tree --prefix none

# find dependencies that can optionally use a crate as well, e.g. Tokio has a feature flag to enable parking_lot, but I want to find all my dependencies that have similar feature flags.
cargo metadata --format-version 1 | jq -c '.packages[] | select(
    .dependencies | any(
        (.name == "parking_lot")
        and
        (.optional == true)
    )
) | .name'

# find lib targets
cargo metadata --format-version 1 | jq '.packages[].targets[] | select(.kind[] == "lib") | .name'
# find example targets
cargo metadata --format-version 1 | jq '.packages[].targets[] | select(.kind[] == "example") | .name'

# list example targets
cargo run --example

# Release
$ cargo install cargo-release
```

## Installation/Upgrade
- Gotcha: after asdf upgrade:
```bash
cargo install cargo-release
cargo install --locked cargo-outdated
cargo install cargo-edit
```
## cargo login, Authentication for Crates.io
- PAT generated via Github OAuth



# Dependency management ...........................................................................
- version differences between Cargo.toml and Cargo.lock arise from the flexibility allowed by version constraints in Cargo.toml.
- Cargo will choose the latest compatible version for your dependencies according to the constraints, and then record the exact version in Cargo.lock.
- This allows Cargo to automatically pick up non-breaking updates for your dependencies while maintaining reproducible builds.
```bash
# show  dependencies [kbknapp/cargo-outdated](https://github.com/kbknapp/cargo-outdated/tree/6007f069790a150eff585ee33eafd8c390779bcb/)
cargo install --locked cargo-outdated
cargo outdated

# upgrade Cargo.toml
cargo install cargo-edit
cargo upgrade  # update Cargo.toml

# upgrade Cargo.lock
cargo update
```

# Structure ........................................................................................
- [Package Layout - The Cargo Book](https://doc.rust-lang.org/cargo/guide/project-layout.html)
- projects can be composed of multiple files (which are modules), that can be nested within folders (which are also modules).
- In order to access the root of that module tree, you can always use the crate:: prefix.

   project_name/
   ├── Cargo.toml
   ├── src/
   │   ├── lib.rs
   │   ├── main.rs
   │   └── bin/
   │       ├── bin1.rs
   │       ├── bin2.rs
   │       └── ...
   ├── tests/
   │   ├── test1.rs
   │   ├── test2.rs
   │   └── ...
   └── examples/
      ├── example1.rs
      ├── example2.rs
      └── ...

  project_name/
      src/
          main.rs
          lib.rs
          models/
              mod.rs
              user.rs
              post.rs
          util/
              mod.rs
              helper.rs
          tests/
              user_test.rs
              post_test.rs

- workspaces, containing packages, containing crates, containing multiple source files belonging to different modules.
- many people use the term "crate" when they actually mean packages. This is because each package contains only one library crate that is assoziated with the package.
- Typically each codebase has only a single workspace, which for small projects often only contains a single package.
- Each package has its own `Cargo.toml` file.
- Each package may contain max ONE library crates and multiple binary test and benchmark crates.
- Crates have a root module code file and code files included from this.

## main.rs, Binaries
- [Cargo Targets - The Cargo Book](https://doc.rust-lang.org/cargo/reference/cargo-targets.html#binaries)
- `main.rs`: entry point for an executable crate.
- If the crate has multiple executables, each executable will have its own `*.rs` file in the `src/bin` directory.
- to make a crate a lib you just rename the file main.rs to lib.rs, now is a library

There are two ways to create executable targets that don't have main.rs at their root.
### Method 1: Declare a binary target in cargo.toml
- You'll then be able to run your example target via `$ cargo run --bin example`
- Any number of binary targets can be declared in this way.
- Add an entry to you cargo.toml that resembles the following:
```
[[bin]]
name = "example"
path = "src/example.rs"
```
### Method 2: The bin directory
- All Rust files in the src/bin directory of your project act as binary targets.  No cargo.toml configuration required.
- If you move src/example.rs to `src/bin/example.rs`, you'll be able to immediately run your example binary with `$ cargo run --bin example`
- Do note that once you've declare more than one binary target, you will need to use the --bin flag whenever you invoke cargo run. For more information, see the Cargo Targets docs for binary targets.


## lib.rs
- is the crate's root module and serves as the public interface for the crate.
- should contain the mod declarations for the crate's other modules, as well as any other code that needs to be visible to other crates. (`__init__.py`)
- is optional. If your crate doesn't have any public APIs that need to be visible to other crates, you can omit the lib.rs file and put all of your crate's code in other files.
```rust
// For example, if your crate has a module called my_module, you would include a mod declaration in lib.rs like this:
mod my_module;
// You can then use the use keyword in other parts of your crate to bring the contents of my_module into scope.

// If your crate has multiple levels of modules
mod utils;
mod server {
    mod router;
    mod handler;
}
// This crate has a top-level utils module and a server module, which has two submodules called router and handler.
```
### Using external Crates
The `extern crate` construct was needed in Rust 2015.

---
<!--ID:1688811538565-->
1. How to use external Creates
> ```rust
> // You can now directly write:
> use crate::SomeType;
> ```

---
```rust
extern crate skim;
use skim::SomeType;

// You can now directly write:
use skim::SomeType;

// The extern crate construct is still needed in a few situations in Rust 2018:
// You want to rename a crate for use in your program.
extern crate rand as my_rand;

// You're creating a binary and need to use a crate only for its side effects (for example, it only provides macros or it links with a library).
extern crate my_macro_crate;
```


## build.rs, Build Script
When Cargo detects a build.rs file in the root of a package, it treats it as a build script and executes it before building the main package.
Build scripts are used to perform various tasks during the build process, such as code generation, configuration, compilation of C/C++ code, linking to system libraries, and more.

modifies the build process by emitting special instructions to the console.
instructions start with the `cargo:` prefix and have a predefined format.

Here are some common use cases for a build.rs file:

### Linking to C/C++ libraries:
If your Rust project depends on C/C++ libraries, you can use a build script to specify the search paths and link the libraries. For example:
```rust
fn main() {
    println!("cargo:rustc-link-search=native=/path/to/libs");
    println!("cargo:rustc-link-lib=static=my_library");
}
// CBC config
fn main() {
    println!("cargo:rustc-link-search=/Users/Q187392/dev/bin/coinbrew/dist/lib");
}
```
### Compiling C/C++ code:
You can use a build script to compile C/C++ code and link it to your Rust project. This can be done using the cc crate, which provides a convenient API for compiling C/C++ code. For example:
```rust
use cc::Build;

fn main() {
    Build::new()
        .file("src/my_library.c")
        .compile("my_library");
}
```

## Cargo.toml
```toml
[package]
name = "my-project"
version = "0.1.0"
authors = ["Your Name <your@email.com>"]
description = "A brief description of my project"
license = "MIT"
repository = "https://github.com/your-username/my-project"
readme = "README.md"
keywords = ["keyword1", "keyword2"]
homepage = "https://your-project-homepage.com"
documentation = "https://your-project-documentation.com"

[lib]
name = "my_project"
crate-type = ["cdylib"]
path = "src/lib.rs"

[dependencies]
# Add dependencies for your project here

[dev-dependencies]
# Add development dependencies here

[build-dependencies]
# Add build dependencies here

[features]
# Enable or disable features for your project here

[workspace]
members = ["sub-project-1", "sub-project-2"]

[package.metadata.docs.rs]
all-features = true

[badges]
travis-ci = { repository = "your-username/my-project" }
```

## Workspace, Package Members
A Rust project can have more than one Cargo.toml file if it's a workspace.
A workspace is a set of packages that share the same Cargo.lock and output directory.
This allows you to share common dependencies between packages and manage them in one place, while still keeping your packages separate.
Each package/member has its own Cargo.toml file, and they can be built and tested independently. This can be particularly useful for larger projects.

- The format of the `[member]` section is a list of file or directory paths, each on a new line, that are relative to the package's root directory.
- package members file can have separate `main.rs` files.
- member with a `main.rs` file will be built as a separate binary.
- It's also possible to have a package with multiple binary members in which each of them can have its own `main.rs` and `lib.rs` if needed.
- This can be useful for creating multiple command line utilities or applications within a single package.
```toml
# Here's an example directory structure for a workspace:
/my_project
  Cargo.toml
  /my_library
    Cargo.toml
  /my_binary
    Cargo.toml

// In this case, the top-level Cargo.toml file is used to define the workspace:
[workspace]
members = [
    "my_library",
    "my_binary",
]

// And each of the my_library and my_binary directories contains a Cargo.toml for their own package:
// my_library/Cargo.toml
[package]
name = "my_library"
version = "0.1.0"
edition = "2021"

[dependencies]

// my_binary/Cargo.toml
[package]
name = "my_binary"
version = "0.1.0"
edition = "2021"

[dependencies]
my_library = { path = "../my_library" }
```



# Packages, Crates, Modularity ....................................................................
- Packages provide functionality and include a Cargo.toml file that describes how to build a bundle of 1+ crates.
- Crates are a tree of modules, where a binary crate creates an executable and a library crate compiles to a library.

Rust code is organized on two levels:
1. as a tree of modules inside a crate
2. and as a directed acyclic graph of crates

- Cyclic dependencies are allowed between the modules, but not between the crates.
- Crates are units of reuse and privacy: only crate's public API matters
- crates are anonymous, so you don't get name conflicts and dependency hell when mixing several versions of the same crate in a single crate graph.
- This makes it very easy to make two pieces of code not depend on each other (non-dependencies are the essence of modularity): just put them in separate crates.
- During code review, only changes to Cargo.toml need to be monitored carefully.

- Most of the time when Rustaceans say "crate", they mean library crate, and they use "crate" interchangeably with the general programming concept of a "library"
- A package can contain as many binary crates as you like, but at most only one library crate.
- If a package contains `src/main.rs` and `src/lib.rs`, it has two crates: a binary and a library, both with the same name as the package.

## Modules

---
<!--ID:1689490637284-->
1. What is a module?
> - all files and folders are modules
> - Modules are a privacy boundary: items are private by default (hides implementation details).
> - module: `mod xxx {}`, provides encapsulation
> - folders as modules: file named `mod.rs` can exist (similar to `lib.rs`)
> - think of the `mod.rs` file as defining the interface to your module
> ```rust
> src/
>   collections/
>     prefixed.rs
> // lib.rs, main.rs
> mod collections {
>     pub mod prefixed;
> }
> src/
>   collections/
>     mod.rs
>     prefixed.rs
> // collections/mod.rs:
> pub mod prefixed;
> ```

---

## Visibility

---
<!--ID:1689490637285-->
1. Explain item visibility.
> Everything inside a module (ie, a file or subfolder within the /src folder) can access anything else within that module.
> Everything outside a module can only access public members of that module.
> A module can bring symbols from another module into scope: `use std::collections::HashSet;`
> each item (function, struct, enum, MODULE, etc.) controls its own visibility.
> ```rust
> // lib.rs
> pub mod col {  // pub required
>     pub mod prefixed {  // pub required
>         pub fn hello() {  // pub required
>             println!("Hello, world!");
>         }
>     }
> }
> use twlr::col::prefixed::hello;
> fn main() {
>     hello();
> }
> ```

---

## imports
- When import with `mod`, Rust automatically creates a module namespace.
- The module namespace is automatically taken from the file name
- import all public names from a module with a wildcard `::*`
- access the root of the module tree (ie, the main module in this case) using `crate::`
```rust
mod geo;
use geo::calculations::distance as d
```

## re-exports
- act of exposing the contents of another crate in the current crate's public interface.
- allows users of the current crate to use the types, functions, and constants of the re-exported crate as if they were defined in the current crate.
- re-export a commonly used crate to make it easier for users of your crate to access its functionality,
```rust
extern crate collections;
pub use collections::HashMap;  // would allow users of your crate to use the HashMap type as follows:

use your_crate::HashMap;
```

## prelude
pattern for making available all types you want to be public
```
mod foo;
mod prelude {                             <-- Create module inline
  pub use crate::foo::{MyStruct,Another}; <-- Note the 'pub' here!
}
use crate::prelude::*;                    <-- Make the types exposed
                                              in the prelude
                                              available
fn main() {
  let _ms = MyStruct {};
  let _a = Another {};
}
```

## Tricks
- un-conditional sharing of code:
```rust
// lib.rs:
#[cfg(test)]
mod test_commons;

// use it in unit tests
#[cfg(test)]
mod tests {
    use super::*;
    use crate::test_commons::{self,ContainerKind,Blocking};
...
// us in integration tests
#[path = "../../src/test_commons.rs"] mod test_commons;
```

## Features
- to find out the available features: read Cargo.toml on Github
