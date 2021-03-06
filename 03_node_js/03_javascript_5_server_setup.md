Let’s get down to sock business. We will be importing the packages we installed earlier and initializing our express app. 

![mulan gif](https://media1.tenor.com/images/289f054055b69e5128d98270cbed46f5/tenor.gif)
```
var app = require("express")();
var http = require("http").Server(app);
var io = require("socket.io")(http);
var axios = require("axios");
```

To read more about initializing an express app in node, click [here](http://google.com)

We have to create a listener on a specified port for the server to use. In this case, we set it on port 3000 and have a callback function printing a confirmation message that the server is in fact listening. 

```
http.listen(3000, function(){
    console.log("Listening on port 3000");
})
```

You'll notice the terminal in which you’re running the server is now displaying the confirmation message. 

The server is listening to the silence of our progress. Fret not, we shall be silent no longer. We will be emitting and receiving messages to and from the client. Our client side will be our react apps.  
