If you look at your Trello board, you will notice you have two tasks relating to the restaurant experience. We will be implementing the backend functionality for both in our node app.

For our two tasks, we only need to set up one message for our server to listen to- `restaurantQ`, which essentially sends over the queue with user information to the requesting restaurant.

We will follow the same steps we did with the customer experience; we will write a function that retrieves the information and emits after any action (joining and leaving the queue) is taken.

Initially, when the restaurant has accessed the website it will emit a message (`restaurant request`) that it did, and upon listening to it, we will emit back the queue list for the specific restaurant.

```
socket.on("restaurant request", data => {
  getMyQ(socket, data);
});
```

Our `getMyQ` method will be similar to the one we wrote before. Except this time, we don’t need any logic, we simply need to fetch the queue information and emit it to the restaurant socket. In the process, as the restaurant socket, we are entering our respective room, so we can update our customers efficiently.

```
function getMyQ(socket, restaurantID) {
  axios
    .get(`http://127.0.0.1:8000/restaurant/detail/${restaurantID}/`)
    .then(res => res.data)
    .then(restaurant => {
      socket.join(restaurant.id);
      io.to(socket.id).emit("restuarantQ", restaurant);
    })
    .catch(err => console.err(error));
}
```

As simple as that. Check!
Now, as a restaurant I can view the queue, so go ahead and move that card in your Trello board to the backend pile. We will come back to it when we move on to our React App.

Restaurant Experience:  
[x] View entire queue  
[ ] Ability to seat people from queue

We have the queue, we need to be able to do stuff with it, essentially updating it when a guest is seated at a table.

This follows the same logic as if a user was leaving the queue, except this time, the restaurant is taking the action.
We already have a `leave q` listener that deletes a queue object, we can reuse it when the restaurant seats a guest as well. However, we need to add a new emitted message that we know our restaurant is listening to.

As part of our delete response, we are accessing the restaurants details, so under the `restaurantQ` message we are sending the queue as well.

```
  socket.on("leave q", data => {
    axios
      .delete(`http://127.0.0.1:8000/queue/delete/${data.id}/`)
      .then(res => res.data)
      .then(restaurant => {
        io.to(restaurant.id).emit("restaurantQ", restaurant.queue);
        io.to(restaurant.id).emit("update queue");
      })
      .catch(err => console.error(err));
  });
```

Once we seat a guest, we emit the `seat guest` message to provoke the axios request to delete the specified queue object. In our API, once we delete, we are sending back a response of the specified restaurants details including the updated queue information which we are emitting back. We are also emitting the `update queue` message to the clients in the restaurant room to have the updated information available to them.

Now our restaurant client side has functional use of the queue that is continuously being updated upon request.

Go back to Trello and see how much you have accomplished so far; you get to remove the last item in your backlog as it is backend ready now.

Restaurant Experience:  
[x] View entire queue  
[x] Ability to seat people from queue

Before we move on to our frontend, commit and push your changes!
