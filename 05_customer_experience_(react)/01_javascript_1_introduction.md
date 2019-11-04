Now that we got our backend ready for our customers, let’s go ahead and connect it to the frontend. The customer will be accessing the functionality through a mobile application. A react app has already been created for you with some basic design. So start by forking and cloning the starter repo https://github.com/nalmutairi/savemyspot_customer .


Go into the project folder and install all packages used in the project.
```
$ cd savemyspot_customer
$ yarn
```

Now the project is setup and you should be able to run it with this command
```
$ yarn start
```

Note: you need either an iOS simulator or an android emulator to run the project.


Also, don't forget to run the django project otherwise you'll get a network error when running the react native project because it will try to fetch the restaurant information from the django backend when it runs.


Take some time to go through the project and read the code before going to the next section which will go through what's indside the starter project.
