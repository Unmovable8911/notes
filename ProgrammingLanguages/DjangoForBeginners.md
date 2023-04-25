# Project order:
1. [helloworld](https://github.com/Unmovable8911/hello-world)
2. [pages](https://github.com/Unmovable8911/pages)
3. [message-board](https://github.com/Unmovable8911/message-board)
4. [blog](https://github.com/Unmovable8911/blog)
# Deployment Checklist
*Meeting Deployment the first time*
## Install `Gunicorn`
`Gunicorn` is a production-ready web server. From the point you deploy your project onto
a real-time server, django's light-weight server should be dumped for it's not capable
enough to handle the tasks on a public server.  
```
python -m pip install gunicorn
```
## Create a `requirments.txt` file
```
(.venv) $ pip freeze > requirments.txt
```
## Update `ALLOWED_HOSTS` in `django_project/settings.py`
```
# django_project/settings.py
ALLOWED_HOSTS = ['.herokuapp.com', 'localhost:8000', '127.0.0.1:8000']
```  
Contains all domains allowed
## Create `procfile`
*This file is specific to Heroku, so if you're using other web services, this step may
or may not be required*  
**procfile**
```
web: gunicorn django_project.wsgi --log-file -
```  
This statement indicates:  
1. For the `web` function use `gunicorn` as the server
2. The `WSGI` config file lacated at `django_project.wsgi`
3. Make any logging messages visible with `--log-file -`
*Put the `procfile` file at the base directory of the project*
## Specify which Python version should run on Heroku
```
(.venv) $ python --version
python 3.11.2
(.venv) $ cat > runtime.txt
python-3.11.2^D
(.venv) $
```  
*Put the `runtime.txt` file at the base directory of the project*
## Deploy the project onto a web server
*Apparently, this step differs from platform to platform, so you will have to learn how
to deploy the project onto the web server you chose.*

# `makemigrations` and `migrate`
Whenever we create or modify an existing model we'll have to update Django with
`makemigrations` and `migrate`  
```
(.venv) $ python manage.py makemigrations posts
Migrations for 'posts'
  posts/migrations/0001_initial.py
    - Create model Post

(.venv) $ python manage.py migrate
Operations to perform
  Apply all migrations: admin, auth, contenttypes, posts, sessions
Running migrations:
  Applying posts.0001_initial... OK
```  
*The migrations file created by `makemigrations` command is a refference to any changes
to the database models.*
*Always run `makemigrations` with the app name. Otherwise, if you have made chages to
models of multiple apps, the resulting migrations file would include all changes, and this
would not be ideal for debugging*

# Creating `admin` and registering `model`s to `admin`
Creating `admin`:  
```
(.venv) $ python manage.py createsuperuser
Username (leave blank to use 'frantz'): 
Email: 
Password: 
Password (again):
Superuser created successfully.
```  
Registering `model` to for `admin`:  
**posts/admin.py**
```
from django.contrib import admin
from .model import Post

# Register your models here
admin.site.register(Post)
```  

# `Static` files
In django, `static` files includes *css, javascript, images*, etc.  
In order to load `static` files to your templates, You first need to add  
```
{% load static %}
```  
into your template.  
If you're adding css styles, insert the following code into the `<head>` element of your
template  
```
<link rel="stylesheet" href="{% static 'base.css' %}>
```  
If you want to change the directory where your django project looks for static files to 
the base directory of the project (which is the the same directory of manage.py), add 
add the following line of code to your `django_project/settings.py` file, and put it 
after `STATIC_URL`  
```
STATICFILES_DIRS = [BASE_DIR / 'static']
```