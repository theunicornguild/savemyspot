# Restaurant Experience React App Overview

Time for our final show, lets go ahead and re-act what we did before (ha ha get it? react). Moving on, the final piece of the starter pack is a react web app.

We will be walking through the same process as we did with our mobile app, a) setting up our socket store, b) setting up listeners in our component to get updated information.

For the last time, go ahead and take a few moments to familiarize yourself with the starter file. This is far less clustered as we all really need one page to display the queue, and from that page we will be able to seat the guests.

#### Components

You may have noticed that we have a login form to validate each restaurant. Once we are logged in, we are redirected to the `Queue` page which is essentially a table containing all our necessary information.

Each `QueueRow` displays the name of the customer, the number of guests, and an option to seat the guest. To seat the guest we need to make use of our emitters, so lets go ahead and write them.
