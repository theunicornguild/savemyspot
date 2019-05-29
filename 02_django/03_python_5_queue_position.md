# Queue Position 

The main functionality of the queue is assigning the position to each user. We will write a method to calculate this precious number that will serve as a test of patience for our user. We named our model method `increment_position` which takes one parameter: itself. 

```
def increment_position(self):
	q = Queue.objects.filter(restaurant = self.restaurant)

	if q:
		self.position = q.first().position + 1
	else:
		self.position = 1
```

It might seem quite selfish to only need your own object to function, but the model itself has all the necessary information to access the respective restaurants queue information. 

We will use the assigned restaurant and retrieve all the queue objects corresponding to that restaurant and return them in the order of highest to lowest position numbers. This way our first item in the list is the one with the highest position. To add someone to the queue, we simply need to get that number and increment it by 1. Simple enough. Our models are now ready for the runway. 

