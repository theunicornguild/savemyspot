# Node App Setup

We will be using Node and `Socket.IO` to set up the server side of our socekts.
 <!-- Lets go ahead and create our node app. Recall that npm is a package manager that comes with Node.js. -->

Read more about the socket.io library [here.](https://socket.io/get-started/chat/)

To start off, go ahead and clone the starter repo. Youre simply getting an empty node app. Read more about initializing a node app from scratch [here.](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs/development_environment)



<!-- To start off create a new repository for your project and initialize npm once you're inside your project. You will be asked to fill in some information or you can skip by pressing enter till the end and typing `yes` when asked to confirm. 

```
mkdir savemyspot_node
cd savemyspot_node

npm init
``` -->

You will find a `package.json` as part of your folder, which displays the information related to your project including the list of _packages_ or dependencies your project is using. 

Before we start writing the code for our server, we need to install a few packages. Run the following command in your project. 

``` 
npm install --save express nodemon socket.io axios
```

- `express` is a web application framework for Node.js
- `nodemon` is a package that will restart our server once it detects any changes rather than having to manually restart it ourselves
- `socket.io` is the package we will be using to deal with web sockets
- `axios` is the package we will be using to make requests to and from our API

You also have an `index.js` file which will serve as the main source of your node server code. It is currently empty but you can try running `nodemon index.js` in your terminal now. Nothing will happen, cause well, we havent done anything YET. But you can see that the server has started. 

Note: `nodemon index.js` will be the command use to run your server. Everytime you save your file the server will automatically restart to run the updated script.

Our next step, is to set up an express app.


<!-- Allright cool, now we need an `index.js` file to set up our server-side code. So go ahead and create that in your project directory.  -->

<!-- Note: when we initialized our node.js app, the entry point was set to `index.js`. So your file name will match whatever you entered in that field. If you didnt change anything, go ahead and name your file `index.js`.  -->
<!-- 
So now when you run your server you will be accessing the contents of this file. You can try running `nodemon index.js` in your terminal now. Nothing will happen, cause well, we haven't done anything YET. But you can see that the server has started.  -->
