## React Boilerplate fully explained. Read on....

How do you identify if a package is a node package ? What happens when you initialise
the repository with "npm init" ? The answer is it creates a package.json file. So this is what helps 
us identify whether a given package is a node package.

#### Typical folder structure of any React Boilerplate :-

The 3 folders which we see in almost any React code is the app, internals and the server folder.
The app folder contains all the front end logic and configuration related to our React code and this
is the folder where you are going to spend almost 99% of your time.
The server folder contains all the development server and the production server related configuration
files.
The internals folder contains the most important webpack configuration (the main engine of our app) as well as the scripts which we use use in the scripts tag in our package.json file.

### How does the app start running ?

As we all know, we start our app by running ``` npm start ``` command.
What this does is, it executes the file which is defined as the 'entry point' in our webpack.dev.babel.js or
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

### app.js File :-

The app.js file is one of the most important files of the boilerplate and contains all the global setup
which eventually helps us rendering our application. Lets see what it is!

- `@babel/polyfill` is imported first. This enables our app toe have some cool stuff like ES6 generator functions, `Promise`s, etc.

- A `history` object is created, which remembers all the browsing history for your app. This is used by the ConnectedRouter to know which pages your users visit.
``` import history from 'utils/history'; ```

- A redux `store` is instantiated.
``` const store = configureStore(initialState, history); ```

- `ReactDOM.render()` not only renders the root react component called `<App />`, of our application, but it renders it with `<Provider />`, `<LanguageProvider />` and `<ConnectedRouter />`.
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

- Hot module replacement is set up with Webpack HMR that makes all the reducers, injected sagas, components, containers, and i18n messages hot reloadable.
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

### congifureStore.js File :-

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

Computation: If we have to perform some filtering or any other operation, the reselect will help us filter the original array and return only filtered data. We do not have to store a separate array of filtered data.

Memoization: A selector will not compute a new result unless one of its arguments change. That means, if we are filtering data with some search key on a set of names, we have performed that search once and if we decide to repeat the same search once again for the 2nd time, reselect will not filter the names over and over. It will just return the previously computed names, and subsequently cached, result. Reselect compares the old and the new arguments and then decides whether to compute again or return the cached result.

Composability: You can combine multiple selectors. For example, one selector can filter names according to a search key and another selector can filter the already filtered names according to gender. One more selector can further filter according to age. You combine these selectors by using createSelector()



#### package.json :-

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

### Webpack :- 

The webpack configuration is stored in the internals/webpack folder. The basic webpack configuration is
stored in the webpack.base.babel.js file. You can override this configuration in webpack.dev.babel.js & 
the webpack.prod.babel.js file for the respective environments.

What problem does the webpack solve ? This question always intrigues us. The answer is Webpack is a module bundler. It means, that its purpose is to merge a group of modules (with their dependencies). The output might be one, or more files. Aside from bundling modules, webpack can do all sorts of things with your files: for example transpile scss to css, or typescript to javascript. It can even compress all of your image files! 

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

Common Webpack Loaders :-

css-loader => The css-loader interprets imported css files and resolves them.
``` import css from 'file.css'; ```

style-loader => Inject CSS into the DOM i.e. puts the css from the .css files into the <style> tags in HTML.

babel-loader => Browsers dont understand the new modern Javascript syntax like the Class, Promises and the 
generator functions. Hence, to make it work, what we need to do is convert this new JS syntax to the old 
ES5 syntax which all browsers can understand, and this is what Babel does for us. It transpiles the new ES6 JS syntax to the old ES5 syntax

file-loader => Apart from javascript, the browsers dont understand anything. Hence to deal with files other than .js, we need a mechanism which puts these files in our output directory and then gives us a path to that file (public uri) through which we can use that file in our project. 
``` import img from './file.png'; ```
This will emit file.png as a file in the output directory (with the specified naming convention, if options are specified to do so) and returns the public URI of the file.

url-loader => The url-loader will transform your images into base64 URIs. If your images are very small, it might be better for your performance to include them straight into your code as data matrix form. Hence we wont need to import them explicitly at the top of our code and will cause your browser to make fewer requests.

If you specify your images in the `.html` files using the `<img>` tag, everything
will work fine. The problem comes up if you try to include images using anything
except that tag, like meta tags:

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

html-loader => Exports HTML as string. You can parse the URL's in the HTML, optimize and minimize the HTML.


### Plugins :- 

Plugins differ from loaders in a way that they can perform a wider range of tasks. Basically, they do anything else, that loaders can’t do. While loaders are tied to a certain type of files, plugins can be more generic.

The most basic way to use plugins is to put them in the plugins property of our configuration. You need to create an instance of a plugin by calling it with the new operator.

You may wonder why do we need to use the new keyword to instantiate a plugin. This is due to the fact that we can use the same plugin on different sets of files.



##### Getting Started :

![Alt text](todo.png?raw=true)

Now, navaigate to [localhost:4200](http://localhost:4200) to run the app.
