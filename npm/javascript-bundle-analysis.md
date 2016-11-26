# JavaScript bundle analysis

## gzip-size-cli & pretty-bytes

Get the gzipped size of a file or stdin.

### install
```bash
npm i -g gzip-size-cli pretty-bytes
```

### usage
```bash
gzip-size bundle.min.js | pretty-bytes
```

[github](https://github.com/sindresorhus/gzip-size-cli)


## Source map explorer

Analyze and debug space usage through source maps.

### install
```bash
npm install -g source-map-explorer
```

### usage
```bash
source-map-explorer bundle.min.js
source-map-explorer bundle.min.js bundle.min.js.map
```

[github](https://github.com/danvk/source-map-explorer)


## disc | for browserify bundle

A tool for analyzing the module tree of a browserify bundle or node project.

### install
```bash
npm install -g disc
```

### usage
```bash
browserify --full-paths index.js | discify --open
```

[github](https://github.com/hughsk/disc)


## webpack-bundle-analyzer | for webpack bundle

Webpack plugin and CLI utility that represents bundle content as convenient interactive zoomable treemap.

### install

```bash
npm i -D webpack-bundle-analyzer
```

### usage

in `webpack.config.js`:
```js
var BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin;

// ...
plugins: [new BundleAnalyzerPlugin()]
// ...
```

[github](https://github.com/th0r/webpack-bundle-analyzer)

## package.json sample
```json
{
  "scripts": {
    "build": "browserify --full-paths -d src/app.js -o bundle.js",
    "inspect:browserify:bundle": "npm run build && discify bundle.js > disc.html --open",
    "inspect:gzip:size": "npm run build && gzip-size bundle.js | pretty-bytes",
  },
  "devDependencies": {
    "browserify": "latest",
    "disc": "latest",
    "gzip-size-cli": "latest",
    "pretty-bytes": "latest"
  }
}
```
