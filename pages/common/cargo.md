# cargo

> Manage Rust projects and their module dependencies (crates).
> Some subcommands such as `build` have their own usage documentation.
> More information: <https://doc.rust-lang.org/cargo>.

- Search for crates:

`cargo search {{search_string}}`

- Install a binary crate:

`cargo install {{crate_name}}`

- List installed binary crates:

`cargo install --list`

- Create a new binary or library Rust project in the specified directory (or the current working directory by default):

`cargo init --{{bin|lib}} {{path/to/directory}}`

- Add a dependency to `Cargo.toml` in the current directory:

`cargo add {{dependency}}`

- Build the Rust project in the current directory using the release profile:

`cargo build --release`

- Build the Rust project in the current directory using the nightly compiler (requires `rustup`):

`cargo +nightly build`

- Build using a specific number of threads (default is the number of logical CPUs):

`cargo build --jobs {{number_of_threads}}`



# Operations ............................................................................................
- dev-dependencies documentation cannot be generated. Workaround: add it as regular dependency
- examples of deps can be run directly, crates seem to be self sufficient
- binary files for the cargo-edit commands (such as cargo-add, cargo-rm, and cargo-upgrade) are installed in the bin directory under the default Cargo home directory.
```bash
# create new project
cargo new my_binary_project --bin
cd my_binary_project
cargo run

# install all deps
cargo build

cargo add serde --features derive  # list features via crate's Cargo.toml

cargo tree
cargo tree --prefix none

# list example targets
cargo run --example

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

## Environment variables Cargo sets for crates

---
<!--ID:1690266452871-->
1. How to get important automatic Cargo variables via environment variables in your program?
> ```rust
> let version = env!("CARGO_PKG_VERSION");
> ```
> Note that if one of these values is not provided in the manifest, the corresponding environment variable is set to the empty string, `""`.
> `CARGO_MANIFEST_DIR` -- The directory containing the manifest of your package (Cargo.toml)
> not usefull for `include_dir` because it requires string literal instead of string
> [Environment Variables - The Cargo Book](https://doc.rust-lang.org/cargo/reference/environment-variables.html#environment-variables-cargo-sets-for-crates)

---


# Dependency management ...........................................................................
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
A Rust project can have more than one Cargo.toml file if it's a workspace.
- projects can be composed of multiple files (which are modules), that can be nested within folders (which are also modules).
- In order to access the root of that module tree, you can always use the `crate::` prefix.
- workspaces, containing packages, containing crates, containing multiple source files belonging to different modules.
- Cyclic dependencies are allowed between the modules, but not between the crates.

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

Rust code is organized on two levels:
1. as a tree of modules inside a crate
2. and as a directed acyclic graph of crates

- crates are anonymous, so you don't get name conflicts and dependency hell when mixing several versions of the same crate in a single crate graph.
- This makes it very easy to make two pieces of code not depend on each other (non-dependencies are the essence of modularity): just put them in separate crates.
- During code review, only changes to Cargo.toml need to be monitored carefully.

---
<!--ID:1703356138638-->
1. Explain `main.rs vs lib.rs`:
> structure separates the `main.rs` and `lib.rs` files, with `main.rs` being the entry point for binary applications and `lib.rs` serving as the main module for library code.
> `lib.rs` contains the library code (functions, structs, traits, etc.) that can be reused, while `main.rs` typically contains the application-specific logic.
>
> If you find yourself needing to import structs from `main.rs` into `lib.rs`, it often indicates a need to restructure your code.
> 1. **Move Shared Code to `lib.rs`**: Place any structs, functions, or traits that need to be shared between `main.rs` and other parts of your application in `lib.rs`.
> 2. **Import from `lib.rs` to `main.rs`**: In `main.rs`, you can import and use these shared items from your library.

---


## Workspace
- is a set of packages that share the same `Cargo.lock` and output directory.
- Typically each codebase has only a single workspace, which for small projects often only contains a single package.
- This allows to share common dependencies between packages and manage them in one place, while still keeping packages separate.
- Each package/member has its own `Cargo.toml` file, and they can be built and tested independently
- `[member]` section is a list of file or directory paths, each on a new line, that are relative to the package's root directory.
- member with a `main.rs` file will be built as a separate binary.
```toml
# Here's an example directory structure for a workspace:
/my_project
  Cargo.toml
  /my_library
    Cargo.toml
  /my_binary
    Cargo.toml

# In this case, the top-level Cargo.toml file is used to define the workspace:
[workspace]
members = [
    "my_library",
    "my_binary",
]

# And each of the my_library and my_binary directories contains a Cargo.toml for their own package:
# my_library/Cargo.toml
[package]
name = "my_library"
version = "0.1.0"
edition = "2021"

[dependencies]

# my_binary/Cargo.toml
[package]
name = "my_binary"
version = "0.1.0"
edition = "2021"

[dependencies]
my_library = { path = "../my_library" }
```


## Package
- include a Cargo.toml file that describes how to build a bundle of 1+ crates.
- many people use the term "crate" when they actually mean packages. This is because each package contains only one library crate that is assoziated with the package.
- Each package has its own `Cargo.toml` file.
- A package can contain as many binary crates as you like, but at most only one library crate.
- If a package contains `src/main.rs` and `src/lib.rs`, it has two crates: a binary and a library, both with the same name as the package.

### Crates
- Crates are a tree of modules, where a binary crate creates an executable and a library crate compiles to a library.
- Crates have a root module code file and code files included from this.
- Each package may contain max ONE library crate and multiple binary test and benchmark crates.
- Crates are units of reuse and privacy: only crate's public API matters
- Most of the time when Rustaceans say "crate", they mean library crate, and they use "crate" interchangeably with the general programming concept of a "library"

### imports
- When import with `mod`, Rust automatically creates a module namespace.
- The module namespace is automatically taken from the file name
- import all public names from a module with a wildcard `::*`
- access the root of the module tree (ie, the main module in this case) using `crate::`
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

### prelude
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
<!--ID:1690266452875-->
1. Given the structure:
```
├── confguard
│   └── tests.rs
├── confguard.rs
├── lib.rs
└── main.rs
```
Explain it (and `mod` vs `use`).
> library crate with an associated binary crate
> ### `mod`: define/bring modules into scope
> - **Purpose**: The `mod` keyword is used to declare a new module: namespaces
>   - This can be done either inline with `mod module_name { /* code */ }` or by linking to another file with the same name as the module (e.g., `mod module_name;` will link to `module_name.rs` or `module_name/mod.rs`).
>
> ### `use`: abbreviate path
>   - simplifies code. instead of referring to `math::add`, you can bring it into scope at the top of your file with `use math::add;` and then simply call `add()` in your code.
>   - The `use` keyword can also be used to bring in just a module, a specific function, or even use renaming for convenience.
>
> ### lib.rs:
> - This is the entry point for a Rust library crate.
> - Any public (i.e., pub) functions, structs, enums, etc. that you define in this file will be accessible when someone uses your library.
> - You can also `mod` other files to bring their contents into scope.
>
> ### confguard.rs:
> - module file. Inside lib.rs, you find a line `mod confguard;`, which would bring the contents of confguard.rs into scope.
> - definitions inside confguard.rs are scoped under the confguard module. would access `foo` as `confguard::foo` from other modules or crates (assuming it's public).
>
> ### confguard/:
> - confguard is a module, defined by the presence of confguard.rs which acts like `mod.rs`
> - When you have a module that grows in complexity and needs to be split into multiple files, you can use a directory with the module's name and move the module's content into that directory.
>
> ### confguard/tests.rs:
> - submodule module of the confguard module
> ```rust
> // If to access the tests.rs file (which is a sub-module) from confguard.rs
> // confguard.rs
> mod tests;
>
> // And if you want to use something from the tests module:
> // confguard.rs
> mod tests;
> use tests::some_function;
> ```

<!--ID:1703152115449-->
1. Where does Rust look for modules?
> ### Root File (main.rs or lib.rs), entrypoint
> - **Main Crate**: For a binary crate, the root file is typically `main.rs`.
> - **Library Crate**: For a library crate, it's usually `lib.rs`.
> - These root files serve as the entry points for compiling the crate and can define modules or reference other files as modules.
>
> ### Module Declaration and File System
> 1. **Inline Modules**:
>    - Defined directly in the file using the `mod` keyword followed by a block of code.
>    - Example: `mod mymodule { /* module code */ }` within `main.rs` or `lib.rs`.
>
> 2. **File-Based Modules**:
>    - When you declare a module with `mod module_name;` (without an inline block), Rust looks for the module's code in a separate file.
>    - The compiler looks for a file named `module_name.rs` or a directory named `module_name` with a `mod.rs` file inside it.
>    - This file or directory should be in the same directory as the file that contains the `mod module_name;` declaration.
>
> ### Nested Modules
> - For nested modules, the path extends in the file system. For example, if you declare `mod nested;` inside `module_name.rs`, Rust looks for `module_name/nested.rs` or `module_name/nested/mod.rs`.
>
> ### Example Structure
> - `src/main.rs` or `src/lib.rs` (root)
>    - `mod utils;` (looks for `src/utils.rs` or `src/utils/mod.rs`)
>    - Inside `utils.rs`:
>      - `mod helpers;` (looks for `src/utils/helpers.rs` or `src/utils/helpers/mod.rs`)

---

### Hierarchical Module Structure

---
<!--ID:1691326957561-->
1. How to structure a hierarchical module in Rust?
> ```rust
> src/
> |-- geometry/
> |   |-- circle/
> |   |   |-- area.rs
> |   |   |-- circumference.rs
> |   |   |-- tests/
> |   |       |-- area_tests.rs
> |   |       |-- circumference_tests.rs
> |   |-- rectangle/
> |   |   |-- area.rs
> |   |   |-- perimeter.rs
> |   |   |-- tests/
> |   |       |-- area_tests.rs
> |   |       |-- perimeter_tests.rs
> |   |-- circle.rs
> |   |-- rectangle.rs
> |-- geometry.rs
> |-- lib.rs
>
> // lib.rs
> pub mod geometry;
>
> // geometry.rs
> pub mod circle;
> pub mod rectangle;
> // geometry/circle.rs
> pub mod area;
> pub mod circumference;
> #[cfg(test)]
> mod tests;
> // geometry/rectangle.rs
> pub mod area;
> pub mod perimeter;
> #[cfg(test)]
> mod tests;
> ```
<!--ID:1692204458347-->
1. What is a submodule?
> - A module is a way to organize code within a crate into separate namespaces.
> - Modules can be nested within other modules, and when they are, the nested module is often referred to as a "submodule."
> ```rust
> // lib.rs or main.rs
> mod outer {
>     // content of the outer module
>
>     mod inner {
>         // content of the inner submodule
>     }
> }
>
> .
> └── outer.rs          // This file corresponds to the `outer` module
> └── outer/            // This directory also corresponds to the `outer` module due to the Rust 2018 edition's path clarity
>     └── mod.rs        // In older Rust editions, this would be the primary file for the `outer` module when split across multiple files
>     └── inner.rs      // This file corresponds to the `inner` submodule
> ```

---


### Spliting struct implementation accross files/modules
- use submodule directory structure
- integrate submodules in main-module
```rust
// main.rs
mod my_struct_impl;
pub struct MyStruct {
    pub data: i32,
}
fn main() {
    let instance = MyStruct::new(5);
    instance.display();
}

// my_struct_impl.rs
mod display_impl;  // !!!!!
use super::MyStruct;
impl MyStruct {
    pub fn new(value: i32) -> Self {
        MyStruct { data: value }
    }
}

// my_struct_impl/display_impl.rs
use super::MyStruct;
impl MyStruct {
    pub fn display(&self) {
        println!("Data: {}", self.data);
    }
}
```


## Features of a Crate
- only reliable way to explore available features of a crate: read Cargo.toml on Github



## Visibility
- Rust seems not to look at semantic structure (e.g. struct split over several impl files/modules), but only at fs structure

test_guard.rs does not see private members in guard.rs:
- Gotcha: sibling modules vs. sub-modules (must be in folder with same name)

├── confguard
│   ├── test_guard.rs
│   ├── guard.rs
├── confguard.rs  # mod test_guard;
├── lib.rs
└── main.rs

correct:
├── confguard
│   ├── guard
│   │   └── test_guard.rs
│   ├── guard.rs  # mod test_guard;
├── confguard.rs
├── lib.rs
└── main.rs

---
<!--ID:1689490637285-->
1. Explain item visibility.
> By default, in Rust, everything (including submodules) is private to its enclosing module.
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
<!--ID:1691587766819-->
1. What is the effect of `mod` keyword?
> Declaring a module.
> ```rust
> mod profile;
> mod settings;
> ```
> ## File Loading:
> - compiler will look for two files in same directory as the current file.
> - It will treat the contents of profile.rs as the profile module and the contents of settings.rs as the settings module (siblings)
>
> ## Namespace Creation:
> - Within the current module (or crate, if you're at the root), you now have two sub-modules
> - if `profile` has a public function named `get_profile`, you can call it as `profile::get_profile()` from outside the profile module but within the module that declares `mod profile;`.
>
> ## Path Resolution:
> - to use an item from one module in another module, use the full path or use the `use` keyword to bring it into scope
> - use inside settings module: ` use crate::profile::get_profile;`

<!--ID:1696482593670-->
1. Explain `mod` vs `use`.
> - `mod` declares and includes a module but requires you to use the module's prefix to access its items.
> - `use` brings specific items into the immediate scope, allowing you to use them without a prefix.
>
> While mod brings the module code into scope, you still need use for unqualified access to its items.

<!--ID:1696482593671-->
1. Explain `use super::*;`
> - imports all items from the parent module into the current module's scope.
> - This allows you to use those items without having to prefix them with `super::`.

---


## Rules of Thumb
- Cohesion: Code that is closely related in functionality or purpose should be kept together
- Ease of Navigation: One benefit of splitting code into multiple files is ease of navigation
- Reduce Merge Conflicts: In a team setting, if everyone is working on the same file, it can lead to frequent merge conflicts
- Compile Time: Rust's incremental compilation can speed up compile times if the codebase is organized into multiple files
- Test Organization: Using Rust's convention of placing unit tests at the bottom of the file they test (inside a #[cfg(test)] module) can lead to long files if there are many tests


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

#### Installation
```bash
cargo install --path .  # nothgin else needed in Cargo.toml
```
### Method 3: workspaces
[Building Multiple Binaries in Rust - crustc](https://crustc.com/building-multiple-binaries-in-rust/)


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

---
<!--ID:1690041467775-->
1. How to use functions from lib.rs in main.rs?
> - make sure the function is declared as pub (public) in lib.rs
> - import it: 'your_create_name' from Cargo.toml (NOT: lib.rs)
> ```rust
> // lib.rs
> pub fn say_hello() {
>     println!("Hello, world!");
> }
> // main.rs
> use your_crate_name::say_hello;  // replace `your_crate_name` with the actual name of your crate
> fn main() {
>     say_hello();
> }
> ```

---

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



# Compiler .........................................................................................
## Caching and Accelartor
[A random assortment of Rust notes – Brian Kung](https://briankung.dev/2023/07/16/rust-notes/)
[GitHub - mozilla/sccache: sccache is ccache with cloud storage](https://github.com/mozilla/sccache)
