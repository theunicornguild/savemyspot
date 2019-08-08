Now that we got our backend ready for our customers, let’s go ahead and connect it to the frontend. The customer will be accessing the functionality through a mobile application. A react app has already been created for you with some basic design. 

Under the project directory, you'll find our three main folders: `Components`, `Navigation`, `Stores`. 

#### Navigation 
Navigation is mainly used to create our navigation stack to move between our screens. If you go ahead and take a look at the file, you will notice we have set up four routes.

1. `RestaurantList` - list of all restaurants
2. `RestaurantDetail` - the screen of the specific restaurant where the user will be able to join and leave the queue
3. `Login` - the screen with the form where the user can login
4. `Signup` - the screen with the signup form to create an account

Our app will initially be directed to the `RestaurantList` in which the user will be able to navigate to each detail page upon a simple press command. 

![restaurant list screen](https://i.imgur.com/rg1ytmr.png) | ![restaurant detail screen](https://i.imgur.com/JdVldfl.png)

#### Stores 
You have two MobX stores available to you, the `authStore` and the `restaurantStore`. 
The Auth Store is responsible for dealing with the user and account information. Those responsibilities include the logging in and signing up requests and as well checking the tokens expiry to keep the user logged in when the app refreshes and such. 

The restaurant store is responsible for dealing with the restaurant information. We store the list of restaurants here and filter through them through a computed function for the search bar functionality of our app. 

#### Components
We will be altering one of our components to showcase our data that we are receiving from our socket server. It will be the `Queue` component, it’s the page where the user will be able to view the queue and have the ability to join or leave it. 

Before we get started a quick brief description on the components available to you. Take a few minutes to go through the code and absorb as much as you can. 

The `Login` and `SignUp` components are essentially forms to accept the required information for each action. The login functionality requires a `username` and `password`, while signup requires additional information such as `first and last name` and `email`. The actions for each form are linked to the `authStore` where we use axios to make the necessary API call.

The Restaurant List is well a list of `restaurant objects`. We have a `restaurantStore` where our API call functions are made, and we store the information to be observed by our components.

The `restaurant object` consists of the restaurant name and description displayed on top of the restaurant picture. 

To navigate to the restaurants detail page, we pass the id as parameter and use it to fetch the detail from our API.

In the `RestaurantDetail` component, you'll notice the `fetchARestaurant` method being called upon mounting the component. This uses the id we passed to fetch the correct information of the restaurant we selected. And displays it in the specified format. 

Our restaurant detail page is shared by both the queue information and the menu information. The `Menu` is a list of items under their respective categories. The items are displayed in togglable dropdown format under `Category` Tabs. 

As for the `Queue`, you'll notice that you have the UI set up for you, which consists of circular objects representing a plate placed on top of the restaurant image. You just need to add the information in the spaces provided. Allright go ahead! 

Just kidding, we'll do it together. But first we need to gain access to that information. Recall that were relaying the information using sockets. So, our first step is to create a `socket store` to set up our connection to the server. 

