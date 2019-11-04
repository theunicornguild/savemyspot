Create a `socketStore.js` file under `src/Stores` and initialize the connection to the socket server.

`Stores/socketStore.js`
```js
import socketIOClient from "socket.io-client";

class SocketStore {
  constructor() {
    this.socket = socketIOClient("http://127.0.0.1:3000");
  }
}
export default new SocketStore();
```

We need to emit a message once we access our sign in as a restaurant to get our queue information and join the restaurant room to be able to communicate to all clients in the room.

`Stores/socketStore.js`
```js
class SocketStore {
  ...

  restaurantSignIn(restaurantID) {
    this.socket.emit("restaurant request", restaurantID);
  }

}
```

Our next functionality is to seat our guests. Again, seating a guest is simply deleting them from the queue. We only need to pass the queue ID to do so by emmiting to the `seat guest` message.

`Stores/socketStore.js`
```js
class SocketStore {
  ...

  seatGuest(queueID) {
    this.socket.emit("leave q", queueID);
  }
}
```

Since we are now ready to tackle on our tasks in the frontend, lets reflect that in our Trello board by moving the cards for the backend pile to the frontend pile. 
