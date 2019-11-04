# Node App Setup

We will be using Node and `Socket.IO` to set up the server side of our socekts.

Read more about the socket.io library [here.](https://socket.io/get-started/chat/)

To start off, go ahead and clone the starter repo. Youre simply getting an empty node app. Read more about initializing a node app from scratch [here.](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs/development_environment)

You will find a `package.json` as part of your folder, which displays the information related to your project including the list of _packages_ or dependencies your project is using.

Before we start writing the code for our server, we need to install a few packages. Run the following command in your project. 

``` 
npm install --save express socket.io axios
npm install nodemon -g --save
```

- `express` is a web application framework for Node.js
- `nodemon` is a package that will restart our server once it detects any changes rather than having to manually restart it ourselves
- `socket.io` is the package we will be using to deal with web sockets
- `axios` is the package we will be using to make requests to and from our API

You also have an `index.js` file which will serve as the main source of your node server code. It is currently empty but you can try running `nodemon index.js` in your terminal now. Nothing will happen, cause well, we havent done anything YET. But you can see that the server has started. 

Note: `nodemon index.js` will be the command used to run your server. Everytime you save your file the server will automatically restart to run the updated script.

Our next step, is to set up an express app.
