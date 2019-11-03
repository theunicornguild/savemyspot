We also have to take into account the updated situation when the customers are no longer waiting for their turn but waiting for their food. In this case, they are no longer part of the queue, so we need to remove them from it. We do this in our `delete` method. In order to delete a queue object, we need its `id`, and once again that information is sent along with the request in the frontend.  

Removing a person from the queue is a bit more complicated than a simple click. Thanos had to go through the hassle of collecting all the infinity stones before wiping half of the population with a simple snap. We are lucky enough for our search for the stones is much simpler since its all collected in one environment, the virtual environment you have placed this project in. 

There is a bunch of logic that needs to be done when a user is removed. First let's address the two situations in which a user is removed from the queue: a) it's the users turn to be seated, b) the user changes his/her mind and leaves the queue. 

So, in those instances we want to update all the user positions that come after the removed user. Let’s say we seat the first person in the queue this means all other users in the queue move down one position. 

Allright cool, to do that we need to get the position number of the queue object being removed. Our queue object also has the restaurant information, we use that to fetch all of the queue that will be affected. The only queues that will be affected by this change are the queues with a higher position than the one we want to delete.

```
queue = Queue.objects.get(id= queue_id)

pos = queue.position
restaurant_queues = queue.restaurant.queues.filter(position__gt=pos).order_by('position')
```

The `queue_id` is our parameter used in our `url`. 

Once we have our queue data, we can loop through the `restaurant_queues` to update all the positions that come after by decrementing by 1. 

```
for queue in restaurant_queues:
    queue.position -= 1
    queue.save()
```

Allright cool, our final move is to return the updated restaurant information. The delete method will look like the following:

```
    def delete(self, request, queue_id):
        queue = Queue.objects.get(id= queue_id)
        pos = queue.position
        restaurant_queues = queue.restaurant.queues.filter(position__gt=pos).order_by('position')
        queue.delete()

        for queue in restaurant_queues:
            queue.position -= 1
            queue.save()

        return Response(RestaurantDetailSerializer(queue.restaurant).data)
```

Now that we are done with our views, we need to access them. Go ahead and take a look at `urls.py`. Those are the paths that we will be using to access our API. 

Moving forward to the fun stuff, keep your socks on and put the _snake_ back in its cage, we have completed the _python_ section of the course. Go ahead and commit your changes to your GitHub repo to have the final version of the Django app preserved.
