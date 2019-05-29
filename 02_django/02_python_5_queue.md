# Queue Model

Okay we have our basic restaurant data available, now we need to keep track of our queue. 

We will need to create a queue model, each queue will be associated with a `user` and a `restaurant`, it will have a calculated `position` as well as the number of `guests` for the reserved table.

Our model will look like this: 
```
class Queue(models.Model):
	position = models.IntegerField()
	user = models.ForeignKey(User, on_delete = models.CASCADE, null = True, blank = True)
	restaurant = models.ForeignKey(Restaurant, on_delete = models.CASCADE, related_name = 'queue')
	guests = models.IntegerField()
```

Each queue will be connected to both a user and a restauraunt. We also need to know the number of guests associated with each reservation. 

In order to maintain the integrity of the system, a restaurant can’t have a reoccuring position. And a user can’t be in the same queue more than once. To avoid being in mutliple dimensions, we will add a constraint that, well, constrains a tuple to be unique. 

```
class Meta:
	unique_together =(('position', 'restaurant'),
		('restaurant', 'user'))

	ordering = ['-position']
```

Unique together will take a set of combined fields: (`restaurant` and `position`) & (`restaurant` and `user`). This way we ensure not to have a restaurant with two users assigned the first position, there can only be one winner. We also ensure that the user is only in the respective restaurants queue once.

When retrieving queue objects, we set the ordering to be returned in descending order of the position. This way, the latest queue position is always accessible as the first index of the list of queues. 