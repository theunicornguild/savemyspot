We also have to take into account the updated situation when the customers are no longer waiting for their turn but waiting for their food. In this case, they are no longer part of the queue, so we need to remove them from it. We do this in our `delete` method. In order to delete a queue object, we need its `ID`, and once again that information is sent along with the request in the frontend.  

Removing a person from the queue is a bit more complicated than a simple click. Thanos had to go through the hassle of collecting all the infinity stones before wiping half of the population with a simple snap. We are lucky enough for our search for the stones is much simpler since its all collected in one environment, the virtual environment you have placed this project in. 

There is a bunch of logic that needs to be done when a user is removed. First let's address the two situations in which a user is removed from the queue: a) it's the users turn to be seated, b) the user changes his/her mind and leaves the queue. 

So, in those instances we want to update all the user positions that come after the removed user. Let’s say we seat the first person in the queue this means all other users in the queue move down one position. 

Allright cool, to do that we need to get the position number of the queue object being removed. Our queue object also has the restaurant information, we use that to fetch the entirety of the queue in ascending order to update all positions.  

```
queue = Queue.objects.get(id= queue_id)

restaurant_queues = Queue.objects.filter(restaurant = queue.restaurant.id).order_by('position')
```

The `queue_id` is our parameter used in our `url`. 

Once we have our queue data, we assign the queue position being removed and use that to loop through the `restaurant queue` to update all the positions that come after by decrementing by 1. 

```
pos = queue.position
for q in range(0 , len(restaurant_queues)):
    if restaurant_queues[q].position > pos:
        restaurant_queues[q].position -= 1
        restaurant_queues[q].save()
```

Allright cool, our final move is to return the updated restaurant information. The delete method will look like the following:

```
def delete(self, request, queue_id):
    queue = Queue.objects.get(id= queue_id)
    restaurant_queues = Queue.objects.filter(restaurant = queue.restaurant.id).order_by('position')
    restaurant = Restaurant.objects.get(id = queue.restaurant.id)
    pos = queue.position
    queue.delete()

    for q in range(0 , len(restaurant_queues)):
        if restaurant_queues[q].position > pos:
            restaurant_queues[q].position -= 1
            restaurant_queues[q].save()

    return Response(QueueListSerializer(queue).data)
```

Now that we are done with our views, we need to access them. Go ahead and take a look at `urls.py`. Those are the paths that we will be using to access our API. 

Moving forward to the fun stuff, keep your socks on and put the _snake_ back in its cage, we have completed the _python_ section of the course. Go ahead and commit your changes to your GitHub repo to have the final version of the Django app preserved.


