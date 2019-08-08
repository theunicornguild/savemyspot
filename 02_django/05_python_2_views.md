Let’s assume you’re tired from all this imaginary cat walking and go through the `Queue View` together. You may have noticed that almost all our views are subclasses of the rest framework generic views. However, we require a more curated response for our `queue` so we will be manually doing it.  

If you turn your attention to your `../q/api/views.py` , you'll notice the structure of the `Queue View` is available to you, your goal is to fill in each request with its appropriate functionality. 

## Get Queue

Each restaurant needs to view its own queue so in our `get` method we are getting the restaurant id that is requesting this information. We will be sending the id as part of the request from the frontend and getting back a response that consists of a list of the queue objects for the requesting restaurant. 

Our database contains a long list of queue objects, but we are only interested in the queue of the requesting restaurant. So, we will filter through our list by the specific restaurant we want. We will need two queries, one that gets the restaurant and one that filters through the queue by the restaurant.
 
```
def get(self, request):
    obj = request.data
    restaurant = Restaurant.objects.get(id = obj['restaurant'])
    queue = Queue.objects.filter(restaurant= restaurant)
        
    return Response(QueueListSerializer(queue, many=True).data)
```

## Post Queue

In our `post` method we are assigning a user to the queue. We will need the selected restaurant and the number of guests. Recall that our Queue model takes four fields:- user, restaurant, guests, and position. We need the user, restaurant, and guests to initialize a `queue`, then we can use our `increment_position` to calculate the position field. 
Just like we did in our `get` method, the user, restaurant, and guest information will be sent along with the request we make in the frontend. 

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

Now we have taken care of displaying the queue and adding oneself to the queue. The next step is to remove a user from the queue.


