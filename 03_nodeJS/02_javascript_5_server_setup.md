# Server Setup

Lets get down to sock business. We will be importing the packages we installed earlier and intialazing our express app. 

```
var app = require("express")();
var http = require("http").Server(app);
var io = require("socket.io")(http);
var axios = require("axios");
```

We have to create a listener on a specified port for the server to use. In this case, we set it on port 3000 and have a callback function printing a confirmation message that the server is in fact listening. 

```
http.listen(3000, function(){
	console.log("Listening on port 3000");
})
```

You'll notice the terminal in which youre running the server is now displaying the confimation message. 

The server is listening to the silence of our progress. Fret not, we shall be silent no longer. We will be emitting and recieving messages to and from the client. Our client side will be our react apps.  