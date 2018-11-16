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


---

create alias for `./manage.py runserver $IP:$PORT`   
call it `run`

in .bash_aliases add
```
alias run="./manage.py runserver $IP:$PORT"
```

Restart .bash_aliases
```
. ~/.bash_aliases
```

alternatively we can say
```
source ~/.bash_aliases
```
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

### Model and Forms

Under `posts` directory open `models.py`. We will be doing some work with time 
and date. For this reason we will import `timezone` from `django.utils`.  

Create a new class called `Post` which inherits from `models.Model`.

```python 
from django.db import models
from django.utils import timezone

class Post(models.Model):
    """
    A single blog post.
    """
    
    title = models.CharField(max_length=200)
    content = models.TextField()
    created_date = models.DateTimeField(auto_now_add=True)
    published_date = models.DateTimeField(blank=True, null=True, default=timezone.now)
    views = models.IntegerField(default=0)
    tag = models.CharField(max_length=30, blank=True, null=True)
    image = models.ImageField(upload_to="img", blank=True, null=True)
    
    def __unicode__(self):
        return self.title
```

* **title** field - character field with a maximum length of 200

* **content* - a text field 

* **created_date** - when our blog post was created - a created date field, a 
date and time field with an attribute of auto_now_add = true, ie. as soon as the 
record is created then the current date and time will be added into that field. 

* **published_date** - created date could be very different to published date 
This starts off blank, nulls are allowed but its default value is the current
time as we get from the time zone. 

* **views** - record the number of views that our blog post has, integer field 
with default set to zero (when we start, we have no views). 

* **tags** - ability to add tags to group blog posts together, 
character field with maximum length of 30, can be blank at the beginning and 
nulls are allowed 

* **image** - user can upload an image, image field 
attribute upload_to="img". *img* corresponds to the directory that we created 
under media. This can be blank and can be null.


We need to make sure that our Posts are accessible through the admin backend, 
we need to add them and register them in the admin.py.
```python 
from django.contrib import admin
from .models import Post
 
admin.site.register(Post)
```


### Forms

Use the Django forms library to create our form. 

Create a `forms.py` file in the posts app folder. Import the forms library. 
We also need to import the Post class from our models.

We will create a BlogPostForm. We only want the user editable fields in the 
form.
```python 
from django import forms
from .models import Post

class BlogPostForm(forms.ModelForm):
    class Meta:
        model = Post
        fields = ('title', 'content', 'image', 'tag', 'published_date')
```
 
 Becasue we are using `image field` we need a library called **`Pillow`**. We 
 will pip install this otherwise our migrations would fail. Then we will create 
 a new requirements.txt file.
 
 ```bash 
 (foo) (master) $ pip3 install pillow
 (foo) (master) $ pip freeze > requirements.txt
 ```
 
 Next we will do migrations.
 ```bash 
 (foo) (master) $ ./manage.py makemigrations
Migrations for 'posts':
  posts/migrations/0001_initial.py
    - Create model Post
 (foo) (master) $ ./manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, posts, sessions
Running migrations:
  Applying posts.0001_initial... OK
```
 
Now our database is updated. We created our models and forms.
 
Next: create views.
 
### Views
 
Open `views.py` file from `posts` directory. We need some more libraries apart 
 from the standard *render*.
* get_object_or_404
* redirect

We want the timezone because we are handling date and time.  
Import the model *Post* and the form *BlogPostForm* we created.  

We will have three views:
1. **get_posts** - return list of all posts.
2. **post_detail** - return all the information about a single post.
3. **create_or_edit_post** - allows the editing of a post or creation of a new one.


```python 
from django.shortcuts import render, get_object_or_404, redirect
from django.utils import timezone
from.models import Post
from .forms import BlogPostForm

def get_posts(request):
    """
    Create a view that will return a list of
    Posts that were published prior to 'now'
    and render them to the 'blogposts.html' template.
    """
    
    posts = Post.objects.filter(published_date__lte=timezone.now()).order_by('-published_date')
    return render(request, "blogposts.html", {"posts": posts})


def post_detail(request, pk):
    """
    Create a view that returns a single Post
    object based on the post ID (pk) and render
    it to the 'postdetail.html' template.
    OR return a 404 error if the post is not found.
    """
    
    post = get_object_or_404(Post, pk=pk)
    post.views += 1
    post.save()
    return render(request, "postdetail.html", {"post": post})
    
def create_or_edit_post(request, pk=None):
    """
    Create a view that allows us to create or edit
    a post depending if post ID is null or not.
    """
    
    post = get_object_or_404(Post, pk=pk) if pk else None
    if request.method == "POST":
        form = BlogPostForm(request.POST, request.FILES, instance=post)
        if form.is_valid():
            post = form.save()
            return redirect(post_detail, post.pk)
    
    else:
        form = BlogPostForm(instance=post)
    
    return render(request, "blogpostform.html", {"form": form})
```

### Urls

We will start working in the project's `urls.py` file.  
Open `blog/urls.py` file.


We need to add some libraries. Add the *include*, *RedirectView* and *serve* 
libraries. We also want to import MEDIA_ROOT from `settings.py` to serve our
media url.

```python 
from django.conf.urls import url, include
from django.contrib import admin
from django.views.generic import RedirectView
from django.views.static import serve
from .settings import MEDIA_ROOT

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^$', RedirectView.as_view(url='posts/')),
    url(r'posts/', include('posts.urls')),
    url(r'^media/(?P<path>.*)$', serve, {'document_root': MEDIA_ROOT}),
]
```

We will be including a `urls.py` file in our `posts` app. We have to create this.

```python 
from django.conf.urls import url
from .views import get_posts, post_detail, create_or_edit_post

urlpatterns = [
    url(r'^$', get_posts, name="get_posts"),
    url(r'^(?P<pk>\d+)/$', post_detail, name="post_detail"),
    url(r'new/$', create_or_edit_post, name="new_post"),
    url(r'?P<pk>\d+)/edit/$', create_or_edit_post, name="edit_post"),
]
```

### Templates

We need a `templates` folder at the top level of our project to hold our 
`base.html` file. This is the base template.

Done.  

#### CSS
Create a `custom.css` file in `static/css`.  
copied code from https://github.com/Code-Institute-Solutions/blog_miniproject1/blob/master/static/css/custom.css  
Overriding some bootstrap classes and adding some extra styling.
  
  
#### First template in posts
In `posts/templates` create `blogposts.html`.  
Next create `postdetail.html` in the same folder.

Berfore the next template we need to install a new library.

```bash 
(foo) (master) $ pip install django-forms-bootstrap
Collecting django-forms-bootstrap
  Downloading https://files.pythonhosted.org/packages/f2/bd/6bff32d77e093ed7a5804591b8a4dd46f1ccd621129fdcad84afc6d8be30/django_forms_bootstrap-3.1.0-py2.py3-none-any.whl
Installing collected packages: django-forms-bootstrap
Successfully installed django-forms-bootstrap-3.1.0
```
This help to style forms with bootstrap classes.

Update requirements.txt using pip freeze.

Add this new app to the INSTALLED_APPS in `settings.py`.

Next create our final template, `blogpostform.html`.  
This template will load bootstrap_tags provided by django-forms-bootstrap library.  
It allows us to format our forms nicely.

The form is going to use **`enctype='multipart/form-data`**. 
This is an encoding type that allows files to be sent through a POST. 
Quite simply, without this encoding the files cannot be sent through POST. 
If you want to allow a user to upload a file via a form, you must use this enctype.

The form will be automatically generated based on the form passed by the view.


**CSRF token** is a protection mechanism provided by Django to make sure that 
your website isn't vulnerable to cross-site request forgery attacks.

### Create superuser

```bash 
(foo) (master) $ ./manage.py createsuperuser
Username (leave blank to use 'ubuntu'): admin
Email address: admin@example.com
Password: 
Password (again): 
Superuser created successfully.
```

password is 123qwe456


### Uploading media

uploaded from github


### Housekeeping

In `settigns.py`:

1. Secret Key
    We are going to replace the temporary secret key we were using with a variable
    stored in `env.py` file or an environment variable in heroku. This file will 
    not be pushed to github.

    Will generate new keys using 
    [Django Secret Key Generator - miniwebtools](https://www.miniwebtool.com/django-secret-key-generator/)
    
    create `env.py` file. Added to `.gitignore`
    ```python 
    import os

    os.environ.setdefault("SECRET_KEY", "'----put-new-secret-key-here-----'")
    ```
    
    This is for local testing. On heroku we will set the Environment variable.
    





