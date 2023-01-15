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



# Cargo ............................................................................................
- dev-dependencies documentation cannot be generated

## Operations
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

## Structure
[Package Layout - The Cargo Book](https://doc.rust-lang.org/cargo/guide/project-layout.html)

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

The examples directory contains example code that demonstrates how to use the crate. Each example file is a Rust source file with a name that ends in `*.rs`.


### main.rs, Binaries
[Cargo Targets - The Cargo Book](https://doc.rust-lang.org/cargo/reference/cargo-targets.html#binaries)
`main.rs` file is the entry point for an executable crate.
If the crate has multiple executables, each executable will have its own `*.rs` file in the `src/bin` directory.
Instead of src/main.rs you can have src/bin/tool1.rs and src/bin/tool2.rs etc. If you then have src/lib.rs then all those can access a single support crate for code

There are two ways to create executable targets that don't have main.rs at their root.
#### Method 1: Declare a binary target in cargo.toml
- You'll then be able to run your example target via `$ cargo run --bin example`
- Any number of binary targets can be declared in this way.
- Add an entry to you cargo.toml that resembles the following:
```toml
[[bin]]
name = "example"
path = "src/example.rs"
```
#### Method 2: The bin directory
- All Rust files in the src/bin directory of your project act as binary targets.  No cargo.toml configuration required.
- If you move src/example.rs to `src/bin/example.rs`, you'll be able to immediately run your example binary with `$ cargo run --bin example`
- Do note that once you've declare more than one binary target, you will need to use the --bin flag whenever you invoke cargo run. For more information, see the Cargo Targets docs for binary targets.

### lib.rs
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
### Cargo.toml
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

## Packages, Crates, Modularity
Rust code is organized on two levels:
1. as a tree of inter-dependent modules inside a crate
2. and as a directed acyclic graph of crates

- Cyclic dependencies are allowed between the modules, but not between the crates.
- Crates are units of reuse and privacy: only crate's public API matters, and it is crystal clear what crate's public API is.
- Moreover, crates are anonymous, so you don't get name conflicts and dependency hell when mixing several versions of the same crate in a single crate graph.
- This makes it very easy to make two pieces of code not depend on each other (non-dependencies are the essence of modularity): just put them in separate crates.
- During code review, only changes to Cargo.tomls need to be monitored carefully.

- Most of the time when Rustaceans say "crate", they mean library crate, and they use "crate" interchangeably with the general programming concept of a "library"
- A package can contain as many binary crates as you like, but at most only one library crate.
- If a package contains `src/main.rs` and `src/lib.rs`, it has two crates: a binary and a library, both with the same name as the package.
- `pub mod xxx {}`, provides encapsulation
- location: `/Users/Q187392/.asdf/installs/rust/1.66.0/registry/src/github.com-1ecc6299db9ec823`

### imports
```rust
mod geo;
use geo::calculations::distance as d
```

### re-exports
- act of exposing the contents of another crate in the current crate's public interface.
- allows users of the current crate to use the types, functions, and constants of the re-exported crate as if they were defined in the current crate.
- re-export a commonly used crate to make it easier for users of your crate to access its functionality,
```rust
extern crate collections;
pub use collections::HashMap;  // would allow users of your crate to use the HashMap type as follows:

use your_crate::HashMap;
```

### Tricks
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
