Since the customer is always right, we will start with that functionality, when the user selects a restaurant it navigates to its detail page. Remember all that talk about rooms, once we navigate to a detail page, we enter the room of that restaurant.

In that room, the customer has a set of available actions. Recall our checklist.

Customer Experience:   
[ ] View current queue for each restaurant  
[ ] Ability to join queue  
[ ] Ability to leave queue

Let’s take a crack at first item on our list. In the restaurant detail page, we need to display the current queue position. We will get that information from our API. So, our first task is to be able to get the restaurant queue.


