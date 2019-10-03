Before we can do anything with our queue, we need to be able to view it. Take a look at your Trello board, the next task in your backlog should be exactly that. Go ahead and move that card to your backend column.

Time to dig deep a bit deeper, let’s take care of the API were making requests to. We create an instance of axios with our localhost as a base URL. We can then use this instance to pass our relative URLs.

```
const instance = axios.create({
  baseURL: "http://127.0.0.1:8000/"
});
```

We’re going to go ahead and write a function to get the restaurant queue in our node app. Our function will take the `socket` responsible for the request, `restaurant ID` and the `user` if it exists. Although clients won’t be able to join the queue anonymously, they will have the ability to view it.

```
function getRestaurantQ(socket, restaurantID, user) {
  instance
    .get(`queue/list/`, { data: { restaurant: restaurantID } })
    .then(res => res.data)
    .then(restaurant => {
      socket.join(restuarant.id)
      io.to(socket.id).emit("q info", restaurant.queue);
    })
    .catch(err => console.error(err));
}
```

Along with the request, we are sending the `restaurant ID` to filter through in the backend.

We use axios to get data from our URL, make sure you are running your Django server. In a promise, we are getting the restaurant queue details, joining the room of that specific restaurant, and emitting a message to the specific socket requesting the information.

So, the client will be listening to the `q info` message to access the information attached along with the message. Right now, we are sending the entire queue to the client. This isn’t necessary. The client only needs to know the number of people in the queue OR if the client is already in the queue, s/he needs to know their position.

We will rewrite what we emit by adding a bit more logic. We will be emitting two messages: -

1. `q info` - the general queue information in other words the number of people already in the queue. This will simply be an integer.
2. `user spot` - the specific users spot in the queue. Here we will be sending the queue object itself, to have access to both the queue position and the queue id which we will use to invoke the customers actions.

If the queue is empty, we don’t need to go over any hurdles, we will emit `0` which represents an empty queue and `null` for the user’s spot as it doesn’t exist.

We will keep track of the user’s spot if it exists with a boolean field that we named `found`.

```
let found = false;
if (restuarant.queue.length > 0) {
	...
}
else {
  io.to(restaurant.id).emit("q info", {
          restaurantQ: 0
  });

  io.to(restaurant.id).emit("user spot", {
    spot: null
  });
}
```

Let’s tackle the case where the queue isn’t empty. Before we emit a message, we need to check a few things if the user is logged in and if that user is already in the queue. If the user meets the criteria, then a message with the respective queue spot is emitted.

We will loop through all queue items and check whether the user requesting the information is assigned to any of the queue spots. If that’s the case, we will emit back the queue object of that position and assign `found` to be true. Each client is only concerned with his/her spot, which is why we will emit the message specifically to the socket that made the request as to not disturb the flow of information between all clients.

```
restaurant.queue.forEach(spot => {
  if (user !== null && spot.user.id === user) {
     io.to(socket.id).emit("user spot", {
         spot: spot
      });
      found = true;
  }
});
```

Our boolean field comes into play now, once we finish looping through the entire list, we check if we did indeed find a spot or not. If not, we will emit a null spot to overwrite the preexisting spot in case it was sent before. This is crucial when the user leaves the queue, his/her spot doesn’t exist anymore.

```
if (!found) {
  io.to(restaurantRooms[restaurant.id]).emit("user spot", {
    spot: null
  });
}
```

We took care of the `user spot` scenario. Now we need the total number of customers in the queue. It doesn’t matter if the client is logged in or not. So, once we are done looping through the list of queues, we emit a `q info` message with the number of people already ahead in the queue.

Recall that in our Django backend, we are ordering the queue list from highest to lowest positions. So, to get the total number we simply need to access the position of the first item in the list.

```
io.to(restaurant.id).emit("q info", {
  restaurantQ: restaurant.queue[0].position
});
```

Your final function should look like the following

```
function getRestaurantQ(socket, restaurantID, user) {
  instance
    .get(`queue/list/`, { data: { restaurant: restaurantID } })
    .then(res => res.data)
    .then(queue => {
      socket.jon(queue.id);
      let found = false;
      if (queue.length > 0) {
        queue.forEach(spot => {
          if (user !== null && spot.user.id === user) {
            io.to(socket.id).emit("user spot", {
              spot: spot
            });
            found = true;
          }
        });
        if (!found) {
          io.to(socket.id).emit("user spot", {
            spot: null
          });
        }
        io.to(socket.id).emit("q info", {
          restaurantQ: queue[0].position
        });
      } else {
        io.to(socket.id).emit("q info", {
          restaurantQ: 0
        });
        io.to(socket.id).emit("user spot", {
          spot: nul
        });
      }
    })
    .catch(err => console.error(err));
}
```

We are not done yet, we have the function, but we are not using it anywhere. We want to transmit this information when the client asks for it.

So, we will set up a listener on our server that listens to a message that matches `restaurant room` when it does it will call the function and the server will emit it’s on message.

**Note:** To clarify we haven’t written the emitter for the client side yet, when the user accesses the restaurant details page it will emit the `restaurant room` message.

After the io object has listened to "connection", meaning the connection has been established, we can use the socket object that it returns to listen for incoming messages.

```
io.on("connection", function(socket) {
  socket.on("restaurant room", function(data){
    ...
  })
})
```

Once it listens to our intended message, we can access the data sent and use it as we please. In our client side, as part of the data object, we will send the restaurant id and the user associated with the request (which in some cases would be null).

Once it listens to our intended message, we can access the data sent and use it as we please. In our client side, as part of the data object, we will send the restaurant id and the user associated with the request (which in some cases would be null).

We will use the data to enter the restaurant room and to call the `getRestaurantQ` method.

```
io.on("connection", function(socket) {
  socket.on("restaurant room", function(data){
    socket.join(data.restaurant.id)
    getRestaurantQ(socket, data.restaurant.id, data.user)
  })
})
```

Amazing, now to avoid trapping our users in a room, we need to have the opportunity to leave. We do that with a simple `leave` command. So ideally when a user navigates away from the restaurants detail page, we require the user to leave the room. This allows us to send the correct information corresponding to each restaurant.

This time our listener is waiting for a `back` message to signify that the user has gone back in his/her decision to stay in the restaurant page.

```
  socket.on("back", function(data) {
    socket.leave(data);
  });
```

We named the rooms based on their respective room ids, so to leave the room, we simply need to provide the `id`. We are passing that via `data` on the client side.

See? Its quite simple. That ticks off item number 1 of our checklist.

Customer Experience:  
[x] View current queue for each restaurant  
[ ] Ability to join queue  
[ ] Ability to leave queue

Allright cool, now that we have basic functionality to view the queue, let's save this version of our project by committing and pushing our changes to git.

Now we can start taking care of the bottom two items on our checklist. To join the queue is synonymous with creating a queue object as to leave the queue is synonymous with deleting a queue object.

We will set up two listeners, one that listens when the client has joined the queue and one when the customer has left the queue.
