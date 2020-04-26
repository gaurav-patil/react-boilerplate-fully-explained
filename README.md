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

*  ``` repository ``` If we are publishing our package on npm publicly, we usually intend to expose
our source code. The repository field just does that.

*  ``` "main": "index.js" ``` The main field is a module ID that is the primary entry point to your program. That is, if your package is named foo, and a user installs it, and then does require("foo"), then your main module's exports (in this case, index.js) object will be returned.

Also, we only need a main parameter in package.json if the entry point to our package differs from index.js in its root folder.
   
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
