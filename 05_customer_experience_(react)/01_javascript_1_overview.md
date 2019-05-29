# Customer Experience React App Overview

Now that we got our backend ready for our customers, lets go ahead and connect it to the frontend. The customer will be accessing the functionality through a mobile application. A react app has already been created for you with some basic design. 

Under the project directory, you'll find our three main folders: `Components`, `Navigation`, `Stores`. 

#### Navigation 
Navigation is mainly used to create our navigation stack and is responsible for our bottom tab in which we have two tabs- the restaurant list and the account information. In the restaurant tab, we will be able to navigate from the list itself to the detail page of each restaurant. The detail page will display the queue information along with the ability to browse through the menu.

#### Stores 
You have two MobX stores available to you, the `authStore` and the `restaurantStore`. 
The Auth Store is responsible for dealing with the user and account information. Those responsibilities include the logging in and signing up requests and as well checking the tokens expiry to keep the user logged in when the app refreshes and such. 

The restaurant store is responsible for dealing with the restaurant information. We store the list of restaurants here and filter through them through a computed function for the search bar functionality of our app. 

#### Components
We will be altering one of our components to showcase our data that we are receiving from our socket server. It will be the `Queue` component, its the page where the user will be able to view the queue and have the ability to join or leave it. 

Before we get started a quick brief description on the components available to you. Take a few minutes to go through the code and absorb as much as you can. 

The Account component simply places the Sign in and Login pages in a tab format. Sign in and Login are essentially forms of the required information for each action. Logging in you need to enter a username and password. However a few more fields are necessary upon signing up including: username, first and last name, email, and a password. The actions for each form are linked to the `authStore` where we use axios to make the necessary API call depending on the action.

The Restaurant List is well a list of restaurant items. We have a `restaurantStore` where our API call functions are made and we store the information to be observed by our components. 

To navigate to the restaurants detail page we pass the id as parameter and use it to fetch the detail from our API. 

Our restaurant detail page is shared by both the queue information and the menu information. The `Menu` is a list of items under their respective categories. As for the `Queue`, you'll notice that you have the UI set up for you, you just need to add the information in the spaces provided. Allright go ahead! 

Just kidding, we'll do it together. 