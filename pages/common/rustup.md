# rustup

> Rust toolchain installer.
> Install, manage, and update Rust toolchains.
> More information: <https://github.com/rust-lang/rustup.rs>.

- Install the nightly toolchain for your system:

`rustup install nightly`

- Switch the default toolchain to nightly so that the `cargo` and `rustc` commands will use it:

`rustup default nightly`

- Use the nightly toolchain when inside the current project, but leave global settings unchanged:

`rustup override set nightly`

- Update all toolchains:

`rustup update`

- List installed toolchains:

`rustup show`

- Run cargo build with a certain toolchain:

`rustup run {{toolchain_name}} cargo build`

- Open the local rust documentation in the default web browser:

`rustup doc`


# Custom ...........................................................................................

1. Managing Rust components:
    rustup allows you to manage Rust components, such as rust-std, rust-docs, rust-src, cargo, clippy, and rustfmt.
    You can add, remove, or update these components using the rustup component command. asdf doesn't provide built-in support for managing these components.

2. Managing Rust targets:
    rustup lets you manage Rust target platforms (for cross-compilation).
    You can add or remove target platforms using the rustup target command. This feature is not available in asdf.

3. Updating the Rust toolchain:
    With rustup, you can easily update your Rust toolchain to the latest stable, beta, or nightly release using the rustup update command.
    While you can still update Rust with asdf, the process requires manually checking for the latest version and then installing it.

4. Rustup-init:
    rustup provides the rustup-init script that automatically installs the Rust toolchain and configures your environment.
    asdf requires manual installation of the Rust plugin and manual configuration of the environment.

5. Rustup-specific commands:
    rustup provides some commands specific to the Rust ecosystem, such as rustup override, rustup show, rustup toolchain, and rustup self.
    These commands allow you to manage multiple Rust toolchains and their settings with more control than asdf.
