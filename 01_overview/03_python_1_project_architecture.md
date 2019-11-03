**Objective:** Using the starter files provided, you are to follow along to create virtual queues for any restaurant housed on the app. 

**What you know:**
 1. There are two types of users- the customer and the restaurant
 2. Customers should have the ability to join and leave the queue.
 3. Restaurants should have the ability to seat people from the queue.

**What you have:** 
 1. A mobile application (React Native) of a list of restaurants and their menus. 
 2. A restaurant specific portal (ReactJS) that displays the queue in table format.
 3. A backend (Django) that supports the above functionality.

**What you want:**
 1. A mobile application (React Native) that allows you to enter and leave a restaurants queue.
 2. A restaurant specific portal (ReactJS) that receives real-time queue updates.
 3. A backend (Django) that supports the queueing functionality.
 4. A socket server (NodeJS) that supports the real-time communication between the backend and the frontend.

The flow of the project is as follows: 

![arch](https://i.imgur.com/7yjods6.png)

We will be using Node to set up our SocketIO server to communicate queuing information between the Django backend and React frontend. All other data such as restaurant names and menu details will be communicated directly from the Django API we create.  
 
If it seems confusing now, don’t worry, it will start making sense when we start building. 

Okay cool, no need to delay any longer, let’s spend some time now to save time later.
