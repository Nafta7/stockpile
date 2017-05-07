# Browserify npm scripts

## Inferno as plugin

dependencies:
```bash
yarn add browserify babel-plugin-inferno babel-preset-es2015 babelify -D
```
script:
```bash
browserify --debug -d app/index.js -o bundle.js  -t [ babelify --presets [ es2015  ] --plugins [ inferno ] ]
```
