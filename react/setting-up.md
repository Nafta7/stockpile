# Setting up React with NPM, Babel, and Webpack

## Dependencies

```
npm i -S react react-dom react-router
```

## Development dependencies

```
npm i -D html-webpack-plugin webpack webpack-dev-server babel-{core,loader} babel-preset-{react,es2015}
```

## Configuration files

### webpack.config.babel.js

```js
const HtmlWpp = require('html-webpack-plugin')
const htmlConfig = new HtmlWpp({
  template: `${__dirname}/app/index.html`,
  filename: 'index.html',
  inject: 'body'
})

module.exports = {
  entry: [
    './app/index.js'
  ],
  output: {
    path: `${__dirname}/dist`,
    filename: 'index_bundle.js'
  },
  module: {
    loaders: [
      {
        test:  /\.js$/,
        exclude: /node_modules/,
        loader: 'babel-loader',
        query: {
          presets: ['es2015', 'react']
        }
      }
    ]
  },
  plugins: [htmlConfig]
}
```

## Scaffold files

### app/index.html
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>App title</title>
    <!-- Needs to install bootstrap via npm if you wanna use this way -->
    <link rel="stylesheet" href="../node_modules/bootstrap/dist/css/bootstrap.css"
      media="screen" title="no title" charset="utf-8">
  </head>
  <body>
    <div id="app">
    </div>
  </body>
</html>
```

### app/index.js

```js
import React from 'react'
import ReactDOM from 'react-dom'
import routes from './config/routes'

ReactDOM.render(
  routes,
  document.getElementById('app')
)

```

### app/config/routes.js

```js
import React from 'react'
import { Router, Route, IndexRoute, hashHistory } from 'react-router'
import HelloWorld from '../components/HelloWorld'

const routes = (
  <Router history={hashHistory}>
    <Route path='/' component={HelloWorld}>
      <IndexRoute component={HelloWorld} />
      <Route path="home" header="Home" component={HelloWorld} />
    </Route>
  </Router>
)

export default routes
```

### app/components/HelloWorld.js

```js
import React from 'react'

const HelloWorld = () =>
  <div>
    <h1>Hello World!</h1>
  </div>

export default HelloWorld
```

## package.json

Scripts section:

```json
"scripts": {
    "production": "webpack -p",
    "start": "webpack-dev-server --inline -d"
  }
```

Based on the tutorial called 'React.js Fundamentals' made by Tyler McGinnis: http://courses.reactjsprogram.com/courses/reactjsfundamentals/
