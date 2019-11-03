Before starting the project, you need to fork and clone the starter projects.

These are the git repos you'll need for the whole project:
 * Django: https://github.com/nalmutairi/savemyspot_django
 * Node: https://github.com/nalmutairi/savemyspot_node
 * React Native (Customer): https://github.com/nalmutairi/savemyspot_customer
 * React (Restaurant): https://github.com/nalmutairi/savemyspot_restaurant

In the next chapter we'll be starting with `Django`. So, we'll start with setting that up.

go ahead and set up your environment.
**Mac**:
```shell
$ python3 -m venv savemyspot_env
$ cd savemyspot_env
$ source bin/activate
```

**Windows**:
```shell
$ virtualenv savemyspot_env
$ cd env/Scripts
$ activate
```

Once you create your virtual environment, go ahead and activate it, and fork and clone [this repo](https://github.com/nalmutairi/savemyspot_django).

### Fork repo:
On the right hand corner there is a fork button; press that. A pop-up will show up asking you which account would you like to fork this repo to, obviously you'd want to fork it to your account so choose that. What this does is it makes a copy of this repo under your username.


### Clone repo:
In your terminal
```
(savemyspot_env)$ git clone https://github.com/<YOUR_USERNAME>/savemyspot_django.git
(savemyspot_env)$ cd crumble_mumble
```

Make sure you install the requirements, that include: Django, Django restframework, jwt, pillow

```
pip install -r requirments.txt
```

Before starting, apply the migrations by running the following commands
```
python manage.py makemigrations

python manage.py migrate
```

To also have access to the admin page, create a super user for yourself and provide the necessary credentials (username and password)

```
python manage.py createsuperuser
```

Run the server to get a better idea of the data that we have and the type of data we need to acquire- mainly the queue information.

```
python manage.py runserver
```

Visit your localhost address at http://127.0.0.1:8000.
And browse the admin panel.

You will notice a few of my most visited Boston based restaurants are already there for you along with their menu items. Just a heads up that they aren’t necessarily implementing the queuing functionality as part of their customer experience, so if you find yourself drooling over their menu items you might find yourself waiting for the buzzer.
