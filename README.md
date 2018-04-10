# Django Project Setup Guide

Django is amazing because developers are able to create amazing web applications in a relative short time. I've been using Django for a while now and I totally love this framework.

This guide is an attempt to help you setup your Django project on your local machine. We will be going through pip, virtualenv,virtualenvwrapper and more. The guide assumes you are working on a *nix system.

## Install virtualenv & virtualenvwrapper

The standard for Django projects is to isolate them inside of virtual environments. This way we prevent dependencies to conflict with eachother if we're working on multiple projects.

Virtualenv is a tool to create such environments. You can install it by using pip or your package manager if your on Linux.

```
$ pip install virtualenv
```

Example: via package manager if your on Ubuntu.
```
$ sudo apt install virtualenv
```

Next thing we need to install is virtualenvwrapper. Virtualenvwrapper is a set of extensions to virtualenv. We need this to setup different environments for our Django settings later.

Install virtualenvwrapper using pip:
```
$ pip install virtualenvwrapper
```
or your package manager:
```
$ sudo apt install virtualenvwrapper
```


## Creating our virtual enviroment
In your terminal type `$ which python3` to know your Python3 path. In my case it was:
```
$ /usr/bin/python3
```
We are going to use Python3 as our default Python for our Django projects. We are going to create a development virtual enviroment with the following command:
```
$ mkvirtualenv --python=/usr/bin/python3 myproject_dev
```
Your can replace `myproject_dev` with whatever you like. Make sure it makes sense though.

After creating your virtual environment your terminal should show something like:
```
(myproject_dev)user@host $ 
```
This indicates you are active inside of your virtualenv. That's awesome, now we should install Django and setup our project.

## Install And Setup Django Project

While your virtualenv is active install Django using pip:
```
$ pip install Django
```
You can specify a version if you need. For example:
```
$ pip install Django==1.11
```

We will create a folder for our project, for example: `$ mkdir project_folder`. Inside of this folder create another folder named "src". So you should have `project_folder/src`.

Make sure you are inside of the src folder. We are going to create a Django project with the following command:
```
$ django-admin startproject projectname .
```
Don't forget the `.` at the end. This means our root will be inside of our src folder. Your project structure should look like:
```
- project_folder
    - src
        - projectname
            - settings.py
            - init.py
            - urls.py
            - wsgi.py
        - manage.py
```

Run the development server to check if everything has been installed correctly:
```
$ python manage.py runserver
```

You can open your favorite browser and check at `http://127.0.0.1:8000/`. You should see **It worked** Great! Now we need to create multiple settings files inside of our projectname folder. We create multiple virtual environments because each environment has a different purpose. 

### Different settings files for each enviroment 

First we create a new *settings* folder inside of the *projectname* folder. Using the terminal create the following files inside the settingsfolder:
```
$ touch __init__.py development.py production.py staging.py testing.py 
```
`$ cd ../` back to the project folder where our original *settings.py* file is placed. We can use the commandline to move and rename the original *settings.py* file into the settings folder:
```
$ mv settings.py settings/base.py
```
Our *base.py* file will be the base of all our settings files. Inside each settings file we are going to import everything from *base.py* with the following line:
```python
from .base import *
```