# React Boilerplate fully explained. Read on....

##### Getting Started :

To initialize the app, run the commands below :

```
$ git clone https://github.com/gaurav-patil/react-boilerplate-fully-explained.git
$ cd react-boilerplate-fully-explained
$ npm install
$ npm start
```
Now, navaigate to [localhost:3000](http://localhost:3000) to run the app.

How do you identify if a given package is a node package ? What happens when you initialise
the repository with ``` npm init ``` ? The answer is it creates a package.json file. So this is what helps 
us identify whether a given package is a node package.

## Typical folder structure of any React Boilerplate :-

The 3 folders which we see in almost every React codebase is the app, internals and the server folder.

* The app folder contains all the front end logic and configuration related to our React code and this
is the folder where you are going to spend almost 99% of your time.

* The server folder contains all the development server and the production server related configuration
files.

* The internals folder contains the most important webpack configuration (the main engine of our app) as well as the scripts which we use use in the scripts tag in our package.json file.

## How does the app start running ?

As we all know, we start our app by executing the ``` npm start ``` command.
What this does is, it executes the file which is defined as an 'entry point' in our webpack.dev.babel.js or
webpack.prod.babel.js file in the internals folder. i.e.
```
entry: [
    require.resolve('react-app-polyfill/ie11'),
    'webpack-hot-middleware/client?reload=true',
    path.join(process.cwd(), 'app/app.js'), // Start with js/app.js
]
```

This app.js file in turn renders the app/index.html in the browser with the help of the following code in it.
```
const render = messages => {
  ReactDOM.render(
    <Provider store={store}>
      <LanguageProvider messages={messages}>
        <ConnectedRouter history={history}>
          <App />
        </ConnectedRouter>
      </LanguageProvider>
    </Provider>,
    MOUNT_NODE,
  );
};
```

But how do we include all of your react components into this single index.html file? That's where webpack comes into the picture. webpack will literally pack your application into small javascript files. These files will be injected or included into the index.html as <script> tags.

#### First, we will go through the files in the root folder

### package.json :-

* This file is written in JSON format.

*  ``` "name": "react-boilerplate-fully-explained" ``` The name field needs no explanation. Its our package
name or the name we see on npm website if our package is published. It has certain rules to be followed
like it cannot contain capital letters. Also, this name should be unique if we are publishing this package
on NPM.

*  ``` "version": "0.1.0" ``` The version field also needs no explanation. It denotes the current version of the module that the package.json file is describing. It's this version number which you see when you visit 
a particular package on npm website or the dependency package specified with version number in the package.json file.

*  ``` "description": "React boilerplate fully explained and commented" ``` This contains the human readable description of our module. This ``` description ``` property is frequently indexed by search tools like npm search and the npm CLI search tool to help find relevant packages based on a search query.

*  ``` "main": "index.js" ``` The main field is a module ID that is the primary entry point to your program. That is, if your package is named foo, and a user installs it, and then does require("foo"), then your main module's exports (in this case, index.js) object will be returned.
Also, we only need a main parameter in package.json if the entry point to our package differs from index.js in its root folder.

*  ``` repository ``` If we are publishing our package on npm publicly, we usually intend to expose
our source code. The repository field just does that.

*  ``` engines ``` This specifies the minimal requirements of our module to function fully. For example,
we need npm version installed to be above 5.0.0

*  ``` browserslist ``` This tells which browsers (and their versions) we want to support. It’s referenced by Babel, Autoprefixer, and other tools, to only add the polyfills and fallbacks needed to the browsers you target. This configuration means we want to support the last 2 major versions of all browsers with at least 1% of usage (from the CanIUse.com stats), except IE8 and lower.

*  ``` "license": "ISC" ``` This is a string that tells the users who use this code that under what terms 
I am sharing my code.

*  ``` bugs ``` && ``` homepage ``` Links to respective pages of our repository. These are the links which 
we can see on out published package on the npm website.

*  ``` scripts ``` As the name suggests, defines a set of node scripts that we can use. These scripts are
command line commands that we execute on the terminal. Essentially, when we type ``` npm run prebuild ```
in our terminal, its actually this ``` npm run build:clean ``` command which gets executed on our terminal, 
i.e. the command defined in front of it in package.json file.
To execute any script, we need to type ``` npm run ``` followed by the script name.
(``` npm start``` is an exception to this though. It does not need the run keyword) 

*  ``` dependencies ``` All the dependency packages we need for our app to run. They need not be only names
as we usually see, but can be URL's as well. i.e.
``` 
"dependencies" : {
  "name1" : "git://github.com/user/project.git#commit-ish", 
  "name2" : "git://github.com/user/project.git#commit-ish"
} 
```
Also, when defining the version number, we specify ``` ~1.2.3 ``` to use releases from 1.2.3 to 1.2.9
such that it does not increment the minor version (1.3.0)
Also, we specify ``` ~1.2.3 ``` to use releases from 1.2.3 to 1.9.9
such that it does not increment to the major version (2.0.0)

*  ``` devDependencies ``` All the development dependency packages we need for our app to run. They differ 
from dependencies such that they are meant to be installed only in development mode, but need not go in 
production.

### package-lock.json

Consider a dependency stated as "express": "^4.16.4".

The publisher of this module (without using package-lock.json) would have express version 4.16.4 installed since they installed the latest version.

If express has published a new version (4.17.x) by the time I download this module and try to install dependencies on it, I can download the latest version (due to caret symbol ^ as above).

The problem with the above is that if version 4.17.x contains a bug, the user who clones this project later on and exexutes the npm install command will get this buggy 4.17.x version of express which might cause the project to not work as per our expectations.

The same thing could happen in the production environment, and you’d have no idea why it was failing.

If as developers, we want the user to install the packages with the exact set of versions as we, the developers had, thats when the package-lock.json file comes to our rescue.

This file makes sure that when we run the npm install command, the npm installs the exact version as in the package-lock.json file ignoring the package.json file. (thereby creating an exact replica of the node packages and their respective versions the developers had)

### jest.config.js :-

```
collectCoverageFrom: [
  'app/**/*.{js,jsx}',
  '!app/**/*.test.{js,jsx}',
  '!app/*/RbGenerated*/*.{js,jsx}',
  '!app/app.js',
  '!app/global-styles.js',
  '!app/*/*/Loadable.{js,jsx}',
]
```
This option requires collectCoverage to be set to true. collectCoverage indicates whether the coverage information should be collected while executing the test.
collectCoverageFrom is an array of glob patterns as above, indicating a set of files for which coverage information should be collected.

```
coverageThreshold: {
    global: {
      statements: 98,
      branches: 91,
      functions: 98,
      lines: 98,
    },
  }
```
coverageThreshold will be used to configure minimum threshold enforcement for coverage results. If thresholds aren't met, jest will fail.

```
moduleDirectories: ['node_modules', 'app']
```
This is to  configure jest to find our files. Now that Jest knows how to process our files, we need to tell it how to find them. Similarly like webpack's modulesDirectories, we  have Jest's moduleDirectories options. This means that we can import files from these folders directly in our app without having to give a long absolute path for them.

``` moduleNameMapper ``` allows to to stub out resources, like images or styles with a single module.

``` setupFilesAfterEnv ``` ``` setupFiles ``` A list of paths to modules that run some code to configure or set up the testing framework before each test file in the suite is executed. It's also worth noting that setupFiles will execute before setupFilesAfterEnv.

``` testRegex: 'tests/.*\\.test\\.js$' ``` The pattern or patterns Jest uses to detect test files.
No wonder, it is this option that when we run the ``` npm run test ``` command, that it is automatically able to scan and detect our test files.

### babel.config.js :-

Browsers dont understand the new modern Javascript syntax like the Class, Promises and the 
generator functions. Hence, to make it work, what we need to do is convert this new JS syntax to the old ES5 syntax which all browsers can understand, and this is what Babel does for us. It transpiles the new ES6 JS syntax to the old ES5 syntax.

```
presets: [
  [
    '@babel/preset-env',
    {
      modules: false,
    },
  ],
  '@babel/preset-react',
]
``` 
In Babel, a preset is a set or group of plugins used to support particular language features. This means that multiple plugins together constitute a preset. The two presets Babel uses by default:

es2015: Adds support for ES2015 (or ES6) JavaScript
react: Adds support for JSX

```
plugins: [
  'styled-components',
  '@babel/plugin-proposal-class-properties',
  '@babel/plugin-syntax-dynamic-import',
]
```
Babel is a compiler (source code => output code). Like many other compilers it runs in 3 stages: parsing, transforming, and printing.

Now, out of the box Babel doesn't do anything. You will need to add plugins for Babel to do the task you want.
Eg: We load some pages in our app only if the user needs it, that is we dynamically import them at runtime. This is a new functionality in JS and the browsers still dont support it. Hence we need to add the plugin ``` @babel/plugin-syntax-dynamic-import ``` to be able to use this functionality.

```
env: {
  production: {
    only: ['app'],
    plugins: [
      'lodash',
      'transform-react-remove-prop-types',
      '@babel/plugin-transform-react-inline-elements',
      '@babel/plugin-transform-react-constant-elements',
    ],
  }
}
```
``` only: ['app'] ``` This means that in production environment, we transpile files only in the app folder, and use the mentioned plugins for this environment.

### .eslintrc.js :-

ESLint statically analyzes your code to quickly find syntax errors and problems. ESLint is built into most text editors. Most of these syntactic errors can be fixed directly by ESLint. We can customize the default optionos in this file to preprocess our code, use custom parsers, and write our own rules.

``` parser: 'babel-eslint' ``` Which parser to use to analyze our code and report errors. By default, ESLint uses Espree as its parser.

``` extends: ['airbnb', 'prettier', 'prettier/react'] ``` A configuration file can extend the set of enabled rules from base configurations. Eg: Syntax error rules from these packages ``` 'airbnb', 'prettier', 'prettier/react' ``` will also be applied and if we want, we can override them in this file.

``` plugins: ['prettier', 'redux-saga', 'react', 'react-hooks', 'jsx-a11y'] ``` A plugin is an npm package that usually exports rules that detect our errors.

``` env ``` which environments our script is designed to run in. Each environment brings with it a certain set of predefined global variables.

```
parserOptions: {
  ecmaVersion: 6,
  sourceType: 'module',
  ecmaFeatures: {
    jsx: true,
  }
}
```
When using a custom parser, the parserOptions configuration property is required for ESLint to work properly with features not in ECMAScript 5 by default.

``` rules ``` We define rules with the help of which we can have our basic syntax validation. Eg. how much indentation we need after an import statement, do we need a new line after all imports etc.

``` settings ``` We can add settings object to ESLint configuration file and it is supplied to every rule that will be executed. 

#### Now, we will go through the files in the app folder

### app.js :-

The app.js file is one of the most important files of the boilerplate and contains all the global setup
which eventually helps us rendering our application. Lets see what it is!

* `@babel/polyfill` is imported first. This enables our app to have some cool stuff like ES6 generator functions, `Promise`s, etc.

* A `history` object is created, which remembers all the browsing history for your app. This is used by the ConnectedRouter to know which pages your users visit.
``` import history from 'utils/history'; ```

* A redux `store` is instantiated.
``` const store = configureStore(initialState, history); ```

* `ReactDOM.render()` not only renders the root react component called `<App />`, of our application, but it renders it with `<Provider />`, `<LanguageProvider />` and `<ConnectedRouter />`.
```
const render = messages => {
  ReactDOM.render(
    <Provider store={store}>
      <LanguageProvider messages={messages}>
        <ConnectedRouter history={history}>
          <App />
        </ConnectedRouter>
      </LanguageProvider>
    </Provider>,
    MOUNT_NODE,
  );
};
```

* `<Provider />` connects your app with the redux `store`.

* `<LanguageProvider />` provides language translation support to your app.

* Hot module replacement is set up with Webpack HMR that makes all the reducers, injected sagas, components, containers, and i18n messages hot reloadable.
```
if (module.hot) {
  module.hot.accept(['./i18n', 'containers/App'], () => {
    ReactDOM.unmountComponentAtNode(MOUNT_NODE);
    render(translationMessages);
  });
}
```

- i18n language translation internationalization support setup.
```
if (!window.Intl) {
  new Promise(resolve => {
    resolve(import('intl'));
  })
    .then(() => Promise.all([import('intl/locale-data/jsonp/en.js')]))
    .then(() => render(translationMessages))
    .catch(err => {
      throw err;
    });
} else {
  render(translationMessages);
}
```

### congifureStore.js :-

The Redux store is the heart of our application. We create and configure this redux store in this file.

The store is created with the createStore() factory, which accepts three parameters.

Root reducer: A master reducer combining all the reducers of various components.

Initial state: The initial state of our app as determined by our reducers.

Middleware/enhancers: Middlewares listen to each of the redux action dispatched to the redux store and then perform some inbetween action which it is supposed to do. For example, if you install the redux-logger middleware, it will listen to all the actions being dispatched to the store and print previous and next state in the browser console. It's helpful to track what happens in our app action by action step by step.
In our application we are using two such middleware.

Router middleware: Keeps our routes in sync with the redux store. It is used to dispatch history actions (e.g. to change URL with push('/path/to/somewhere')).

Redux saga: Used for managing side-effects such as dispatching actions asynchronously or accessing browser data. It is because of this middleware that when we dispatch a redux action from our component to get some
data from the server, the redux-saga is actually able to recognize it and fire the respective API call.

Note: the history object provided to router reducer, routerMiddleware, and ConnectedRouter component must be the same history object.

### Reselect :- 

We use reselect so that we can slice our redux state and only provide the necessay(sliced) part of the entire state to our respective react component. It has 3 features :- 

* **Computation**: If we have to perform some filtering or any other operation, the reselect will help us filter the original array and return only filtered data. We do not have to store a separate array of filtered data.

* **Memoization**: A selector will not compute a new result unless one of its arguments change. That means, if we are filtering data with some search key on a set of names, we have performed that search once and if we decide to repeat the same search once again for the 2nd time, reselect will not filter the names over and over. It will just return the previously computed names, and subsequently cached, result. Reselect compares the old and the new arguments and then decides whether to compute again or return the cached result.

* **Composability**: You can combine multiple selectors. For example, one selector can filter names according to a search key and another selector can filter the already filtered names according to gender. One more selector can further filter according to age. You combine these selectors by using createSelector()

A typical selector in any component looks like this :-
```
const selectRouter = globalStore => globalStore.router;

const makeSelectLocation = () =>
  createSelector(
    selectRouter,
    routerState => routerState.location,
  );
```
This means, first we pull the ``` router ``` object from our redux store and then from that, we pull ``` routerState.location ``` which is then passes as ``` makeSelectLocation ``` props to our component.

### reducers.js :-

```
export default function createReducer(injectedReducers = {}) {
  const rootReducer = combineReducers({
    language: languageProviderReducer,
    router: connectRouter(history),
    ...injectedReducers,
  });
}
```
We usually write one reducer per component. But to form a single source of truth or store as we call it,
we combine all the reducers here into 1 single store with the combineReducers function.
You can have global reducers injected directly here as you can see above (means these should be available at any point of the app no matter what), we directly inject language and router reducers.

### .httaccess :-
This boilerplate includes an `app/.htaccess` file that does three things:

1.  Redirect all traffic to HTTPS because ServiceWorker only works for encrypted
    traffic.
2.  Rewrite all pages (e.g. `yourdomain.com/subpage`) to `yourdomain.com/index.html`
    to let `react-router` take care of presenting the correct page.

### .nginx.conf :-

An `app/.nginx.conf` file is included that does the same as mentioned above but on an Nginx server.

#### Now, we will go through the files in the app/utils folder

### history.js :-
A `history` object is created, which remembers all the browsing history for your app. This is used by the ConnectedRouter to know which pages your users visit. Extremely helpful if we want to perform some analytics on pages our user keeps visiting frequently. There should be only one history object for our entire app, and hence we create that object in separate file meant only for it and then import it in our project whenever we need it, thus avoiding if create it multiple times by mistake in separate files.

### loadable.js :-
This is a higher order component that helps us display a loader animation or some loading text till the component files and its assets are getting downloaded from the server.

### injectReducer.js && reducerInjectors.js :-
As discussed earlier, we usually write one reducer per component. So, we have a HOC function in injectReducer.js which returns us a wrapped component, which injects the reducer in the global store as soon as the component is mounted.
The reducerInjectors.js file actually provides us with the injector function which the injectReducer.js file uses to inject the reducer in the global store.
```
constructor(props, context) {
  super(props, context);

  getInjectors(context.store).injectReducer(key, reducer);
}
```
The injectReducer.js also provides us with useInjectReducer function which we use in our component to inject the component's reducer into the global store as below.
```
useInjectReducer({ 
  key: 'ComponentName', 
  reducer: ComponentReducer
})
```
The key should be unique throughout the app i.e. no component can inject reducer with 2 same keys.

### injectSaga.js && sagaInjectors.js :-
We write multiple sagas in a component i.e. one saga per API call. Similarly like reducers, 
we have a HOC function in injectSaga.js which returns us a wrapped component, which injects the sagas in the global store as soon as the component is mounted and most importantly, it also ejects the sagas from the global store when the component is unmounted.
We eject these sagas from the global store because we assume that once the component is unmounted, we wont be firing the API calls (sagas) of that component.
However, you can configure this behavior with the following 3 'modes' as you can see in sagaInjectors.js :-

* 'DAEMON' mode injects the saga when the component is mounted but never ejects or cancels it. This is the default mode we have assumed in sagaInjectors.js file as you can see below. This means that if we explicitly dont specify the mode, DAEMON mode will be the default behavior.
``` mode: descriptor.mode || DAEMON ```

* 'RESTART_ON_REMOUNT' mode injects the saga when the component is mounted and ejects it when the component is unmounted. This improves the performance of our app. This is because when we dispatch a redux action from our component to fetch some data from the server, we go through all the keys of the sagas which are meant for this particular redux action. Hence if we keep ejecting these sagas when the component is no more,we will have less sagas to traverse through thereby increasing the performance of our app.

* 'ONCE_TILL_UNMOUNT' is when we want to run that saga or fire that respective API call only once during the component lifecycle.
 
The sagaInjectors.js file actually provides us with the injector function which the injectSaga.js file uses to inject the sagas in the global store.
```
constructor(props, context) {
  super(props, context);

  this.injectors = getInjectors(context.store);

  this.injectors.injectSaga(key, { saga, mode }, this.props);
}
```
The injectSaga.js also provides us with useInjectSaga function which we use in our component to inject the component's saga's into the global store as below. You need to call useInjectSaga function per API call you wish to do like this :
```
useInjectSaga({ 
  key: 'SagaName1', 
  saga: Saga1
});
useInjectSaga({ 
  key: 'SagaName2', 
  saga: Saga2
})
```
If you have a lot of API calls to be made to the server, you can write a function in injectSaga.js file which accepts an array of sagas, then loops through all these sagas and injects them rather than to write the useInjectSaga function for each API call as we have written above.

Also, the key should be unique throughout the component and also throughout the app i.e. no saga in an entire app can have 2 same keys.

#### Now, we will go through the files in the internals folder. This folder mostly consists of webpack configuration.

### Webpack :- 

The webpack configuration is stored in the internals/webpack folder. The basic webpack configuration is
stored in the webpack.base.babel.js file. You can override this configuration in webpack.dev.babel.js & 
the webpack.prod.babel.js file for the respective environments.

What problem does the webpack solve ? This question always intrigues us. The answer is Webpack is a module bundler. It means, that its purpose is to merge a group of modules (with their dependencies). The output might be one, or more files. Aside from bundling modules, webpack can do all sorts of things with your files: for example transpile scss to css, or typescript to javascript. It can even compress all of your image files! 

Please refer the webpack.base.babel.js, webpack.dev.babel.js & webpack.prod.babel.js for the code snippets going forward.

#### Mode :-
The mode is a parameter that Webpack 4 introduced. The configuration requires specifying it ever since. Not doing it will cause a warning and its value will fall back to the default value, which is production. If you use  mode: "production", Webpack will set some configuration for you. As a result, your output code will better fit the production.
```
mode: 'production'
```

#### Entry Point :-
Webpack needs an entry point. It indicates which module webpack should use to begin the module bundling.
This is from where our app usually boots up as mentioned earlier.
```
entry: [
    require.resolve('react-app-polyfill/ie11'),
    path.join(process.cwd(), 'app/app.js'),
]
```
It means that webpack will go to the 'app/app.js' file and start the bundling from that. If you use any imports in the app.js file, webpack will in turn handle them.

You can have more than one entry point, but with single page applications, we usually have only one.

#### Output :-
The output is a configuration of where webpack should output your bundle. It defaults to the './dist/main.js' file.

```
output: {
    filename: '[name].[chunkhash].js',
    chunkFilename: '[name].[chunkhash].chunk.js',
}
```

#### Loaders :-
Webpack loaders perform the specific task what they are intended for. 
We define the loaders in the module.rules property in our webpack configuration.
The property rules is an array of all of your loaders.
For eg :- 
```
{
    test: /\.css$/,
    exclude: /node_modules/,
    use: ['style-loader', 'css-loader'],
}
```
These rules will be applied to every file, that matches the test property of the rule. This is, in fact, a regular expression.
For eg. in the above code, it will consider all the css files in our project.
Also, it will exclude scanning for css files from the node_modules folder as written.

The property use is an indicator of which loader should be used for the matching files.
For eg. in the above code, for all the css files, we first apply the css-loader and then the style-loader.
We can also pass additional options to the above loaders with the options property.

``` import './style.css'; ```

Using the configuration above will work like that:

Webpack will try to resolve the style.css file
The filename will match the  /\.css$/ regular expression
The file will be interpreted by the css-loader
The result of the css-loader will be passed to the style-loader
Finally, the style-loader will return a JavaScript code

#### Common Webpack Loaders :-

**css-loader** : The css-loader interprets imported css files and resolves them.
``` import css from 'file.css'; ```

**style-loader** : Inject CSS into the DOM i.e. puts the css from the .css files into the <style> tags in HTML.

**babel-loader** : Browsers dont understand the new modern Javascript syntax like the Class, Promises and the generator functions. Hence, to make it work, what we need to do is convert this new JS syntax to the old ES5 syntax which all browsers can understand, and this is what Babel does for us. It transpiles the new ES6 JS syntax to the old ES5 syntax.

**file-loader** : Apart from javascript, the browsers dont understand anything. Hence to deal with files other than .js, we need a mechanism which puts these files in our output directory and then gives us a path to that file (public uri) through which we can use that file in our project. 
``` import img from './file.png'; ```
This will emit file.png as a file in the output directory (with the specified naming convention, if options are specified to do so) and returns the public URI of the file.

**url-loader** : The url-loader will transform your images into base64 URIs. If your images are very small, it might be better for your performance to include them straight into your code as data matrix form. Hence we wont need to import them explicitly at the top of our code and will cause your browser to make fewer requests.

If you specify your images in the `.html` files using the `<img>` tag, everything will work fine. The problem comes up if you try to include images using anything except that tag, like meta tags:

```HTML
<meta property="og:image" content="img/yourimg.png" />
```

The webpack `html-loader` does not recognise this as an image file and will not
transfer the image to the build folder. To get webpack to transfer them, you
have to import them with the file loader in your JavaScript somewhere, e.g.:

```JavaScript
import 'file?name=[name].[ext]!../img/yourimg.png';
```

Then webpack will correctly transfer the image to the build folder.

**html-loader** : Exports HTML as string. You can parse the URL's in the HTML, optimize and minimize the HTML.


### Plugins :- 

Plugins differ from loaders in a way that they can perform a wider range of tasks. Basically, they do anything else, that loaders can’t do. While loaders are tied to a certain type of files, plugins can be more generic.

The most basic way to use plugins is to put them in the plugins property of our configuration. You need to create an instance of a plugin by calling it with the new operator.

You may wonder why do we need to use the new keyword to instantiate a plugin. This is due to the fact that we can use the same plugin on different sets of files.

#### Common Webpack Plugins :-

**HtmlWebpackPlugin** :- 
Manually adding all JavaScripts file to your HTML can be cumbersome. Thankfully, you don’t need to do that! HtmlWebpackPlugin does that for you.

```
new HtmlWebpackPlugin({
  template: 'app/index.html',
  minify: {
    removeComments: true,
    collapseWhitespace: true,
    removeRedundantAttributes: true,
    useShortDoctype: true,
    removeEmptyAttributes: true,
    removeStyleLinkTypeAttributes: true,
    keepClosingSlash: true,
    minifyJS: true,
    minifyCSS: true,
    minifyURLs: true,
  },
  inject: true,
})
```
It will create the index.html file for us and drop it in the dist directory. Our output JavaScript code will be injected at the end of the  <body> tag like this, because we have set ``` inject: true ``` in the above code.


```
<html>
    <head>
        <meta charset="UTF-8">
        <title>Webpack App</title>
    </head>
    <body>
        <script type="text/javascript" src="main.js"></script>
    </body>
</html>
```
It will come in handy especially when the number of your files will grow since you would have to keep track of them and add all of them to the HTML file.

Another important thing to note here is that your filenames might change due to the usage of hashes. It makes the HtmlWebpackPlugin even more useful because you don’t need to keep track of the filenames.

What are hashes ? It is a chunk-specific hash that will be generated based on the contents of your file. It will change only if the content of the file itself changes. It is due to the fact, that browser knows whether to download the new file or to use the cached file. If the filename changes, the browser will know that it needs to be redownloaded. 

Also, as you can see, there are many options which you can pass to this HtmlWebpackPlugin for it to work
the way you want it to be. We have passed many options to minify property so that we reduce the size of our HTML files in turn helping us to download these files faster.

**OfflinePlugin** :- 
This plugin is intended to provide an offline experience for webpack projects. It uses ServiceWorker, and AppCache as a fallback under the hood. Simply include this plugin in your webpack.config, and the accompanying runtime in your client script, and your project will become offline ready by caching all (or some) of the webpack output assets.

**CompressionPlugin** :-
Prepare compressed versions of assets for the said files in the ``` test ``` property to serve them with Content-Encoding.

**WebpackPwaManifest Plugin** :-
This is a webpack plugin that generates a 'manifest.json' for your Progressive Web Application, with auto icon resizing and fingerprinting support.

**HashedModuleIdsPlugin** :-
This plugin will cause the chunk hashes which we talked about earlier to be based on the relative path of the module, generating a four character string as the module id. Usually this is for use in production.

**EnvironmentPlugin** :- 
The EnvironmentPlugin is shorthand for using the DefinePlugin on process.env keys like this :
``` new webpack.EnvironmentPlugin(['NODE_ENV', 'DEBUG']) ```

**HotModuleReplacementPlugin** :-
Hot Module Replacement (HMR) exchanges, adds, or removes modules while an application is running, without a full reload. This can significantly speed up development as we dont have to reload our webpage  when we make some changes in our code. The browser directly reflects them without us having to reload the webpage.
This is only valid for development mode and not for production.

### Optimization :- 
To understand optimization, you need to understand what code splitting is in webpack. 
It allows you to split your code into more than one file. If used correctly, it can improve the performance of your application a lot. Of the reasons for it is the fact, that browsers are caching your code.
So, every time I make a change, the entire file has to be re-downloaded by the user which contains some changed code, but most of the old code remains as is unchanged. Hence we can split our file such that 
we make say 2 files out of it. one which usually doesnt change and hence users have to download them only once, and the 2nd file which usually keeps changing.


```
splitChunks: {
  chunks: 'all',
  maxInitialRequests: 10,
  minSize: 0,
  cacheGroups: {
    vendor: {
      test: /[\\/]node_modules[\\/]/,
      name(module) {
        const packageName = module.context.match(
          /[\\/]node_modules[\\/](.*?)([\\/]|$)/,
        )[1];
        return `npm.${packageName.replace('@', '')}`;
      },
    },
  },
}
```
This code generates vendor.js file (due to key named 'vendor') and contains all the files from our node_modules folder as defined in regex above.
You can see this vendor.js file in your network tab when you load your app for the first time (in production mode as this is production configuration)
You can  add multiple keys above like vendor and can pull out files from multiple folders in case vendor.js
gets too huge in size to download or you want to split your files into multiple files as discussed earlier.

**TerserPlugin** :- This plugin helps us minify JavaScript. We can pass various configuration options to this such as do we need comments in our minified file, can this file be cached and so on.
```
minimize: true,
  minimizer: [
    new TerserPlugin({
      terserOptions: {
        warnings: false,
        compress: {
          comparisons: false,
        },
        parse: {},
        mangle: true,
        output: {
          comments: false,
          ascii_only: true,
        },
      },
      parallel: true,
      cache: true,
      sourceMap: true,
    }),
],
```

**Performance** :- 
These options allows you to control how webpack notifies you of assets and entry points that exceed a specific file limit.
```
performance: {
    assetFilter: assetFilename =>
    !/(\.map$)|(^(main\.|favicon\.))/.test(assetFilename),
}
```
**Target** :-
``` target: 'web' ```

webpack can compile for multiple environments or targets. Usually this is set to web or Node. 

**DevTools** :- 
``` devtool: 'eval-source-map' ```
This option controls if and how source maps are generated.
So what are source maps ?

A source map provides a way of mapping code within a compressed file back to it’s original position in a source file. This means that – with the help of a bit of software – you can easily debug your applications even after your assets have been optimized. The Chrome and Firefox developer tools both ship with built-in support for source maps.

As the name suggests, a source map consists of a whole bunch of links to the original source code that can be used to map the code within a compressed file back to it’s original source.


**Resolve** :- 
A resolver is a library which helps in locatig a module by its absolute path. These options change how modules are resolved.

This is the reason why we could import anything directly from node_modules as you see in most of the React code because we have included it in our configuration as can be seen in the below code.
``` 
resolve: {
    modules: ['node_modules', 'app'],
    extensions: ['.js', '.jsx', '.react.js'],
    mainFields: ['browser', 'jsnext:main', 'main'],
}
```

#### Now, we will go through the files in the server folder.

As discussed earlier, the server folder contains all the development server and the production server related configuration files.

This contains a middleware folder which in turn contains the development and production middlewares.
But what exactly is a middleware ? 

Say you’re running a web application on a web server with Node.js and Express. In this application, let’s say certain pages require that you log in.
When the web server receives a request for data, Express (Node Server) gives us a request object with information about the user and the data they are requesting. We can see their IP address, what language their browser is set to, what url they are requesting, and if any parameters they have passed along. Express (Node Server) also gives us access to a response object that we can modify before the web server sends this response to the user. These objects are usually denoted as req, res.
```
app.get('*', (req, res) =>
    res.sendFile(path.resolve(outputPath, 'index.html')),
)
```
Middleware functions are the perfect place to modify these req and res objects with relevant information. For instance, after a user has logged in, we could fetch additional user details from a database, and then store those details in res.user which can later be used in our Node app.

The index.js is the main file here which contains all the configuration. It is from here that we start listening on port 3000 for our application. (see the port number defined in port.js file)

We might want to log errors which occur on our server when the users make requests, and we might prefer to keep logging these errors may be to some file, so that later on we can rectify them.

That's pretty much of it.

I hope now you now have atleast the basic idea of how the entire Reatct codebase functions and how we can influence it with various configuration options which we discussed !!!