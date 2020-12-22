# STEPS
## Create Project
### Create a Directory
* `cd ~/Documents/Code`
* `mkdir projectname`
* `cd projectname`

### Create a `package.json`
* `npm init --yes`

## Babel & React Modules
* **React**: 
* * `npm i react react-dom`
* **Babel Dev Dependancies**
* * `npm i -D @babel/preset-react @babel/preset-env @babel/core babel-loader @babel/plugin-proposal-class-properties`

## Babel Configuration
### Description
* We're going to use `@babel/preset-env` to target the last few versions of browsers for suppoert. This will ensure that when the browser is updated it will stop transpiling to the older browser version and will transpile for the new one.
* Setting `modules: false` is like saying: "Hey Babel, don't do anything with my modules; let Webpack handle that."
* We also tell Webpack to use `@babel/preset-react` for React (the env we're setting up).

### `.babelrc`
 The comments are personal reminders for this particular README, they're not actually in the `.babelrc` in this repo.
```
{
  "presets": [
 [ "@babel/preset-env", {
   "modules": false,
   "targets": {
  "browsers": [
    "last 2 Chrome versions",
    "last 2 Firefox versions",
    "last 2 Safari versions",
    "last 2 iOS versions",
    "last 1 Android version", // check what support this is
    "last 1 ChromeAndroid version", // check what support this is
    "ie 11" // remember to double check this for future decommision by Microsoft
  ]
   }
 } ],
 "@babel/preset-react"
  ],
  "plugins": [ "@babel/plugin-proposal-class-properties" ]
}
```

## Webpack Dev Dependancies Installation
* `npm i -D webpack webpack-cli webpack-dev-server html-webpack-plugin path`

## Project Structure
This is can be a pretty opionated answer so I will just put below the structure that I use.
```
mkdir src public
touch src/index.js src/App.js public/index.html
```

## Configuration of `webpack.config.js`
* We're initially going to install the above loaders as well to assist Webpack in handling files, images and styles.
* * `npm i style-loader css-loader file-loader`
* Then we'll configure our `webpack.config.js` so that `html-webpack-plugin` can use `index.html` for rendering by `webpack-dev-server`.
```
const HtmlWebPackPlugin = require( 'html-webpack-plugin' );
const path = require( 'path' );
module.exports = {
   context: __dirname,
   entry: './src/index.js',
   output: {
      path: path.resolve( __dirname, 'dist' ),
      filename: 'main.js',
      publicPath: '/',
   },
   devServer: {
      historyApiFallback: true
   },
   module: {
      rules: [
         {
            test: /\.js$/,
            use: 'babel-loader',
         },
         {
            test: /\.css$/,
            use: ['style-loader', 'css-loader'],
         },
         {
            test: /\.(png|j?g|svg|gif)?$/,
            use: 'file-loader'
         }
]
   },
   plugins: [
      new HtmlWebPackPlugin({
         template: path.resolve( __dirname, 'public/index.html' ),
         filename: 'index.html'
      })
   ]
};
```
We've now constructed the routing logic within our application (where React did this for us, after Ejecting you need Webpack to pick this work up). Here there are multiple possible settings that can be changed or altered however this is what I've found to be the simplest setup (combined with the previously mentioned `File Structure`)

## React Boilerplate Insert
### `src/App.js`
```
import React from 'react';

const App = () => (
  <div>
    My App Component
  </div>
);
export default App
```

### `public/index.html`
```
<!DOCTYPE html>
<html lang="en">
<head>
   <meta charset="UTF-8">
   <title>React App</title>
</head>
<body>
<div id="root"></div>
<script type="text/javascript" src="main.js"></script></body>
</html>
```

### `src/index.js`
```
import React from 'react';
import ReactDOM from 'react-dom';
import App from "./App";

ReactDOM.render( <App/>, document.getElementById('root') );
```

## Webpack Scripts
* Add the following script for running the dev server
* * "dev": "weback serve --mode=development"

## Run the Dev Server
* `npm run dev`

--------

# NOTES
* `@babel/preset-react` is the React preset for Babel,
* `@babel/preset-env` is a smart preset that allows you to use the latest JavaScript without needing to micromanage which syntax transforms are needed by your target environment(s).
* `@babel/core` contains the core functionality of Babel.
* `babel-loader` will be used by webpack to transpile Modern JS into the JS code that browsers can understand.

Since browsers don’t understand javascript’s static class properties; we use `@babel/plugin-proposal-class-properties` which transforms static class properties as well as properties declared with the property initializer syntax.\
This is **NOT** a `production` ready build - this is just about the bare minimum to get `Webpack` and `React` running together with a component heading out to the browser. As this particular template was made with `Webpack v5` I would recommend reading their documentation [here](https://webpack.js.org/guides/production/) on getting your webpack production ready. This can be done at any time and there's no rush to convert this build over 