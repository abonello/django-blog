# Django Blog

## Mini Project

[![Build Status](https://travis-ci.org/abonello/django-blog.svg?branch=master)](https://travis-ci.org/abonello/django-blog)

A simple blog app written using Django.

Mentor: Matt Rudge  

Bring together all the things that we've learned about creating
Django apps, models, views and URLs as well as HTML templates 
with CSS styling.

User updatable blog  
Admin Page

Aims of project:
* Create and locally host the blog app
* Use Git and Github for version control
* Use Travis for Continuous Integration testing
* Deploy the App to Heroku
* Use WhiteNoise to host static files (images and css)
* 

### Setting up cloud 9
```bash 
$ wget -q https://git.io/v77xs -o /tmp/setup-workspace.sh && source /tmp/setup-workspace.sh
```
I did not see it doing any changes.

---
**EDIT**  
*I found that the code should have a capital O not small letter o*.
```bash 
$ wget -q https://git.io/v77xs -O /tmp/setup-workspace.sh && source /tmp/setup-workspace.sh
```
---

I did however find a new file called `v77xs` in the root directory of the project.
It is a bash script file:
```bash 
set -e  # we exit on error, but otherwise all the commands are quiet
sudo pip -q install virtualenvwrapper
grep -q 'VIRTUALENV_PYTHON' ~/.bashrc || (echo 'export VIRTUALENV_PYTHON=/usr/bin/python3' >>~/.bashrc)
grep -q 'virtualenvwrapper.sh' ~/.bashrc || (echo 'source /usr/local/bin/virtualenvwrapper.sh' >>~/.bashrc)
grep -q 'Welcome to your Code Institute' ~/.bashrc || (echo 'echo "Hi $C9_USER, welcome to your Code Institute Python workspace in c9, remember to use the mkvirtualenv and workon commands for your projects"' >>~/.bashrc)
source ~/.bashrc
````
I ran each command separately and after the filnal line I did get an output similar 
to that shown in the video.

Now there are a few more lines added to `.bashrc`.

There is a reference to `workon` in the output. I am not sure what this is and the 
video does not follow on it. It has not been applied as far as I know.

---

### Virtual environment

Then use mkvirtualenv to make avirtual environment called foo  
```bash 
$ mkvirtualenv foo
Running virtualenv with interpreter /usr/bin/python3
Using base prefix '/usr'
New python executable in /home/ubuntu/.virtualenvs/foo/bin/python3
Also creating executable in /home/ubuntu/.virtualenvs/foo/bin/python
Installing setuptools, pip, wheel...
done.
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/foo/bin/predeactivate
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/foo/bin/postdeactivate
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/foo/bin/preactivate
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/foo/bin/postactivate
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/foo/bin/get_env_details
(foo) $ 
```
Installs everything that is needed and activates that virtual environment.


`workon` is the command for activating a virtualenv when using the virtualenvwrapper.
Example:  
If you use `mkvirtualenv mynewvenv` then you use `workon mynewvenv` to activate it.

Use `deactivate` to stop a virtual environment.



### Install Django

Install django 1.11
```bash 
(foo) $ sudo pip3 install django==1.11
```

### Requirements file
Follow this by create the first requirements.txt

```bash 
(foo) $ pip freeze > requirements.txt  
```

A `requirements.txt` file is created.

### Create blog project

```bash 
(foo) $ django-admin startproject blog .
```

We now have a `manage.py` file. We can make this executable by adding the executable 
flag.
```bash 
(foo) $ chmod +x manage.py
```

### Do a migration

```bash 
(foo) $ ./manage.py migrate
```

This initialises the database and get tables ready.

### run server

```bash 
(foo) $ ./manage.py runserver $IP:$PORT
```
Error: 
```DisallowedHost at /
Invalid HTTP_HOST header: 'django-blog-ab-anthonybstudent.c9users.io'. You may need to add 'django-blog-ab-anthonybstudent.c9users.io' to ALLOWED_HOSTS.
```
We add `django-blog-ab-anthonybstudent.c9users.io` to `ALLOWED_HOSTS` in 
`blog/settings.py` and refresh. The project now works well.

Next setup git and Travis integration.

### Git

Initialise git

```bash 
(foo) $ git init
Initialized empty Git repository in /home/ubuntu/workspace/.git/
(foo) (master) $ 
```
We now have an empty git repository initialised.

Create a `gitignore` file.
We want to exclude any files that end in sqlite3 so that the database is not 
uploaded. We do not want any of the python bytecode. We do not want the cloud 
9 directory nor the `__pycache__` directory

-e     enable interpretation of backslash escapes
```bash 
(foo) (master) $ echo -e "*.sqlite3\n*.pyc\n.~c9\n__pycache__/" > .gitignore
```
We can check which files will be seen by git by using `git status`.  

Add all the files and commit.
```bash 
(foo) (master) $ git add .
(foo) (master) $ git commit -m "Created simple Django project."
```

Create a github repository. Name: django-blog

The video used SSH but I am using Https

```bash 
(foo) (master) $ git remote add origin https://github.com/abonello/django-blog.git
(foo) (master) $ git push -u origin master
```

### Travis Integration

This is used for Continuous Integration Testing **CI**.

Go to travis-ci.org and sign in with github. Authorize Travis and enter the 
password. When I sign in I have to click on my name and the accounts menu. (It 
looks like this has changed and we are already in the accounts.) 

Find our django-blog project and click on its toggle switch. This will enable
synchronisation between github and Travis.
Now click on the repository name. We want to copy some markup. Next to the 
project name there is a github icon and next to this there is a badge saying
`build unkown`. Click on this. From the second options dropdown select markdown 
and copy the code that appears. Paste this in the README.md. I will place this 
just below the headings.

This will allow us to continually keep track of whether our project is passing 
the tests or not.

The badge actually acts as a link to Travis.

### .travis.yml

We are passing a dummy secret key but when we go on heroku we will hide one as 
an environment variable.

```
language: python
python:
  - "3.4"
# command to install dependencies
install:
  - pip install -r requirements.txt
# command to run tests
script:
  - SECRET_KEY="ThisIsADummySecretKey" ./manage.py test
```
This will sync the repo with Travis.  

After some time (first time takes longer as it has to install requirements), the 
Travis badge turns green showing that our tests have passed. I also recieved an 
email to this effect.



### Set up directory structure

Directories for Templates, Media and Static files.  

1. Create an app  
    Virtual environment need to be activated. Then run:

    ```bash 
    (foo) (master) $ ./manage.py startapp posts
    ```

    A new `posts` directory holding the app is created. In this folder create a 
`templates` directory which will contain all the html templates for this blog
app.

2. We also need a `media` folder which will serve user-uploaded images. Create this 
in the root of the project. Create a subfolder called `img`.

3. Create a `static` folder in the root too. This will contain files that do not 
change. In this folder create a `css` directory for the styles, `img` for static 
images and `js` for javascript.

### settings.py

1. Tell django that we have an app called posts  
    In INSTALLED_APPS add 'posts'.

2. Serving out the templates - Tell django where the templates directory is.
    Find TEMPLATES and find DIR within it
    ```
    TEMPLATES = [
        {   . . .
            'DIRS': [os.path.join(BASE_DIR, 'templates')],
            . . .
        },
    ]
    ```

3. Serving our media files properly   
    Down at the end of context_processors (within the TEMPLATES) add a new 
    context processor (within the OPTIONS)
    `django.template.context_processors.media` 

4. Add a MEDIA_URL to start serving the media.  
    Below the STATIC_URL at the bottom of settings.py add the following
    ```
    MEDIA_URL = '/media/'
    ```

NB. This is how django refers to the url. It does not refer to any specific 
directory at the moment. For this we will need to add the media root setting.  

5. Set MEDIA_ROOT setting  
    The BASE_DIR now refers to the one we created: media  
    ```
    MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
    ```

6. Do the same for static URL, set the ROOT  
    ```
    STATICFILES_DIRS = (os.path.join(BASE_DIR, 'static'),)
    ```


Next create models and forms.

