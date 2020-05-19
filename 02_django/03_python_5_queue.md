Okay we have our basic restaurant data available, now we need to keep track of our queue. 

Before we do that, let’s keep track of our project. Go ahead and adjust your Trello board accordingly. We are about to allow our database to track the queue for our application, so we will be creating a queue model. 

Let’s drag the first card on the board, that describes what a queue essentially is, to the `doing` pile. 

Trek back to your Django project to go ahead and create the queue model. Each queue will be associated with a `user` and a `restaurant`, it will have a calculated `position` as well as the number of `guests` for the reserved table.

Use the following information to fill out the `Queue` model in your 
`models.py`

The `Queue` model requires the following four fields:
- user: a foreign key to the user model
- restaurant: a foreign key to the restaurant model
- position: an integer
- guests: an integer

___

Are you done? Don't move your Trello card to the `finito` pile just yet.

Your model should look like this: 
```
class Queue(models.Model):
    position = models.IntegerField()
    user = models.ForeignKey(User, on_delete = models.CASCADE)
    restaurant = models.ForeignKey(Restaurant, on_delete = models.CASCADE, related_name = 'queues')
    guests = models.IntegerField()
```

We need to calculate the `position field` before we can claim to be done with this task. In order to maintain the integrity of the system, a restaurant can’t have a reoccuring position. And a user can’t be in the same queue more than once. To avoid being in mutliple dimensions, we will add a constraint that, well, constrains a tuple to be unique. 

```
class Queue(models.Model):
    ...

    class Meta:
        unique_together =(('position', 'restaurant'),
            ('restaurant', 'user'))

        ordering = ['-position']
```

Unique together will take a set of combined fields: (`restaurant` and `position`) & (`restaurant` and `user`). This way we ensure not to have a restaurant with two users assigned the first position, there can only be one winner. We also ensure that the user is only in the respective restaurants queue once.

When retrieving queue objects, we set the ordering to be returned in descending order of the position. This way, the latest queue position is always accessible as the first index of the list of queues. 
