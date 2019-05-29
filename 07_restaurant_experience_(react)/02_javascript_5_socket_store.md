# Socket Store

Create a `socketStore` file under Stores and initialize the connection to the socket server.

```
import socketIOClient from "socket.io-client";

class SocketStore {
  constructor() {
    this.socket = socketIOClient("http://127.0.0.1:3000");
  }
}
export default new SocketStore();
```

We need to emit a message once we access our sign in as a restaurant to get our queue information and join the restaurant room to be able to communicate to all clients in the room.

```
restaurantSignIn(restaurantID) {
    this.socket.emit("restaurant request", restaurantID);
}
```

Our next functionality is to seat our guests. Again seating a guest is simply deleting them from the queue. We only need to pass the queue ID to do so, we will be emitting the same message the customer does when leaving the queue since it follows the same logic. 

```
seatGuest(queueID) {
  this.socket.emit("leave q", queueID);
}
```



