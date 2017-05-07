# PostCSS setup

## Installation

### The CLI tool

```bash
yarn add -D postcss-cli
```

### Plugins

```bash
yarn add -D postcss-css-variables postcss-custom-media postcss-easy-import postcss-nesting css-mqpacker
```

### SUGARSS - The parser

```bash
yarn add -D sugarss
```
### OR Install everything

```bash
yarn add -D postcss-cli postcss-css-variables postcss-custom-media postcss-easy-import postcss-nesting sugarss
```
## Configuration

### NPM scripts

```json
"scripts": {
  "build:css": "postcss -c ./postcss-config.js",
  "watch:css": "postcss -c ./postcss-config.js -w",
  "minify:css": "postcss -u cssnano dist/main.css -o dist/main.min.css"
}
```
### PostCSS configuration file
`postcss-config.js`
```js
module.exports = {
  use: [
    "postcss-easy-import",
    "postcss-nesting",
    "postcss-css-variables",
    "css-mqpacker",
    "postcss-custom-media"
  ],
  parser: "sugarss",
  "postcss-easy-import": {
    extensions: ['.sss'],
    onImport: function(sources) {
      global.watchCSS(sources)
    }
  },
  "input": "app/styles/main.sss",
  "output": "dist/main.css"
};
```
