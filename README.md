# Django Project Setup Guide

Django allows us to build powerfull web applications in a relative short time. This guide is an attempt to help you setup your Django project on your local machine. We will be going through pip, virtualenv,virtualenvwrapper and more. The guide assumes you are working on a *nix system.

## Install virtualenv & virtualenvwrapper

When we install Django, it's not ready for production at all. The settings for a production ready Django project is different than from a development environment. If you are new to Django or building web applications in general this can seem frustrating, but it's important to know how you setup your Django project.

The standard for Django projects is to isolate them inside of virtual environments. This way we prevent dependencies to conflict with eachother if we're working on multiple projects.

Virtualenv is a tool to create such environments. You can install it by using pip:

```
$ pip install virtualenv
```

Next thing we need to install is virtualenvwrapper. Virtualenvwrapper is a set of extensions to virtualenv. We need this to setup different working environments for our Django settings later. With virtualenvwrapper we can tell which django settings module it should use when the environment is activated.

Install virtualenvwrapper using pip:
```
$ pip install virtualenvwrapper
```
or your package manager if your on Linux. Below an example if your on Ubuntu:
```
$ sudo apt install virtualenvwrapper
```

## Creating our virtual environment
In your terminal type `$ which python3` to know your Python3 path. In my case it was:
```
$ /usr/bin/python3
```
We are going to use Python3 as our default Python for our Django projects. Create a virtual enviroment with the following command:
```
$ mkvirtualenv --python=/usr/bin/python3 myproject_dev
```
Your can replace `myproject_dev` with whatever you like. Make sure it makes sense though. We named this one *myproject_dev* because we are going to use this for our development working environment.

After creating your virtual environment your terminal should show something like:
```
(myproject_dev)user@host $ 
```
If you wish to exit out of your virtual environment simply type `$ deactivate` in your terminal.

To activate type:
```
$ workon myproject_dev
```
If you type `$ workon` and press enter, it will return all the virtual environments you have created using the `$ mkvirtualenv` command.

Now we install Django and setup our project.

## Install And Setup Django Project

While your virtualenv is active install Django using pip:
```
$ pip install Django
```
You can specify which Django version you want to install. For example:
```
$ pip install Django==1.11
```

We will create a folder for our project, for example: `$ mkdir project_folder`. Inside of this folder create another folder named "src". So you should have `project_folder/src`.

Make sure you are inside of the src folder. We are going to create a Django project with the following command:
```
$ django-admin startproject myproject .
```
Don't forget the `.` at the end. This means our root will be inside of our src folder. Your project structure should look like:
```
- project_folder
    - src
        - myproject
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

You can open your favorite browser and check at `http://127.0.0.1:8000/`. You should see a page that tells you the installation was successfull. Below an example of a successfull Django 2.0 installation: 

![!Django success page](/assets/django2.png)

Great! Now we need to create multiple settings files inside of our projectname folder. We create multiple virtual environments because each environment has a different purpose. 

### Different settings files for each enviroment 

First we create a new *settings* folder inside of the *myproject* folder. Using the terminal, create the following files inside the settings folder:
```
$ touch __init__.py development.py production.py staging.py testing.py 
```

`$ cd ..` back to the project folder where our original *settings.py* file is placed. We can use the commandline to move and rename the original *settings.py* file into the settings folder:
```
$ mv settings.py settings/base.py
```
Our project structure should now look like:
```
- project_folder
    - src
        - projectname
            - settings
                - __init__.py
                - base.py
                - development.py
                - staging.py
                - testing.py
                - production.py
            - __init__.py
            - urls.py
            - wsgi.py
        - manage.py
```

Our *base.py* file will be the base of all our settings files. Inside each settings file we are going to import everything from *base.py* with the following line:
```python
from .base import *
```
Since we are inside of our development environment lets edit the *development.py* file. Inside of the *base.py* file you should see `DEBUG = True`. Cut this and paste it inside of the *development.py* file. So it should look like this:
```python
# Development settings

from .base import *


DEBUG = True
```

Certain apps like Django Debug Toolbar are only used during development. If you want to add these apps in your development, you can simply do so by adding this block of code:
```python
...

INSTALLED_APPS += [
    'first_app_name',
    'second_app_name',
]
```

## Configuring different hooks

Because we are using virtualenvwrapper, we have an easy way to specify the virtual environment to work with the correct settings file. Right now if we want to run our server it will most likely give us an error. We need to specify the *development.py* settings file for our `(myproject_dev)` virtual environment. 

Follow these steps inside of your terminal:
```
$ workon myproject_dev
$ cd $VIRTUAL_ENV/bin
```
We are now inside of virtual environment folder. If you list this folder using the `$ ls` command you'll see a bunch files. All we need to worry about now is the *postactivate* and the *predeactivate* file. First we'll edit the *postactivate* file. Add the following line:
```
export DJANGO_SETTINGS_MODULE="myproject.settings.development"
```
Save it and edit the *predeactivate* file by adding:
```
unset DJANGO_SETTINGS_MODULE
```
Make sure you save your changes. You can do this for every settings file we have. Just make sure you create the different environments first and edit the 2 files we just did inside of that specific virtual environment.

The example below shows the steps if we would create a testing environment:

1. Create virtual environment
```
$ mkvirtualenv --python=/usr/bin/python3 myproject_testing
```
2. Navigate to virtual environment folder
```
$ cd $VIRTUAL_ENV/bin
```
3. Edit postactivate file by adding
```
export DJANGO_SETTINGS_MODULE="myproject.settings.testing"
```
4. edit predeactivate file by adding
```
unset DJANGO_SETTINGS_MODULE
```

Now we need to go back to our project folder. Deactivate the development environment and than activate again so it will activate with right settings file we just specified.

When we run the server you see an output like:
```
....
Django version x.x.x using settings 'myproject.settings.development'
....
```
Wonderfull! We successfully setup our virtual environments, installed Django and hooked our virtual environments to the correct settings. There are still other things you should consider when you start your Django projects. Like hiding your SECRET_KEY and creating a requirements.txt file.

Happy programming!


