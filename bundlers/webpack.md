# Webpack Concepts

## webpack.config.js

#### Entry

By default its value is `./src/index.js`

```js
module.exports = {
  entry: './path/to/my/entry/file.js',
}
```

#### Output
By default its value is `./dist/main.js`
```js
const path = require('path')

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js',
  },
}
```

#### Loaders

```js
const path = require('path')

module.exports = {
  output: {
    filename: 'my-first-webpack.bundle.js',
  },
  module: {
    rules: [{ test: /\.txt$/, use: 'raw-loader' }],
  },
}
```

#### Plugins

```js
const HtmlWebpackPlugin = require('html-webpack-plugin') //installed via npm
const webpack = require('webpack') //to access built-in plugins

module.exports = {
  module: {
    rules: [{ test: /\.txt$/, use: 'raw-loader' }],
  },
  plugins: [new HtmlWebpackPlugin({ template: './src/index.html' })],
}
```

#### Mode

it can be: `development`, `production` or `none`
The default value is `production`.

```js
module.exports = {
  mode: 'production',
}
```
