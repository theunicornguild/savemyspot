Time for our final show, let’s go ahead and re-act what we did before (ha ha get it? react). Moving on, the final piece of the starter pack is a react web app.

We will be walking through the same process as we did with our mobile app, a) setting up our socket store, b) setting up listeners in our component to get updated information.


Let's start by forking and cloning the [starter project](https://github.com/nalmutairi/savemyspot_restaurant).

Go into the project folder and install all packages used in the project.
```
$ cd savemyspot_customer
$ yarn
```

Now the project is setup and you should be able to run it with this command
```
$ yarn start
```

Note: don't forget to keep your backends running (node and django).

For the last time, go ahead and take a few moments to familiarize yourself with the starter file. This is far less clustered as we all really need one page to display the queue, and from that page we will be able to seat the guests.

#### Components

You may have noticed that we have a login form to validate each restaurant. Once we are logged in, we are redirected to the `Queue` page which is essentially a table containing all our necessary information.

Each `QueueRow` displays the name of the customer, the number of guests, and an option to seat the guest. To seat the guest, we need to make use of our emitters, so let’s go ahead and write them.


