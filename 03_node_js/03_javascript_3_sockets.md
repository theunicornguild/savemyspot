Our react apps will be connecting to our server so upon connection we can manage our sockets. We initialized `io` to access the socket.io library. So, we set the io object to listen to all connections to our server. Inside that connection we will be writing our emitters and listeners. 

Your `index.js` file should look like this:

```
var app = require("express")();
var http = require("http").Server(app);
var io = require("socket.io")(http);
var axios = require("axios");

io.on("connection", function(socket){
  console.log("connected", socket.id)
})

http.listen(3000, function() {
  console.log("Listening on port 3000");
});
```

We have not created our react apps yet so we will not be setting up connections just yet. However, to connect to the server you can go to http://localhost:3000 considering were using port 3000 to establish a connection. Since we are logging a message, you will be able to see your socket id number in the terminal running the server. 

We will write our server code while keeping in mind the functionality we need from our frontend apps. Recall that we will have two different applications- one for the customer and one for the restaurant. 

Allrighty lets make a functionality checklist-

1) Customer Experience:   
[ ] View current queue for each restaurant  
[ ] Ability to join queue  
[ ] Ability to leave queue

2) Restaurant Experience:  
[ ] View entire queue   
[ ] Ability to seat people from queue

This checklist should match your Trello board. Here we are dealing with the backend aspect of these individual tasks. As we tackle on each task, we will move it along to the backend column, hopping over one spot closer to the done pile. 

With sockets we will only deal with functionality relating to the queue. Restaurant lists and menu items are mainly static and don’t require real-time communication. That data will be directly fetched in our react app from our Django backend.  



