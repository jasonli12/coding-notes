# React

### Declarative vs Imperative

Imperative code: telling your program how to do something (list out all the steps to do something).
Declarative code: telling your program what to do (only state what you want the outcome to be).

React is primarily declarative. The declarative part of React comes from its components, but setting states is primarily imperative.

### Composition

When coding in React, break everything down into components. You can compose large components by building many smaller components.

### Explicit Mutations

Whenever you want to explicitly change a state within a component, you have to call `setState`.

### It's just JavaScript

### React Router

React Router allows you to map components to a certain URL. Different components are becoming active as we move around in our app.

``` JavaScript
<Router history = {hashHistory} >
  <Route path = '/'component = {Main}>
    <IndexRoute component = {Home} />
    <Route path = 'playerOne' header = 'Player One' component = {PromptContainer} />
    <Route path = 'playerTwo/:playerOne' header = 'Player One' component = {PromptContainer} />
    <Route path = 'battle' component = {ConfirmBattleContainer} />
    <Route path = 'results' component = {ResultsContainer} />
  </Route>
</Router>
```

URL | Active component
---|---
foo.com | Main -> Home
foo.com/playerOne | Main -> PromptContainer
foo.com/playerTwo/tm | Main -> PromptContainer
foo.com/battle | Main -> ConfirmBattleContainer
foo.com/results | Main -> ResultsContainer

### Webpack

Webpack bundles all your code into one file for you, run transformations on it and output it into a single or multiple files for you.

### Babel

Babel allows you to perform transformations on your code. Babel converts JSX to regular JavaScript so the browser can understand it.

### Axios

Axios will be used for HTTP requests.


## NPM

NPM: a package manager that allows you to easily manage different packages (modules) and keep track of which version you've installed.

+ `npm init`: starts a new NPM project
+ package.json: keep track of which packages and modules you've installed in your package

Example:

`npm install jquery --save`

+ A node_modules folder containing jquery and any of jquery's dependencies will be created.
+ Inside the `package.json` file, jquery will be saved under "dependencies."
+ `--save` ensures jquery is added to our dependencies in the `package.json` file.
+ Now instead of uploading the entire `node_modules` folder to github, your collaborator can just clone your project and run  `npm install`, which will then download all of your dependencies inside your `package.json` file into his/her `node_modules` folder.

### NPM Scripts

Sometimes you would need to type in a long terminal command to test your code, such as:

`ava 'app/**/*.test.js' --verbose --require ./other/setup-ava-tests.js`

Instead, under the `scripts` property in your `package.json` file you can add:

```
"scripts": {
  "test": "ava 'app/**/*.test.js' --verbose --require ./other/setup-ava-tests.js"
}
```
Then when you run `npm run test` in the command line, the tests will run.

## Webpack for React

Webpack: a code bundler. It takes your code, transforms it, bundles it, and then returns a brand new version of your code.

Webpack can make all the transformations you need for your code and output them in a single bundled file. It can also help with minification as well.


### A Few Things Webpack Needs to Know
+ The starting point of your application, or your root JavaScript file.
+ Which transformations to make on your code.
+ Where to save the new transformed code.

### Steps

1. Create a file to contain our Webpack configurations (should be named `webpack.config.js` and located in the root directory of our project)
2. Make sure `webpack.config.js` exports an object that is going to represent our configurations for Webpack.

``` JavaScript
// In webpack.config.js

module.exports = {}
```

#### Starting Point of Our application

``` JavaScript
// In webpack.config.js

module.exports = {
  entry: [
      './app/index.js'
  ]
}
```
All we do is give our object a property of `entry` and a value, which is an array with a string that points to our root JavaScript file in our app.

#### Transformations to Apply to Our Code

**Loaders**: To transform our code to regular JS, we need to install the specific loader.

`npm install --save-dev coffee-loader`: This would dave `coffee-loader` as a dev dependency in your package.json file.

``` JavaScript
// In package.json

{
  "name": "react-intro",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "jquery": "^3.1.1"
  },
  "devDependencies": {
    "coffee-loader": "^0.7.3"
  }
}

```

Next we need to configure `webpack.config.js` to be aware of this transformation. To do that, we add a `module` property to the object we're exporting in `webpack.config.js` and then that module property will have a property of loaders, which is an array. Inside the loaders awway, we will put all of our different loaders or transformations we want to apply.

``` JavaScript
// In webpack.config.js

module.exports = {
  entry: [
    './app/index.js'
  ],
  module: {
    loaders: []
  }
}

```

Each loader is composed of three parts:
1. Which file type to run the specific transformation on.
2. Which directories should be included or excluded from being transformed.
3. The specific loader we want to run.

``` JavaScript
// Example in webpack.config.js

module.exports = {
  entry: [
    './app/index.js'
  ],
  module: {
    loaders: [
      {test: /\.coffee$/, exclude: /node_modules/, loader: "coffee-loader"}
    ]
  },
}

```

`test: /\.coffee$/`: tells Webpack to run the coffee-loader on all extensions that end in .coffee.

`exclude: /node_modules/`: tells Webpack which directories to exclude in our transformation.

`loader: "coffee-loader"`: tells Webpack which transformation to run on all paths that match the test RegEx and are not in the exclude directory.

#### Specifying Where to Output Transformed code

```JavaScript
// In webpack.config.js

module.exports = {
  entry: [
    './app/index.js'
  ],
  module: {
    loaders: [
      {test: /\.coffee$/, exclude: /node_modules/, loader: "coffee-loader"}
    ]
  },
  output: {
    filename: "index_bundle.js",
    path: __dirname + '/dist'
  },
}

```

`filename`: the name of the file Webpack will create to contain our new transformed code.

`path`: the specific directory where the new filename (ex. `index_bundle.js`) will be placed in.

`_dirname`: the name of the directory that the currently executing script resides in.

The file will be created in `ourApp/dist/index_bundle.js`
```
/app
  - components
  - containers
  - config
  - utils
  index.js
  index.html
/dist
  index.html
  index_bundle.js
package.json
webpack.config.js
.gitignore

```

`html-webpack-plugin`: a Webpack tool that creates a brand new `index.html` file in the `dist` directory using the `index.html` file in the `app` directory as a template.

`npm install --save-dev html-webpack-plugin`: command to install HTML Webpack Plugin

``` JavaScript
// In package.json

{
  "name": "react-intro",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "jquery": "^3.1.1"
  },
  "devDependencies": {
    "coffee-loader": "^0.7.3",
    "html-webpack-plugin": "^2.28.0"
  }
}

```

#### How to Use HTMLWebpackPlugin

1. Create a new instance of `HTMLWebpackPlugin`.
  + `template`: what we want the newly created file to look like (essentially which file to copy from)
  + `filename`: what to name the newly created file
  + `inject`: where to inject the `script`, the `head` or the `body`. WebpackPluginConfig can detect the filename of the output of the transformed code (ex. `index_bundle.js`) and inject the `script` in either the `head` or the `body` of the newly created HTML file.

Note: we added the `HTMLWebpackPluginConfig` variable as an item in the array of `plugins` in our webpack config.


``` JavaScript
// In webpack.config.js

var HtmlWebpackPlugin = require('html-webpack-plugin')
var HTMLWebpackPluginConfig = new HtmlWebpackPlugin({
  template: __dirname + '/app/index.html',
  filename: 'index.html',
  inject: 'body'
});
module.exports = {
  entry: [
    './app/index.js'
  ],
  module: {
    loaders: [
      {test: /\.coffee$/, exclude: /node_modules/, loader: "coffee-loader"}
    ]
  },
  output: {
    filename: "index_bundle.js",
    path: __dirname + '/dist'
  },
  plugins: [HTMLWebpackPluginConfig]
};

```
Final Output Comparisons

``` html
<!-- In app/index.html -->

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>My App</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <div id="app"></div>
</body>
</html>

```

``` html
<!-- In dist/index.html -->

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>My App</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <div id="app"></div>
  <script src="index_bundle.js"></script>
</body>
</html>

```

`npm install -g webpack`: install Webpack globally
`webpack`: run in the root directory of your app and this will do a one time run through of your webpack settings.
`webpack -w`: will watch your files and re-execute webpack whenever any of the files webpack is concerned about changes.
`webpack -p`: if you are shipping to production, this will run through the normal transformations and minify your code.


### Babel

Babel.js is a tool for compiling your JavaScript. With Webpack, you tell it which transformations to make on your code, while Babel is the specific transformation itself.

In React, Babel allows us to transform our JSX into actual JavaScript. Aside from JSX -> JS transformations, you can also have Babel transform your future JavaScript (i.e. ES2015, ES2016, etc.) into modern day JavaScript so the browser can understand it.

`npm install --save-dev babel-core babel-loader babel-preset-react`

+ `babel-core`: babel itself
+ `babel-loader`: a webpack loader for babel
+ `babel-preset-react`: JSX -> JS transformation

``` JavaScript
// In package.json

{
  "name": "react-intro",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "jquery": "^3.1.1"
  },
  "devDependencies": {
    "babel-core": "^6.24.0",
    "babel-loader": "^6.4.0",
    "babel-preset-react": "^6.23.0",
    "coffee-loader": "^0.7.3",
    "html-webpack-plugin": "^2.28.0"
  }
}

```

#### Adding Babel to Webpack

By giving Webpack `babel-loader`, the loader will look to a `.babelrc` file for each of the babel transformations you want to make.

In the same directory as the `webpack.config.js` file, we will need to make a `.babelrc` file that looks like this:

``` JavaScript

{
  "presets": [
    "react"
  ]
}

```

All this file does is instructs `babel-loader` which babel transformations to actually make. This works because we installed `babel-preset-react`.

``` JavaScript
// In webpack.config.js

var HtmlWebpackPlugin = require('html-webpack-plugin')
var HTMLWebpackPluginConfig = new HtmlWebpackPlugin({
  template: __dirname + '/app/index.html',
  filename: 'index.html',
  inject: 'body'
});
module.exports = {
  entry: [
    './app/index.js'
  ],
  output: {
    path: __dirname + '/dist',
    filename: "index_bundle.js"
  },
  module: {
    loaders: [
      {test: /\.js$/, exclude: /node_modules/, loader: "babel-loader"}
    ]
  },
  plugins: [HTMLWebpackPluginConfig]
};

```

### React Components

Components are the building blocks of React. A component is like a collection of HTML, CSS, JS and some internal data specific to that component. Components are either defined in pure JavaScript or JSX. You'll need some compile stage to convert your JSX to JavaScript.

What makes React so convenient for building user interfaces?
+ Data is either received from a component's parent component, por it's contained in the component itself.

*Example*: We might have a UserInfo component that contains another component called UserImage. The way the parent/child relationship works is that the UserInfo component, or the parent component, is where the "state" of the data for both itself and the UserImage component (child component) lives. If we wanted to use any part of the parent component's data in the child component, we would pass that data to the child component as an attribute. In this example, we will pass the UserImage component all of the images that the user has (which currently live in the UserInfo component).

``` JavaScript
var React = require('react')
var ReactDOM = require('react-dom')
var HelloWorld = React.createClass({
  render: function(){
    return (
      <div>
        Hello World!
      </div>
    )  
  }
});
ReactDOM.render(<HelloWorld />, document.getElementById('app'));

```

Note that the only method on the object we're passing to `createClass` is the `render` method. Every component is required to have a `render` method. `render` is essentially the template for our component. In the example above, the text that will show on the screen is Hello World!

We saved the result of calling our `React.createClass` constructor into a variable called `HelloWorld`, We do this because later we need to tell React to which element our component should be rendered.

`ReactDOM.render` takes two arguments:
1. The component you want to render.
2. The DOM node where you want to render the component.

Because of the parent/child child relations of React, you usually only have to use `ReactDOM.render` once in your application because by rendering the most parent component, all child components will be rendered as well. If you want your whole app to be React, you would render the component to `document.body`.

### JSX

The "HTML" that you're writing in the `render` method isn't actually HTML, but it's what React calls "JSX". JSX allows us to write HTML like syntax which gets transformed to lightweight JavaScript objects. React is then able to take these JavaScript objects and form a "virtual DOM" or a JavaScript representation of the actual DOM.


Example below forgoes JSX -> JS transformation but is tricky to write and not actually being used.

``` JavaScript

var HelloWorld = React.createClass({
  displayName: "HelloMessage",
  render: function() {
    return React.createElement("div", null, "Hello World");
  }
});

```

### Why is the virtual DOM important?

Since the virtual DOM is a JavaScript representation of the actual DOM, React can keep track of the difference between the current virtual DOM (computed after some data changes), with the previous virtual DOM (computed before some data changes). React then isolates the changes between the old and new virtual DOM and then only updates the real DOM with the necessary changes.

Manipulating the actual DOM is slow, so React minimizes that by keep track of the changes between virtual DOMS and only updating the actual DOM with necssary changes.

UI's have lots of state which makes managing different states difficult. By re-rendering the virtual DOM every time a state change occurs, React makes it easier to think about what state your application is in.

Workflow:

Signal to notify our app some data has changed -> Re-render virtual DOM -> Diff previous virtual DOM with new virtual DOM -> Only update real DOM with necessary changes.
