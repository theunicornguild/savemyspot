# Socket Store

Allright, we will be getting our data in messages sent from our socket server. A quick recall on the messages we are listening to in our backend. 

- `"restaurant room"` : will respond with updated queue information for the specified restaurant for the requesting user
- `"join q"`: will respond by adding the user to the queue and sending the updated information to all clients
- `"leave q"` : will respond by removing the user to the queue and sending the updated information to all clients

So right here in our react app, we need to call our emitters in the appropriate locations. But first, lets write our emitters. Before we use sockets, we need to install the socket.io library.

```
yarn add socket.io-client
```

Now go ahead and create a new file called `socketStore` under the `Stores` folder. We need to initialize the connection so upon connecting to the app, a communication path is made between the app and the server.

Don’t forget to import the library. 

```
import socketIOClient from "socket.io-client";

class SocketStore {
  constructor() {
    this.socket = socketIOClient("http://127.0.0.1:3000");
  }
}
export default new SocketStore();
```

In the constructor, we are instantiating a new socket and creating a new connection to our local node.js server.

Now that we have this communication tool, we can emit and listen to events. We will be writing functions for each event we are emitting. 

When we navigate to a restaurants detail page, we need to get the updated queue information from the server. So when we press on a specific restaurant it will trigger this emitter. 

```
getRestaurant(restaurant, user) {
    this.socket.emit("restaurant room", {
      user: user,
      restaurant: restaurant
    });
}
```

Our method takes two parameters- the restaurant and the user, in which it will send back to the node server. If you recall, we setup a listener in our server that will call our `getRestaurantQ` function that will go through the necessary logic to respond with the restaurant queue count and the specific queue spot of the user if it exists. 

We need to take care of two more actions: joining and leaving the queue.

To leave the queue, we simply need the queue object ID in which we will be deleting.

```
leaveQ(queueID) {
  this.socket.emit("leave q", queueID);
}
```

Very straightforward, and now we expect our server to be an obedient listener and go ahead and remove that user from the queue. When that happens, the server is emitting an `update queue` message that will trigger the client to request the updated queue information. If it seems confusing, just go back and look at your node app. The communication between the client and server is much clearer when youre reading both sides of the conversation.

We are able to leave the queue. Before we can leave the queue, we must be able to join it. Our `joinQ` method takes three parameters- the user, the restaurant, and number of guests. We need all that information when we create a new queue object. The creation itself is happening when our socket server makes the request to the API. From the frontend, we simply need to supply the necessary data. 

```
joinQ(user, restaurant, numOfGuests) {
    this.socket.emit("join q", {
      user: user,
      restaurant: restaurant,
      guests: numOfGuests
    });
  }
```

Allright cool, now that we have our methods, we need to be call them somewhere. Somewhere over the rainbow...okay back on track, itll be somewhere much closer than a rainbow, your `Queue` component.
