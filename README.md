# not-pong

A game written in Rust for the [WASM-4](https://wasm4.org) fantasy console.

## Building

Build the cart by running:

```shell
cargo build --release
```

Then run it with:

```shell
w4 run target/wasm32-unknown-unknown/release/cart.wasm
```

### Generating a standalone HTML file

The w4 tool provided by the WASM-4 project has a bundle feature that allows you to generate a standalone HTML file that fully contains the game.  This can then be embedded into another web page using something like an iframe.  The bundle feature supports the Mustache HTML template language.

```
w4 bundle target/wasm32-unknown-unknown/release/cart.wasm --title "not-pong" --html not-pong.html --html-template template.html.mustache
```

For more info about setting up WASM-4, see the [quickstart guide](https://wasm4.org/docs/getting-started/setup?code-lang=rust#quickstart).

## Links

- [Documentation](https://wasm4.org/docs): Learn more about WASM-4.
- [Snake Tutorial](https://wasm4.org/docs/tutorials/snake/goal): Learn how to build a complete game
  with a step-by-step tutorial.
- [GitHub](https://github.com/aduros/wasm4): Submit an issue or PR. Contributions are welcome!
