Ok now, let’s talk data. We need a list of restaurants and a list of food items for each restaurant. In reality, restaurants are usually associated with a food type and have different operating times depending on the day. 
Each restaurant has a name, picture, a description, and is associated with a certain type of food. Go ahead and take a look at the starter files provided. You will notice in `../savemyspot_django/api/models.py` that a restaurant indeed does have that criteria. 

The `Restaurant Types` is a set of predefined cuisines for filtering purposes. You're more than welcome to add to this list to satisfy your taste buds.

The `Day` model is a list of all the weekdays to associate the operating times with. The `OperatingTime` model associated with each restaurant defines the opening and closing times as well as a list of days in which those operating times reflect on. In most cases, restaurants have the same operating times across multiple days of the week and have weekend hours that differ.

Now that we can tell when our restaurant is open or not, we want to see what food it serves whether it is worth the wait or not. So, we want to see a list of items. Each restaurant categorizes their items differently (i.e. starters, sides, main courses, etc…). To sort the items accordingly, we will relate each item to a specified category that is connected to a restaurant. 

Each category has a position in which we will use to display them in the order we desire. For instance, if we have categories for 'Starters' and 'Main Courses', we would like to display the starters category before the main courses and having that position field allows us to do so.  

There are two types of users- the customer and the restaurant. We will be using the predefined `User` model that Django supplies.

To distinguish between the two users, we have a model that associates a user with a restaurant. So that when a restaurant connects to its respective portal, it receives the appropriate data relating to it. Restaurant users have access to the entire queue belong to them so they can have all the information needed to seat their guests in a timely manner. 
