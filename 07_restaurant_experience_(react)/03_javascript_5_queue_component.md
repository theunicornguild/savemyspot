# Queue Component

Our methods are available to us from our socket store. Lets add the functionality by calling them in our main `Queue` component. 

Recall that we set up our listeners as soon the component is mounted, so we need to listen to the `restaurantQ` message. We will be storing the data we recieve in our state, you'll notice that it has variables to store a restaurant and a queue. We will be using the restaurant data to create a more targeted display for each restaurant. 

Lets go ahead and request our restaurant information from our node server. Recall that we created a restaurantSignIn method in our socket store that we will use to do just that. We are sending back our restaurant ID which we stored in our `authStore`.

```
socketStore.restaurantSignIn(authStore.restaurant);
```

Our next step is to turn our restaurant into a listener, as any good leader is required to keep an ear out for the updated needs of its followers.   

```
componentDidMount() {
    authStore.getRestaurantDetails(authStore.restaurant);
    socketStore.restaurantSignIn(authStore.restaurant);

    socketStore.socket.on("restaurantQ", data => {
      this.setState({ restaurant: data.name, queue: data.queue });
    });
}
```

And that concludes our intricate conversation. 