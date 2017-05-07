# Webpack 2

## install webpack

```bash
yarn add -D webpack
```

Initial  'webpack.config.js':
```js
const path = require('path')

const config = {
  entry: path.resolve('src', 'main.js'),

  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'main.js',
    publicPath: '/dist/'
  },

  module: {
    ....
  },

  plugins: [
    ....
  ]
}
```

## build
```bash
webpack
```

## webpack-dev-server
```bash
yarn add -D webpack-dev-server
```

## loaders

### babel loader

```bash
yarn add -D babel-{core,preset-es2015} babel-loader
```

add new rule to 'webpack.config.js':
```js
  ...
  module: {
    rules: [
      {
        test: /\.js$/,
        include: path.resolve('src'),
        exclude: /node_modules/,
        use: [{
          loader: 'babel-loader',
          options: {
            presets: ['es2015']
          }
        }]
      }
    ]
  }
```

### css loaders

```bash
yarn add -D css-loader style-loader
```

add new rule to 'webpack.config.js':
```js
...
{
  test: /\.css$/,
  include: path.resolve('src'),
  use: [{
    loader: 'style-loader',
  }, {
    loader: 'css-loader'
  }]
}
...
```

## deploy to production

add a new script called 'build:prod' in our 'package.json':
```js
  "scripts": {
    "build": "webpack",
    "build:dev": "webpack --watch",
    "build:prod": "NODE_ENV=production webpack",
    "start": "webpack-dev-server"
  }
```

install:
```bash
npm i -D webpack-config-utils
```

add to webpack.config:
```js
var webpack = require('webpack')
var { getIfUtils, removeEmpty } = require('webpack-config-utils')
var {
  ifProduction,
  ifNotProduction
} = getIfUtils(process.env.NODE_ENV || 'development')

...
plugins: {
  removeEmpty([
    ifProduction(new webpack.optimize.UglifyJsPlugin())
  ])
}

```

## extract css file

```bash
yarn add -D extract-text-webpack-plugin
```

add to webpack:
```js
const ExtractTextPlugin = require('extract-text-webpack-plugin')

...js
plugins: {
  removeEmpty([
    ifProduction(new webpack.optimize.UglifyJsPlugin()),
    new ExtractTextPlugin('main.css')
  ])
}
```

also update css ruler:
```js
{
  test: /\.css$/,
  include: path.resolve('src'),
  loader: ExtractTextPlugin.extract('css-loader')
}
```
