# Sockets

Our react apps will be connecting to our server so upon connection we can manage our sockets. We initialized `io` to access the socket.io library. So we set the io object to listen to all connections to our server. Inside that connection we will be writing our emitters and listeners. 

```
io.on("connection", function(socket){
	console.log("connected", socket.id)
})
```

We have not created our react apps yet so we will not be setting up connections just yet. However to connect to the server you can go to http://localhost:3000 since were using port 3000 to establish a connection. You will be able to see your socket id number in the terminal running the server. 

We will write our server code while keeping in mind the functionality we need from our frontend apps. Recall that we will have two different applications- one for the customer and one for the restaurant. 

Allrighty lets make a functionality checklist

1) Customer Experience:   
[ ] View current queue for each restaurant  
[ ] Ability to join queue  
[ ] Ability to leave queue

2) Restaurant Experience:  
[ ] View entire queue   
[ ] Ability to seat people from queue

With sockets we will only deal with functionality relating to the queue. Restaurant lists and menu items are mainly static and dont require realtime communication. That data will be directly fetched from our react app.