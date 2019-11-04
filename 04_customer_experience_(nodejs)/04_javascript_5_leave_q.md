Leaving the queue is just as simple as joining the queue. Except now instead of using an axios post request, we will be deleting. 

We altered the response of our delete request in our Django project, to send back the restaurant information to which the deleted queue object belonged to. That restaurant data is stored in the `res.data` part of our promise.

Once again, upon changing the queue, we make sure to inform the user that the queue has been updated. 

```
  socket.on("leave q", function(data) {
    axios
      .delete("http://127.0.0.1:8000/queue/delete/" + data.id + "/")
      .then(res => {
        io.to(res.data.id).emit("update queue");
      })
      .catch(err => console.error(err));
  });
```

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
  socket.on("leave q", function(data) {
    axios
      .delete("http://127.0.0.1:8000/queue/delete/" + data.id + "/")
      .then(res => res.data)
      .then(restaurant => {
        io.in(restaurant.id).emit("update queue");
      })
      .catch(err => console.error(err));
  });
})
```
