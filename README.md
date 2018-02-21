# Quick-Start-Guide-Babel-with-webpack
A reference guide for setting up Babel compiler with webpack

Initialise your project:
- ```npm init```

Install webpack, babel-core, babel-loader, and babel-preset-env as dev dependencies:
- ```npm install webpack babel-core babel-loader babel-preset-env --save-dev```

Create a config file for webpack...
- ```atom webpack.config.js```

...then:
  - Define your applications entry point (from which the dependency tree will be determined)
  - Define output (where to save outputted files)
  - Tell webpack to use babel-loader for all JS files (except node_modules)
```js
const path = require('path');

module.exports = {
  entry: './index.js',
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
      }
    ]
  }
};
```

Create a babel config file...
- ```atom .babelrc```

...and tell it to use the ```babel-preset-env``` preset:

```js
{
  "presets": ["env"]
}
```

- In ```package.json```, create a build script that calls webpack:

```js
"scripts": {
    "build": "webpack"
  }
```

Start coding in ```index.js```. To set an alternative root file for your dependency tree, update the ```entry``` key in ```webpack.config.js``` (and the appropriate output filename / path)

## Jest additional setup:

- Install jest, babel-jest, and regenerator-runtime

```npm install jest babel-jest regenerator-runtime --save-dev```
