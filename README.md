### Generating a standalone HTML file

The w4 tool provided by the WASM-4 project has a bundle feature that allows you to generate a standalone HTML file that fully contains the game.  This can then be embedded into another web page using something like an iframe.  The bundle feature supports the Mustache HTML template language.

```
w4 bundle target/wasm32-unknown-unknown/release/cart.wasm --title "not-pong" --html not-pong.html --html-template template.html.mustache
```
