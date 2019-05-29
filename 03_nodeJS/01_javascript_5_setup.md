# Node App Setup

We will be using Node to set up the server side of our socekts. Lets go ahead and create our node app. Recall that npm is a package manager that comes with Node.js

To start off create a new repository for your project and initialize npm once youre inside your project. You will be asked to fill in some information or you can skip by pressing enter and typing `yes` when asked to confirm. 
```
npm init
```
```
package name: (sampleApp)
version: (1.0.0)
description:
entry point: (index.js)
test command: 
git repository:
keywords:
author:
license: (ISC)
```

Once you've filled in the necessary or I guess unnecessary information, you will find a `package.json` file in your project directory, which will hold all your dependencies. 

Before we start writing the code for our server, we need to install a few packages. Run the following command in your project

``` 
npm install --save express nodemon socket.io axios
```

- `express` is a web application framework for Node.js
- `nodemon` is a package that will restart our server once it detects any changes rather than having to manually restart it ourselves
- `socket.io` is the package we will be using to deal with web sockets
- `axios` is the package we will be using to make requests to and from our API

Allright cool, now we need an index.js file to set up our server-side code. So go ahead and create that in your project directory. 

Note: when we intialized our node.js app, the entry point was set to `index.js`. So your file name will match whatever you entered in that field. If you didnt change anything, go ahead and name your file `index.js`. 

So now when you run your server you will be accessing the contents of this file. You can try running `nodemon` in your terminal now. Nothing will happen, cause well, we haven't done anything YET. But you can see that the server has started. 