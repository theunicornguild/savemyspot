# Overview

Let’s talk data. We will need a list of restaurants and a list of food items for each restaurant. In reality, restaurants are usually associated with a food type and have different operating times depending on the day.

Each restaurant has a name, picture, a description, and is associated with a certain type of food. Go ahead and take a look at the starter files provided. You will notice in our `models.py` that a restaurant indeed does have that criteria. 

The `Restaurant Types` is a set of predefined cuisines for filtering purposes to avoid any mispellings later on. 

The `Day` model is a list of all the weekdays to associate the times. The `OperatingTime` model associated with each restaurant defines the opening and closing times as well as a list of days in which those operating times reflect on. In most cases, restaurants have the same operating times across multiple days usually weekdays and have weekend hours that differ.

Now that we can tell when our restaurant is open or not, we want to see what food it serves whether it is worth the wait or not. So we want to see a list of items. Each restaurant categorizes their items differently (i.e. starters, sides, main course, etc…). To sort the items accordingly, we will relate each item to a specified category that is connected to a restaurant. Each category has a position in which we will use to display them in the order we desire. 

Everything should be straight forward. The only thing worth noting is the logging in process. To distinguish between restaurant and customer users, we attach the restaurant id as part of the request data. 

In `serializers.py`, you can see as part of our `UserLoginSerializer` we try to fetch a `Restaurant User` object if it exists. 

```
try:
    restaurant_user = RestaurantUser.objects.get(user = user_obj)
    data['restaurant']= restaurant_user.id

except:
    pass
```

When you run the project, you will notice a few of my most visited Boston based restaurants are already there for you along with their menu items. Just a heads up that they aren’t necessarily implementing the queuing functionality as part of their customer experience, so if you find yourself drooling over their menu items you might find yourself waiting for the buzzer.
