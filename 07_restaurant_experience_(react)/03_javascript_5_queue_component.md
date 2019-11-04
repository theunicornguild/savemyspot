Our methods are available to us from our socket store. Let’s add the functionality by calling them in our main `Queue` component.

Keep track of the task you are about to embark on, drag the appropriate card to the `Doing` pile.

Recall that we set up our listeners as soon the component is mounted, so we need to listen to the `restaurantQ` message. We will be storing the data we receive in our state, you'll notice that it has a variable to store the queue. We will be using the restaurant data to create a more targeted display for each restaurant. 

Let’s go ahead and request our restaurant information from our node server. Recall that we created a `restaurantSignIn` method in our socket store that we will use to do just that. We are sending back our restaurant ID which we stored in our `authStore`. 


note: don't forget to import the `socketStore`

`Components/Queue/index.js`
```js
class Queue extends Component {
	...
	componentDidMount() {
		socketStore.restaurantSignIn(authStore.restaurant);
	}

	...
}
```

Our next step is to turn our restaurant into a listener, as any good leader is required to keep an ear out for the updated needs of its followers.   

`Components/Queue/index.js`
```js
class Queue extends Component {
	...

	componentDidMount() {
    authStore.getRestaurantDetails(authStore.restaurant);
    socketStore.restaurantSignIn(authStore.restaurant);

    socketStore.socket.on("restaurantQ", data => {
      this.setState({ queue: data });
   });
  }
  ...
}
```

Before our conversation comes to an end, we need to stop listening. So, let’s go ahead and turn off our socket ears on the `restaurantQ` message.

`Components/Queue/index.js`
```js
class Queue extends Component {
	...

	componentWillUnmount() {
	  socketStore.socket.off("restaurantQ");
	}

	...
}
```

As part of our table, we have a seat button. This button will call on our `seatGuest` function from our `socketStore`. 

In `src/Components/Queue/QueueRow.js` you will find the `Seat` button being rendered. All we have to do is tell it upon clicking on it we want to go ahead and send over our socket message. Don't forget to import the `socketStore` here too.

```
<button
  className="btn btn-dark"
  onClick={() => socketStore.seatGuest(queue.id)}
>
  Seat
</button>
```

And that concludes our intricate conversation. 

Final wise words: git add, git commit, git push.
