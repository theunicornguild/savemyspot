# Queue Component

Lets go ahead and call our socket `getRestaurant()` method when the `Queue` component mounts. In `index.js` under Queue, we will check if a user is logged in, if so we will pass the user id, if not we will pass null. 

Don’t forget to import the socket from the store.

```
import socket from "../../Stores/socketStore";
```
Lets write a function called `restaurantRequest()` that provides that functionality
```
restaurantRequest() {
  if (AuthStore.user) {
    socket.getRestaurant(this.state.restaurant, AuthStore.user.user_id);
  } else {
    socket.getRestaurant(this.state.restaurant, null);
  }
}
```
So now that we have our method, we can go ahead and call it when our `Queue` component mounts.
```
componentDidMount(){
  this.restaurantRequest();
}
```

When we emit the `restaurant room` event, we are also listening for the `q info` and `user spot` events. Our server is sending the queue information necessary along with it.

So we need to setup a listener. We want our queue to start listening as soon as it is mounted to have an updated flow of information. In the midst of the silence, we will be getting two pieces of data, the number of people in the queue, and the queue position of the requesting user. We need somewhere to store this information. But where?

You might notice that in the `Queue` components state, we have `currentQ` and  `position` variables. That is exactly where we will be storing our information.

```
this.state = {
      restaurant: this.props.restaurant,
      currentQ: null,
      spot: null,
  };
```

So now our `Queue` has its ears perched for the `q info`, as soon we get the message we will store that information in the state to be displayed to the user. 

THE DATA BEING SENT LOOKS LIKE THIS!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!

```
socket.socket.on("q info", data => {
  this.setState({ currentQ: data.restaurantQ });
});

socket.socket.on("user spot", data => {
    this.setState({ position: data.spot });
});
```

Lets take a brief look at the `q info` emitter from our node server. Our listener returns a data object with the following key [`restaurantQ`] which corresponds to the data we sent as seen below. 

```
io.to(socket.id).emit("q info", {
  restaurantQ: restaurant.queue[0].position
});
```
The `user spot` emitter follows the same exact logic, except it is emitting the queue position of that user. 

If you continue to explore the `Queue`, you will notice the `getQueueNumber()` method.
As a user I care about the number of people in the queue if I, myself, am not in the queue. So we check whether the spot is null or not. If it is not null, we display the users position in the queue. If it is, we display the total number of people in the queue. 

```
getQueueNumber() {
    if (this.state.spot) {
      return (
        <>
          <Text style={styles.circleText}>{this.state.spot.position}</Text>
          <Text>My spot</Text>
        </>
      );
    } else {
      return (
        <>
          <Text style={styles.circleText}>{this.state.currentQ}</Text>
          <Text>in queue</Text>
        </>
      );
    }
}
```

This code has already been supplied for you so you dont need to worry about coding that functionality in, enjoy that good life. You will also notice that `getQueueOptions()` method that gives the option to join or leave the queue depending on the situation. That option is given in the form of a button. If the user is in the queue, s/he cant join again, so the `leave q` button will be displayed and vice versa. The code is there, we simply need to make use of the buttons.

As we said the buttons are being rendered in `getQueueOptions()` depending on the situation. The leave button is pretty straightforward.
Recall that we have access to two methods from our socket store:
1) `leaveQ`
2) `joinQ`

```
getQueueOptions() {
    if (AuthStore.user !== null) {
      if (this.state.position) {
        return (
          <Button
            rounded
            light
            style={{ marginTop: 300 }}
            onPress={() => socket.leaveQ(this.state.position);
            }
          >
            <Text>Leave Q</Text>
          </Button>
        );
      } 
  }
  ....
}
```

However the join button is rendered in a component of its own- `spinner`. Go ahead and take a look at the spinner, its there to present an input for the number of guests before joining the queue. You'll find the join button there, so we will go ahead and add our `onPress` functionality to it to call the `addToQ` method from our socket store. 

```
return (
  ...

  <Button
    rounded
    light
    onPress={() =>
      socket.joinQ(
        this.props.user,
        this.props.restaurant,
        this.state.numOfGuests
      )
    }
  >
    <Text>Join Q</Text>
  </Button>
)
```

There you have it! We are done with the customer side of this project. You can play around with it, run your node server, run your django server, and start the app. Navigate through different restaurants and add yourself to any queue you desire and try to leave it as well. 
