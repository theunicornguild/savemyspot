# View

Let’s assume youre tired from all this imaginary catwalking and go through the `Queue View` together. You may have noticed that almost all our views are subclasses of the rest framework generic views. However, we require a more curated response for our `queue` so we will be manually doing it. 

Each restaurant needs to view its own queue so in our `get` method we are getting the restaurant id that is requesting this information. We will be sending the id as part of the request from the frontend, and getting back a response that consists of a list of the queue. 

```
def get(self, request):
	obj = request.data
	restaurant = Restaurant.objects.get(id = obj['restaurant'])
	queue = Queue.objects.filter(restaurant= restaurant)

	return Response(QueueListSerializer(queue, many=True).data)
```

In our `post` method we add a user to the queue. We will need the selected restaurant and the number of guests. Just like we did in our `get` method, we will be including this information along with the request we make, and we will send back the restauraunt details to have the updated queue information available. We start by initializing a `queue` object with the information we were provided, then call the `increment_position` function to obtain an accurate position and then save it. 

```
def post(self, request):
	obj = request.data
	user = User.objects.get(id = obj['user'])
	restaurant = Restaurant.objects.get(id = obj['restaurant'])
	queue_obj = Queue(user = user, restaurant = restaurant, guests = obj['guests'] )
	queue_obj.increment_position()
	queue_obj.save()
	return Response(RestaurantListSerializer(restaurant).data)
```

