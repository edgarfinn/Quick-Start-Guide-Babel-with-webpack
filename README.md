# Quick-Start-Guide-Babel-with-webpack
A reference guide for setting up Babel compiler with webpack

Initialise your project:
- ```npm init -y```

Install express as dependency:
- ```npm i express -S```

Create a server file:

- ```atom index.js```

```js

const express = require('express');
const path = require('path');

const app = express();

app.use(express.static(path.join(__dirname, '/dist/')));

app.get('*', (req, res) => {
  res.sendFile(path.join(__dirname+'/dist/'));
});

const port = process.env.PORT || 5000;

app.listen(port);

console.log(`App listening on ${port}`);

```

Install webpack with the below listed dev dependencies:

- ```npm install webpack@3.11.0 html-webpack-plugin webpack-dev-server@2.11.1 clean-webpack-plugin webpack-merge babel-core babel-loader babel-preset-env babel-preset-react uglifyjs-webpack-plugin -D```

Create an index.html file for your browser to render:

- ```atom public/index.html```

...and populate it with essentially a div with a targetable ```id``` tag:

```html
<!-- public/index.html -->
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Webpack React</title>
</head>
<body>
  <div id="root">

  </div>
</body>
</html>

```

Create a 'common' webpack config file for both dev and prod environments to share...
- ```atom webpack.common.js```

...then:
  - Define your applications entry point (from which the dependency tree will be determined)
  - Define output (where to save outputted files)
  - Tell webpack to use babel-loader for all JS and JSX files (except node_modules)
  - Tell webpack (using ```html-webpack-plugin```) to bundle an index.html file together with your bundled JS and JSX to the same output destination (so html and JS are still able to collaborate)

```js

const path = require('path');
const CleanWebpackPlugin = require('clean-webpack-plugin');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/index.js',
  output: {
     path: path.resolve(__dirname, 'dist'),
     filename: 'index.bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: "babel-loader"
      },
      {
        test: /\.jsx$/,
        exclude: /node_modules/,
        loader: "babel-loader"
      }
    ]
  },
  plugins: [
    new CleanWebpackPlugin(['dist']),
    new HtmlWebpackPlugin({
      template: './public/index.html',
      filename: 'index.html',
      inject: 'body'
    })
  ]
};

```

Create a production-specific config file:

- ```atom webpack.prod.js```

...and configure it to clean out the 'dist' folder and minify your code on every build:

```js
const merge = require('webpack-merge');
const webpack = require('webpack');
const UglifyJSPlugin = require('uglifyjs-webpack-plugin');
const common = require('./webpack.common.js');

module.exports = merge(common, {
  plugins: [
    new UglifyJSPlugin({
      sourceMap: true
    }),
    new webpack.DefinePlugin({
      'process.env.NODE_ENV': JSON.stringify('production')
    })
  ]
});
```

Create a dev-specific webpack config file for running a dev server, with dev tools:

- ```atom webpack.dev.js```

```js
const merge = require('webpack-merge');
const common = require('./webpack.common.js');
module.exports = merge(common, {
  devtool: 'inline-source-map',
  devServer: {
    contentBase: './dist'
  }
});

```

Create a babel config file...
- ```atom .babelrc```

...and configure it to use the ```babel-preset-env and babel-preset-react``` preset:

```js
{
  "presets": ["env", "react"]
}
```

In ```package.json```, create a "build" script that calls webpack, and a "start" script that runs webpack-dev-server:

```js
"scripts": {
  "start": "node index.js",
  "dev": "webpack-dev-server --config webpack.common.js",
  "build": "webpack --config webpack.prod.js",
  "postinstall": "webpack --config webpack.prod.js",
}
```
Install React and ReactDOM:

- ```npm install react react-dom -S```

Start writing React components:

- ```atom src/components/App.js```

```js
// src/components/App.js
import React, {Component} from 'react';

class App extends Component {
  render() {
    return (
      <div>
        <h1>Welcome To React Custom!</h1>
      </div>
    )
  }
}

export default App;
```

Finally, tell your index file to render your component to your index file:

- ```atom src/index.js```

```js
//index.js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './components/App';

ReactDOM.render(
  <App />, document.getElementById('root')
)

```

Create a ```Procfile``` for heroku:

- ```atom Procfile```

and tell it what to run after in order to launch your app:

```js
web: node index.js
```

Gitigore your ```node_modules```:

- ```atom .gitignore```

```js
node_modules/
```



To preview your code with webpack-dev-server:

- npm start

To export production files

- npm run build

Your production build will then be exported to the output location defined in your ```webpack.config.js``` file (in this case: dist/index.bundle.js)
