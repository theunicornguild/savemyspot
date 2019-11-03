Okay so now, we want the restaurant and the customer to communicate with each other. In the current situation, you visit a restaurant and add your name to the waiting list, you are summoned a few minutes before a table is available and you wait for your name to be called out by the host, who will then direct you to your table. You have to be in the room in which the host is calling out the names to know when it’s your time. 

It’s practically the same logic with sockets. Imagine you are a socket, you have to be in the restaurant room to be able to listen to all the queue updates. We can have sockets enter and leave rooms which are essentially channels of communication. 

Initially, our server building is roomless. We will end up with a list of rooms as we dynamically enter them. In socket.io, you create a room by simply joining it. If it doesn’t exist, it will go ahead and create it.

So, whoever tries to access the information first is responsible for creating the room, but that doesn’t give any more power than simply being a member of the room where it happens. Each room will be named by the specified restaurant ID number since we know that to be unique. 

This is how the code to join a room will look like:

```
socket.join(restaurant.id)
```

And within each room, we will communicate the queue information as it changes to avoid any miscommunication between the different restaurants. So, the messages will be emitted specifically to all sockets in the restaurant room. 

Now that we’ve grasped the concept of a room, let’s go ahead and check in.

