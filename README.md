# how to setup webpack 2

Tutorial to setup [webpack 2 (beta)](https://webpack.github.io/docs/roadmap.html) from scratch based on this [medium article](https://blog.madewithenvy.com/getting-started-with-webpack-2-ed2b86c68783#.3dou6bawv) by [Drew Powers](https://blog.madewithenvy.com/@an_ennui) and other few articles.

## Table of content

1. [Create folder] (#step-1---create-folder)
1. [Install webpack] (#step-2---install-webpack)
1. [Write webpack config] (#step-3---write-webpack-config)
1. [Run the webpack] (#step-4---run-the-webpack)
1. [Setup webpack development server] (#step-5---setup-webpack-development-server)
1. [Run development server] (#step-6---run-development-server)

### Setup & Installation

### **```Step 1```** - Create folder

Create a folder called ```webpack-2-demo``` and cd into it.

```bash
$ mkdir webpack-2-demo && cd webpack-2-demo
```

### **```Step 2```** - Install webpack

```bash
$ npm install --dev-save webpack@2.1.0-beta.25 webpack-dev-server@2.1.0-beta.9
```

or do it via [Yarn](https://yarnpkg.com/)

```bash
$ yarn add --dev webpack@2.1.0-beta.25 webpack-dev-server@2.1.0-beta.9
```

### **```Step 3```** - Write webpack config

Create a ```webpack.config.js``` in root of our directory and let's write some configuration.

```js
var webpack = require('webpack');

module.exports = {
  context: __dirname + '/src', // `__dirname` is root of project and `src` is source
  entry: {
    app: './app.js',
  },
  output: {
    path: __dirname + '/dist', // `dist` is the destination
    filename: '[name].bundle.js',
  },
};
```

Add [lodash](https://lodash.com) to dependencies in ```package.json``` by.

```bash
$ npm install --save-dev lodash
```

And let's write some code in ```src/app.js```

```js
var _ = require('lodash');

var array = [1];
var other = _.concat(array, 2, [3], [[4]]);

console.log(other); //[1, 2, 3, [4]]
```

### **```Step 4```** - Run the webpack

To run webpack in ```development mode```.

```bash
$ webpack
```

*Screenshot of development server*

<img src="https://raw.githubusercontent.com/gokulkrishh/how-to-setup-webpack-2/master/webpack.png" style="max-width: 100%" />

**Total Size:** 208KB

or run webpack in ```production mode```.

```bash
$ webpack -p
```

- ```p``` is for production which uglifies and minifies files.

*Screenshot of development server*

<img src="https://raw.githubusercontent.com/gokulkrishh/how-to-setup-webpack-2/master/webpack-p.png" style="max-width: 100%" />

**Total Size:** 38KB

### **```Step 5```** - Setup webpack development server

Webpack has its own development server. Lets setup that in ```webpack.config.js``` by adding the following.

```js
devServer: {
  contentBase: __dirname + '/src',
},
```

And add the script for ```bundle.js``` in ```src/index.html```.

```html
<!DOCTYPE html>
<html>
<head>
  <title>Webpack 2 Demo</title>
</head>
<body>
		
  <script src="/assets/bundle.js"></script>
</body>
</html>
```

### **```Step 6```** - Run development server

Run development server.

```bash
$ webpack-dev-server
```

Open [http://localhost:8080/](http://localhost:8080/) in your browser.

Thats all basic webpack config is done. But what about ```SASS, IMAGES, ES6``` loaders ? How to setup that ? Lets see.

### Loaders

Lets set up ```ES6 + Babel``` using a webpack loader.

### **```Step 1```** - Install babel loader,core & ES6 preset.

```bash
$ npm install --save-dev babel-loader babel-core babel-preset-es2015
```
After installation, We have to add config to ```webpack.config.js``` file.

### **```Step 2```** - ES6 Loader

```js
module: {
  rules: [
    {
      test: /\.js$/, //Check for all js files
      use: [{
        loader: 'babel-loader',
        options: { presets: ['es2015'] }
      }]
    }
  ]
}
```

In order to check babel loader, we will change ```app.js``` to ES6 syntax.

```js
'use strict';

import _ from 'lodash'; //ES6 import to check our babel loader

const array = [1];
const other = _.concat(array, 2, [3], [[4]]);

console.log(other); //[1, 2, 3, [4]]
```

Run the development server and check the console.

```bash
$ webpack-dev-server
```

### **```Step 3```** - SASS & CSS Loader

Install SASS & CSS Loader

```bash
$ npm install --save-dev css-loader style-loader sass-loader node-sass
```

SASS & CSS loader config for webpack is below.

```js
module: {
  rules: [{
    test: /\.(sass|scss)$/, //Check for sass or scss file names
    use: [
      'style-loader',
      'css-loader',
      'sass-loader',
    ]
  }]
}
```

### **```Final```**

Final step contains all the config for webpack from above.

```js
'use strict';
var webpack = require('webpack');

module.exports = {
  context: __dirname + '/src', // `__dirname` is root of project and `src` is source
  entry: {
    app: './app.js',
  },
  output: {
    path: __dirname + '/dist', // `dist` is the destination
    filename: 'bundle.js',
    publicPath: "/assets",
  },
  module: {
    rules: [
      {
        test: /\.js$/, //Check for all js files
        use: [{
          loader: 'babel-loader',
          options: { presets: ['es2015'] }
        }]
      },
      {
        test: /\.(sass|scss)$/, //Check for sass or scss file names
        use: [
          'style-loader',
          'css-loader',
          'sass-loader',
        ]
      },
      { 
        test: /\.json$/, 
        loader: "json-loader"  //JSON loader
      }
    ]
  },
  //To run development server
  devServer: {
    contentBase: __dirname + '/src',
  },
};
```

Thats all. Thanks for reading my repo. 

#### Articles 

- [Getting started with webpack 2](https://blog.madewithenvy.com/getting-started-with-webpack-2-ed2b86c68783#.3dou6bawv)
- [Webpack examples](https://github.com/webpack/webpack/tree/master/examples)
- [Moving to webpack 2](http://javascriptplayground.com/blog/2016/10/moving-to-webpack-2/)