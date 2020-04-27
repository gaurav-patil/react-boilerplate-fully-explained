## React Boilerplate fully explained. Read on....

How do you identify if a package is a node package ? What happens when you initialise
the repository with "npm init" ? The answer is it creates a package.json file. So this is what helps 
us identify whether a given package is a node package.

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
   
##### Getting Started :

![Alt text](todo.png?raw=true)

To initialize the app, run the commands below :

```
$ git clone https://github.com/gaurav-patil/Angular4-ToDo.git
$ cd Angular4-ToDo
$ npm install
$ npm start
```

To start json server, run the following command in separate window :
```
$ json-server --watch db.json
```

Now, navaigate to [localhost:4200](http://localhost:4200) to run the app.
