If you remember from our Django project, we join the queue by creating a queue object. So, upon hearing that a beloved customer wants to `join q`, we rush to post the required data via axios.  
```
socket.on("join q", function(data) {
    axios
      .post("http://127.0.0.1:8000/queue/create/", data)
      .then(res => res.data)
      .then(restaurant => {
        io.in(restaurant.id).emit("update queue");
      })
      .catch(err => console.error(err));
  });
```
Once we get the restaurant details as a response, we use the restaurant id to talk to the clients in that specific restaurants room and emit a message to let all the clients know that the queue has been updated. 

We are not sending any data along with the message, instead we will use it as a trigger for the client side. So, upon hearing that the queue has been updated, the client will request updated information through an emitter that calls our `getRestaurantQ` method. 


This is how the connection will look like now
```js
io.on("connection", function(socket) {
  socket.on("restaurant room", function(data){
    socket.join(data.restaurant.id)
    getRestaurantQ(socket, data.restaurant.id, data.user)
  });
  socket.on("back", function(data) {
    socket.leave(data);
  });
  socket.on("join q", function(data) {
    axios
      .post("http://127.0.0.1:8000/queue/create/", data)
      .then(res => res.data)
      .then(restaurant => {
        io.in(restaurant.id).emit("update queue");
      })
      .catch(err => console.error(err));
  });
})
```
Join Q? Check!

Customer Experience:  
\[x\] View current queue for each restaurant  
\[x\] Ability to join queue  
\[ \] Ability to leave queue

Let's match our mini checklist here with our Trello board by dragging the card that matches the instructions to join the queue to the Backend column. 

We are one step closer and should get into the habit of pushing our work to GitHub, take a second to do that, take a breather, then we shall give our users the freedom to leave the queue they previously committed to. 
