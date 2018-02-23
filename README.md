# Quick-Start-Guide-Babel-with-webpack
A reference guide for setting up Babel compiler with webpack

Initialise your project:
- ```npm init```

Install webpack, html-webpack-plugin, webpack-dev-server, babel-core, babel-loader, babel-preset-env and babel-preset-react as dev dependencies:

- ```npm install webpack html-webpack-plugin webpack-dev-server babel-core babel-loader babel-preset-env babel-preset-react --save-dev```

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

Create a config file for webpack...
- ```atom webpack.config.js```

...then:
  - Define your applications entry point (from which the dependency tree will be determined)
  - Define output (where to save outputted files)
  - Tell webpack to use babel-loader for all JS and JSX files (except node_modules)
  - Tell webpack (using ```html-webpack-plugin```) to bundle an index.html file together with your bundled JS and JSX to the same output destination (so html and JS are still able to collaborate)

```js
const path = require('path');
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
    new HtmlWebpackPlugin({
      template: './public/index.html',
      filename: 'index.html',
      inject: 'body'
    })
  ]
};

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
    "start": "webpack-dev-server",
    "build": "webpack"
}
```
Install React and ReactDOM:

- ```npm install react react-dom --save-dev```

Start writing React components:

```atom src/components/App.js```

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

```atom src/index.js```

```js
//index.js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './components/App';

ReactDOM.render(
  <App />, document.getElementById('root')
)

```

To preview your code with webpack-dev-server:

- npm start

To export production files

- npm run build

Your production build will then be exported to the output location defined in your ```webpack.config.js``` file (in this case: dist/index.bundle.js)
