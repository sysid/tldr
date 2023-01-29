# cargo

> Manage Rust projects and their module dependencies (crates).
> Some subcommands such as `cargo build` have their own usage documentation.
> More information: <https://crates.io>.

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
- dev-dependencies documentation cannot be generated

```bash
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
- Typically each codebase has only a single workspace, which for small projects often only contains a single package.
- Each package has its own `Cargo.toml` file.
- Each package may contain one or zero library crates and multiple binary test and benchmark crates.
- Crates have a root module code file and code files included from this.
- many people use the term "crate" when they actually mean packages. This is because each package contains only one library crate that is assoziated with the package.

## main.rs, Binaries
- [Cargo Targets - The Cargo Book](https://doc.rust-lang.org/cargo/reference/cargo-targets.html#binaries)
- `main.rs`: entry point for an executable crate.
- If the crate has multiple executables, each executable will have its own `*.rs` file in the `src/bin` directory.
- to make a crate a lib you just rename the file main.rs to lib.rs, voil, now is a library

There are two ways to create executable targets that don't have main.rs at their root.
### Method 1: Declare a binary target in cargo.toml
- You'll then be able to run your example target via `$ cargo run --bin example`
- Any number of binary targets can be declared in this way.
- Add an entry to you cargo.toml that resembles the following:
```toml
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
- members will be included in the package when it is built using the cargo build command.
- The format of the `[member]` section is a list of file or directory paths, each on a new line, that are relative to the package's root directory.
- package members file can have separate `main.rs` files.
- When building the package using cargo build, each member with a `main.rs` file will be built as a separate binary.
- It's also possible to have a package with multiple binary members in which each of them can have its own `main.rs` and `lib.rs` if needed.
- This can be useful for creating multiple command line utilities or applications within a single package.
```toml
// master Cargo.toml as shell
[workspace]
members = ["db_stuff", "ftbmh"]

// ftbmh/Cargo.tom
[package]
// your stuff

[dependencies]
// your dependencies
db_stuff = { path = "../db_stuff" } // as you may know the `..`
// in the path refers to the mother folder of the current one
```


# Packages, Crates, Modularity ....................................................................
Rust code is organized on two levels:
1. as a tree of inter-dependent modules inside a crate
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
- In Rust, all files and folders are modules
- module: `pub mod xxx {}`, provides encapsulation
- folders as modules: file named `mod.rs` must exist
- think of the `mod.rs` file as defining the interface to your module

## Visibility
Everything inside a module (ie, a file or subfolder within the /src folder) can access anything else within that module.
Everything outside a module can only access public members of that module.

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
