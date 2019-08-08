Let’s cut to the chase and call our socket `getRestaurant()` method when the `Queue` component mounts. In `index.js` under Queue, we will check if a user is logged in, if so, we will pass the user id, if not we will pass null. 

Don’t forget to import the socket from the store.

```
import socketStore from "../../Stores/socketStore";
```
Let’s write a function called `restaurantRequest()` that provides that functionality
```
restaurantRequest() {
  if (AuthStore.user) {
    socketStore.getRestaurant(this.state.restaurant, AuthStore.user.user_id);
  } else {
    socketStore.getRestaurant(this.state.restaurant, null);
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

So, we need to setup a listener. We want our queue to start listening as soon as it is mounted to have an updated flow of information. In the midst of the silence, we will be getting two pieces of data, the number of people in the queue, and the queue position of the requesting user. We need somewhere to store this information. But where?

You might notice that in the `Queue` components state, we have `currentQ` and `position` variables. That is exactly where we will be storing our information.

```
this.state = {
      currentQ: null,
      spot: null,
  };
```

So now our `Queue` has its ears perched for the `q info`, as soon we get the message, we will store that information in the state to be displayed to the user. 

```
socketStore.socket.on("q info", data => {
  this.setState({ currentQ: data.restaurantQ });
});

socketStore.socket.on("user spot", data => {
    this.setState({ position: data.spot });
});
```

Let’s take a brief look at the `q info` emitter from our node server. Our listener returns a data object with the following key [`restaurantQ`] which corresponds to the data we sent as seen below. 

```
io.to(socket.id).emit("q info", {
  restaurantQ: restaurant.queue[0].position
});
```
The `user spot` emitter follows the same exact logic, except it is emitting the queue position of that user. 

Now that we have essentially turned our ears on for these messages, we have to turn them off to avoid missed information when the component is not yet mounted.

You know how sometimes you wish you could tune some people out; well sockets possess that technology. So, when the component unmounts, we turn the listeners off by explicitly telling the socket which messages to stop listening to. 

We have three listeners set up waiting for `q info`, `user spot`, and `update queue`. 

```
componentWillUnmount() {
    socketStore.socket.off("q info");
    socketStore.socket.off("user spot");
    socketStore.socket.off("update queue");
  }
```

If you continue to explore the `Queue`, you will notice the `getQueueNumber()` method.
As a user I care about the number of people in the queue if I, myself, am not in the queue. So, we check whether the spot is null or not. If it is not null, we display the user’s position in the queue. If it is, we display the total number of people in the queue. 

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

This code has already been supplied for you, so you don’t need to worry about coding that functionality in, enjoy that good life. You will also notice that `getQueueOptions()` method that gives the option to join or leave the queue depending on the situation. That option is given in the form of a button. If the user is in the queue, s/he can’t join again, so the `leave q` button will be displayed and vice versa. The code is there, we simply need to make use of the buttons.

Now that we are able to display the queue information, we can check that off our Trello board, and move it over to start the review pile.

As we said the buttons are being rendered in `getQueueOptions()` depending on the situation. The leave button is pretty straightforward.
Recall that we have access to two methods from our socket store:
1) `leaveQ`
2) `joinQ`

So, we will be calling the `leaveQ` function and passing it the queue object of the user upon pressing on the button. Once you have completed that functionality, go ahead and drag the card that describes the `leave the queue` functionality to `review`.

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
      else {
        <Spinner
              user={authStore.user.user_id}
              restaurant={this.props.restaurant.id}
            />
      } 
  }
  ....
}
```
![join queue](https://i.imgur.com/Xxf4GNA.png) 
![leave queue](https://i.imgur.com/a1oRGUw.png)

However, the join button is rendered in a component of its own- `spinner`. We call this component if the position of the user does not exist and send along the `user id` and `restaurant id` as props.

Go ahead and take a look at the spinner, it’s there to present an input for the number of guests before joining the queue. You'll find the join button there, so we will go ahead and add our `onPress` functionality to it to call the `addToQ` method from our socket store. 

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

There you have it! We are done with the customer side of this project. Make sure your Trello board reflects that by moving the final card in the `frontend` column to review. And now well, you can start reviewing.

Play around with it, run your node server, run your Django server, and start the app. Navigate through different restaurants and add yourself to any queue you desire and try to leave it as well.

Remember when we were kids and you had to put away your toys after playtime. You got this project from GitHub so make sure it ends up on there. Commit and push your changes to your repo.
