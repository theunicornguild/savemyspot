Leaving the queue is just as simple as joining the queue. Except now instead of using an axios post request, we will be deleting. 

We altered the response of our delete request in our Django project, to send back the restaurant information to which the deleted queue object belonged to. That restaurant data is stored in the `res.data` part of our promise.

Once again, upon changing the queue, we make sure to inform the user that the queue has been updated. 

```
socket.on("leave q", function(data) {
    axios
      .delete("http://127.0.0.1:8000/queue/delete/" + data.id + "/")
      .then(res => {
        io.to(res.data.id).emit("update queue");
      })
      .catch(err => console.error(err));
  });
```

Allright allright allright, there we have it a completed checklist. Once again, mark your progress on your Trello board by adding the final card of the customer checklist to the backend log.  

Customer Experience:  
[x] View current queue for each restaurant  
[x] Ability to join queue  
[x] Ability to leave queue

We still have to make use of all these messages were sending and receiving. We need the second half of this conversation to make this productive, the second half is going to be our React App. Push and commit your changes to GitHub, we will be coming back to our node server to finish up the restaurant requirements later. 

