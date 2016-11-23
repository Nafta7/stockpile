# NPM build scripts

## NPM package.json

```js
{
  "name": "",
  "version": "0.0.1",
  "description": "",
  "main": "",
  "scripts": {
    "test": "",
    "clean": "rm -r dist/*",
    "pages:clean": "rm pages/*.html",
    "deploy:site": "npm run pages:build && npm run pages:min && npm run pages:push && npm run pages:clean",
    "pages:min": "npm run pages:min:index | npm run pages:min:demo",
    "pages:min:index": "usemin pages/index.html --dest pages -o pages/index.html --htmlmin true --rmlr true",
    "pages:min:demo": "usemin pages/demo.html --dest pages -o pages/demo.html --htmlmin true --rmlr true",
    "pages:push": "gh-pages -d pages",
    "pages:build": "npm run build:css | npm run pages:build:js | npm run pages:build:pug",
    "pages:build:pug": "pug pages/views/*.pug --out pages/ -P",
    "pages:build:css": "node-sass pages/styles -o pages/css",
    "pages:build:js": "browserify -d pages/js/app.js -o pages/js/bundle.js -t [ babelify --presets [ es2015 ] ]",
    "pages:watch:js": "watchify pages/js/app.js -d -o pages/js/bundle.js -t [ babelify --presets [ es2015 ] ]",
    "pages:watch:css": "npm run pages:build:css -- -w",
    "pages:watch:pug": "pug pages/views/*.pug --out pages/ -w",
    "pages:watch": "npm run pages:watch:css | npm run pages:watch:js",
    "pages:sync": "node app & browser-sync start --config bs-config.js",
    "pages:sync_inline": "browser-sync start --server 'pages' --index 'demo.html' --files 'pages/css/*.css, pages/js/*.js, pages/*.html, pages/views/*.pug'",
    "pages:serve": "npm run pages:watch | npm run pages:sync",
    "build:js": "babel --presets es2015 -d lib/ src/scripts",
    "build:css": "node-sass src/styles -o dist/",
    "build": "npm run build:css && npm run build:js",
    "prepublish": "npm run build:js && npm run build:css"
  },
  "keywords": [],
  "author": "",
  "license": "",
  "devDependencies": {
    "babel-cli": "^6.14.0",
    "babel-preset-es2015": "^6.14.0",
    "babelify": "^7.3.0",
    "browser-sync": "^2.16.0",
    "browserify": "^13.1.0",
    "express": "^4.14.0",
    "gh-pages": "^0.11.0",
    "node-sass": "^3.8.0",
    "prismjs": "^1.5.1",
    "pug-cli": "^1.0.0-alpha6",
    "usemin-cli": "^0.1.0",
    "watchify": "^3.7.0"
  }
}
```

# Express app

`app.js`

```js
const express = require('express')
const app = express()
app.set('view engine', 'pug')
app.set('views', 'pages/views')
app.use(express.static('pages'))
app.use(express.static('./'))


app.get('/', function (req, res) {
  res.render('demo')
})

app.get('/index', function (req, res) {
  res.render('index')
})

app.listen(3000, function () {
  console.log('Running...')
})

```

# Browser-sync config settings

`bs-config.js`

```js
module.exports = {
  files: ['pages/css/*.css', 'pages/js/*.js', 'pages/views/*.pug'],
  //proxy: 'localhost:3000',
  server: {
     baseDir: ['./', 'pages']
  },
  startPath: '/pages/html/demo.html'
}
```
