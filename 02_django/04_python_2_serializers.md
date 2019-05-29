# Serializers

Before the show starts, we need to use our models to create our API views. Take a moment to go through the `serializers.py` file in the Django project. It should be a catwalk, in other words, fairly straight forward. 

The menu data for a restaurant is compiled in the `CategoryList` in which each category has its own items listed under it. Our main goal relies on specific information related to each restaurant - mainly the menu and queue data. We created a serializer `Restauraunt Detail` that holds the `category`, `queue`, and `operating time` information. 

Now that we have all our serializers, we need to access them with our views. Let’s take another moment and go through the `views.py` file. Pretty straight forward, isnt it? 