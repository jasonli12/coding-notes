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
2.
