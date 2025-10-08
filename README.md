# fctc-website-yew

This is a quick and dirty remake of the FCTC website.  Primarily written in [Rust](https://rust-lang.org/) using the [Yew framework](https://yew.rs/).

## Running Locally

1. Clone this repository.

2. Install Rust by following the instructions at https://rustup.rs/

3. Add the wasm32-unknown-unknown Rust compilation target for browser-based WebAssembly.

```
rustup target add wasm32-unknown-unknown
```

4. Install Trunk

```
# note that this might take a while to install because it compiles everything from scratch
# Trunk also provides prebuilt binaries for a number of major package managers
# See https://trunkrs.dev/#install for further details
cargo install --locked trunk
```

5. Run Project
   
While running the site under `trunk serve` any time changes are saved to the source files, trunk will rebuild the site and refresh the browser.  This allows for real-time updating of the site.

```
trunk serve
```

## Generate a Static Site

This will produce a fully built version of the site in the dist directory.  It can then be served separately with something like `python3 -m http.server --directory dist`.

```
trunk build --release
```
