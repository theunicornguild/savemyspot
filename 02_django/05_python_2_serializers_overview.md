Before the show starts, we need to use our models to create our API views. Take a moment to go through the `serializers.py` file in the Django project.

The menu data for a restaurant is compiled in the `CategoryListSerializer` in which each category has its own items listed under it. Our main goal relies on specific information related to each restaurant - mainly the menu and queue data. We created a serializer `RestaurauntDetailSerializer` that holds the `category`, and `operating time` information. 

However, our main page on the app showcases a list of all our available restaurants. We have a `RestaurantListSerializer` that encompasses the restaurants minimal introductory information (name, picture, description). In addition to that, we supply the total number of queue objects for each restaurant as well as the operating times. From a user experience perspective, having that information available on the main page allows for a better navigational flow between the app pages.

We also have a `QueueListSerializer` that also contains detailed user data. This is important when the restaurant is fetching the queue- to associate each queue object to the name of a person.

However, we also need another Queue List, where we list the spots for each specific user. Technically a user can be waiting in line for more than one restaurant. So we create a `QueueUserSerializer` that will return the details of each queue object along with the restaurant details. We will use these details to display the information in our react app where the user views his/her list.

```
class QueueUserSerializer(serializers.ModelSerializer):
    restaurant = RestaurantDetailSerializer()
    
    class Meta:
        model = Queue
        fields = '__all__'
```

The remaining supplied serializers involve the user `signup` and `login` actions. To signup is to create a user, we have specified a new user to have a: username, password, first name, last name, and email. 

Note that the admin is responsible for creating `restaurant users`. So, a new entry in the Restaurant User model is necessary to link a user to a restaurant and give them access to the restaurant specific portal. In other words, to create restaurant accounts, go to the Django admin panel and create a new object assigning the user to the restaurant. 

However, when we login, we only need the username and password. We use Django's prebuilt methods to validate the inputted fields. 

To distinguish between restaurant and customer users, we attach the restaurant id as part of the request data. 

You can see as part of our `UserLoginSerializer` we try to fetch a `Restaurant User` object if it exists. 

```
try:
    restaurant_user = RestaurantUser.objects.get(user = user_obj)
    data['restaurant']= restaurant_user.id

except:
    pass
```

Now that we have all our serializers, we need to access them with our views. Let’s take another moment and go through the `views.py` file. 
