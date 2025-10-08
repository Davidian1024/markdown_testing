fctc-website-yew
----------------

This is a quick and dirty remake of the FCTC website.  Primarily written in Rust using the Yew framework.

running this locally
--------------------

Clone this repository.

Install Rust by following the instructions at https://rustup.rs/

Add the wasm32-unknown-unknown Rust compilation target for browser-based WebAssembly.

```
rustup target add wasm32-unknown-unknown
```

Install Trunk

```
# note that this might take a while to install because it compiles everything from scratch
# Trunk also provides prebuilt binaries for a number of major package managers
# See https://trunkrs.dev/#install for further details
cargo install --locked trunk
```
