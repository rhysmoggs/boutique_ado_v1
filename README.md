
- technologies used
    - allauth = "Allauth already has all the features we'll need for the site
and it's completely customizable and will allow us to add even more functionality later on.
Additionally, it's open-source so it's backed by millions of developers who
keep it secure and up-to-date."


DEVELOPMENT/HOW TO REPLICATE:

install django, but not the latest version `pip3 install 'django<4'` in CLI

create project in CLI still, `django-admin startproject boutique_ado .`
`touch .gitignore` to create this file, if not present already (it is in CI's template)
add to thie file:
```
*.sqlite3
*.pyc
__pycache__/
```
if not already in ".gitignore" file.

CLI, run it `python3 manage.py runserver`. should show as successfuly installed in browser.
stop server
CLI, migrate by writing:
`python3 manage.py migrate` then `python3 manage.py createsuperuser` (so that we can login to admin functionalities)
create username, email and password

git add .
git commit -m "Initial commit"
git push

Allauth Setup
"Important!
Please ensure you download the correct version of allauth to follow this video series
In a more recent version of allauth they have removed some files that this project relies on.

To ensure you get all the files you need, please use the command `pip3 install django-allauth==0.41.0`
when installing django-allauth, instead of the command in the video (pip3 install django-allauth)

Please ensure os is imported into "settings.py" (`import os` at top)
The newest version of Django does not automatically import the os module at the top of the "settings.py" 
file as it does for the instructor in this video.
Please check if this line of code is at the top of your settings.py file, it is not, please add it yourself"

Next, follow take from here (https://django-allauth.readthedocs.io/en/latest/installation.html)
in settings.py, find the TEMPLATES section. Add a comment next to this line, so it looks like:
```
'django.template.context_processors.request', # required by allauth, do not delete
```
copy all of the following (from the link above) and add it under the TEMPLATES section
```
AUTHENTICATION_BACKENDS = [
    # Needed to login by username in Django admin, regardless of `allauth`
    'django.contrib.auth.backends.ModelBackend',

    # `allauth` specific authentication methods, such as login by e-mail
    'allauth.account.auth_backends.AuthenticationBackend',
]
```
and copy (again, from the link above) these from the INSTALLED APPS (towards the top of the long list):
```
'django.contrib.sites',
'allauth',
'allauth.account',
'allauth.socialaccount',
```
and add them to the "INSTALLED_APPS" list in settings.py

under (and out of) "AUTHENTICATION_BACKENDS", add `SITE_ID = 1`

in "urls.py", add this `path('accounts/', include('allauth.urls')),` to the "urlpatterns"
and update the imports line to be `from django.urls import path, include`

`python3 manage.py migrate` then `python3 manage.py runserver`
404 error. go to /admin in browser. log in with superuser credentials.
go to Sites > example.com > and update Domain name to be "boutiqueado.example.com" (or your site name)
and Display to "Boutique Ado" (again, wareva you want)

"Sending Emails
When trying to send actual emails from Gitpod, an error stating "Issue binding port" will be
displayed which causes sending of the email to fail. Logging issues to the terminal while
developing on Gitpod, as done in this video, serves to test Authentication and Authorisation
functionality until project deployment.
Once deployed to Heroku, the sending of actual emails will become a possibility, so please
wait until then before attempting it."


in "settings.py", under the `SITE_ID = 1`, add the following:
```
EMAIL_BACKEND = 'django.core.mail.backends.console.EmailBackend'

ACCOUNT_AUTHENTICATION_METHOD = 'username_email'
ACCOUNT_EMAIL_REQUIRED = True
ACCOUNT_EMAIL_VERIFICATION = 'mandatory'
ACCOUNT_SIGNUP_EMAIL_ENTER_TWICE = True
ACCOUNT_USERNAME_MIN_LENGTH = 4
LOGIN_URL = '/accounts/login/'
LOGIN_REDIRECT_URL = '/'
```

run it again, and go to admin login.
under Accounts, click on Email Addresses > Add Email Address (far right button) > User (select 
username from list that popups), enter email in "E-mail address:" input, and tick both "verified"
and "primary". Save.
in CLI:
`pip3 freeze > requirements.txt`
`mkdir templates`
`mkdir templates/allauth`
then git add .
git commit -m "setup allauth"
and git push


Base Template
"
Bootstrap 4 links
Bootstrap recently released their newest version: Bootstrap 5. This means that the standard getbootstrap.com link now defaults to the newest version, which includes updated links, scripts and classes that will not work when following along with this video series.
In order to follow along with these videos, you must ensure you are using the older Bootstrap 4 documentation, which you can find here: https://getbootstrap.com/docs/4.6/getting-started/introduction/
Please bookmark this link to use any time you need to access the Bootstrap documentation during this video walkthrough, including the links you need to install it.
The Bootstrap Github Repository can be accessed here: https://github.com/twbs/bootstrap

Please import the minified jQuery library
While following this videos, please ensure that you import the minified version of jQuery into this project. The slim versions do not include all the features you need for this project

This means that you will need to replace this line from the Starter Template:

`<script src="https://cdn.jsdelivr.net/npm/jquery@3.5.1/dist/jquery.slim.min.js" integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj" crossorigin="anonymous"></script>`
with the following line:
`<script src="https://cdn.jsdelivr.net/npm/jquery@3.5.1/dist/jquery.min.js" integrity="sha256-9/aliU8dGd2tb6OSsuzixeV4y/faTqgFtohetphbbj0=" crossorigin="anonymous"></script>`
Note: Make sure you change the line above in the same place where the Starter Template has it. The jQuery library must be loaded before Popper and Bootstrap for everything to work correctly."
soooooo,

"Because we'll eventually want to customize the Allauth login templates
we'll start this video by making copies of them in our own templates/allauth directory.
This will ensure that our templates take precedence over the built-in ones.
You might recall from the earlier Django videos that anything we install with pip
ends up in the site-packages directory.
So that's where Allauth and all its built-in templates are.
We can copy everything we need with the command (type this in CLI):
`cp -r ../.pip-modules/lib/python3.7/site-packages/allauth/templates/* ./templates/allauth`
explanation (don't add these into CLI):
`cp -r` = to copy recursively
`../` = to go up one level from where we are right now
`python3.7` = may be different depending on which version of python you're using.
so start typing pyth...then hit tab and it'll auto complete and give your version of python.
`/*` = copy everything in this directory.
`/templates/allauth/` = copy into this directory
This gives us a copy of every single Allauth template so we can customize them at will."

"I don't plan on customizing "openid" or the "tests" templates though.So I'm going to delete those 
folders which will just revert those templates back to their Allauth defaults."
Delete those.

"we need a base template for our project itself.
So let's go ahead and right-click the project level templates directory.
And create a new file called 'base.html'"

BOOTSTRAP
bootstrap (this setup uses an older version of bootstrap...4.6. use a newer if you wish):
copy the starter template, found here (https://getbootstrap.com/docs/4.6/getting-started/introduction/#starter-template)
into the newly created "base.html"

then, change the jquery (see above at the big chunk of text with the jQuery links) from the pasted link, to the new one

so it looks like this (moved scripts from bottom to head section, added meta, changed title etc):

```
{% load static %}

<!doctype html>
<html lang="en">
  <head>

    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/css/bootstrap.min.css" integrity="sha384-xOolHFLEh07PJGoPkLv1IbcEPTNtaed2xpHsD9ESMhqIYd0nLMwNLD69Npy4HI+N" crossorigin="anonymous">

    <script src="https://cdn.jsdelivr.net/npm/jquery@3.5.1/dist/jquery.min.js" integrity="sha256-9/aliU8dGd2tb6OSsuzixeV4y/faTqgFtohetphbbj0=" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-Fy6S3B9q64WdZWQUiU+q4/2Lc9npb8tCaSX9FK7E8HnRr0Jz8D6OP9dO5Vg3Q9ct" crossorigin="anonymous"></script>

    <title>Boutique Ado</title>
  </head>
  <body>

  </body>
</html>
```

git add .
git commit -m "added allauth templates and base template"

then, just update it to this:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/dd3dc22adcb97c3f3dce8a047243fceeee517348/templates/base.html)

git add .
git commit -m "added allauth templates and base template"

"so we have a pretty robust base template, now let's create a home app
and extend the base template to get our home page up and running."

`python3 manage.py startapp home`

We need a templates directory inside the home app:
`mkdir -p home/templates/home` ("-p" = make parents as required)

right-click that inner home directory, and create a new file called "index.html"
in index.html:
```
{% extends "base.html" %}
{% load static %}

{% block content %}
    <h1 class="display-4 text-success">It works!</h1>
{% endblock %}
```
we need a view to render this index template page. go to views.py:
```
def index(request):
    """ A view to return the index page """

    return render(request, 'home/index.html')
```
create a new file called urls.py in the home app. add this:
```
from django.contrib import admin
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='home')
]
```
then, add `path('', include('home.urls')),` to the project/main "urls.py"

in "settings.py", add `'home',` to our "INSTALLED_APPS" list.
"then add both the route templates directory.
And our custom allauth directory to the template dirs setting." to look like this:

```
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [
            os.path.join(BASE_DIR, 'templates'),
            os.path.join(BASE_DIR, 'templates', 'allauth'),
        ],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request', # required by allauth, do not delete
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

test it. `python3 manage.py runserver`

git add .
git commit -m "added home app and templates"
git push

go to index.html in the home app. update it to look like:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/6a51b60c029c78540dd10d7c5b241eaa902aa000/home/templates/home/index.html)


git add .
git commit -m "Added homepage content"

got to base.html template, update it to look like (just copy+paste the header. as the scripts are different, from previous steps):
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/ff562b96deb39f74dd41cc7d00beea0e3b21061a/templates/base.html)

git add .
git commit -m "added main page header"

`python3 manage.py runserver`
`mkdir static`
`mkdir media`
`mkdir static/css`
create a "base.css" file in the newly created css folder. add this:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/e3ce173e7475dd8e9c7f51a3af3eaa888c29cb79/static/css/base.css)

copy the "homepage_background_cropped.jpg" file into the media folder created just now.
(https://github.com/Code-Institute-Solutions/boutique_ado_images) < found in the pics folder

update the "base.html" line (1st div) after the header to be:
`<div id="topnav" class="row bg-white pt-lg-2 d-none d-lg-flex">`

import "lato" google fonts into "base.html", not the css file!..in the block corecss, bottom of list in there.
also, add a "base.css" link under that too:
`<link href="{% static 'css/base.css' %}" rel="stylesheet">`
also, add your fontawesome kit code (log in to fontawesome, find kits, click it, link it) to the top of the corejs block in the same file.

go to "settings.py" and update the static files to be:
```
STATIC_URL = '/static/'
STATICFILES_DIRS = (os.path.join(BASE_DIR, 'static'),)
```
and add media files, underneath the added code:
```
# Media files (Uploaded media images will go here)
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
```
"Lastly to allow Django to see the MEDIA_URL.
We need to go to urls.py
Import our settings and the static function from django.conf.urls.static
And then use the static function to add the MEDIA_URL to our list of URLs."
go to "urls.py", and update it to be:
```
from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    path('accounts/', include('allauth.urls')),
    path('', include('home.urls')),
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

git add .
git commit -m "completed homepage header and css"
git push

NAVBAR
go to "base.html" and updated header section to be:
(https://github.com/Code-Institute-Solutions/Boutique-Ado/blob/master/05-The-Home-Page/Main-Site-Navigation/templates/base.html)

"I'm gonna create a directory in the templates folder commonly found in larger web projects called includes.
This gives us a nice place where we can create small HTMLsnippets
and then include them in the base template using Django."

`mkdir templates/includes`
creates two files in that new folder: "main-nav.html" and "mobile-top-header.html"
update them both to be:
(https://github.com/Code-Institute-Solutions/Boutique-Ado/tree/master/05-The-Home-Page/Main-Site-Navigation/templates/includes)

run server to test.

add the class, just under the .border-black class in the "base.css" file
```
.bg-black {
    background: #000 !important;
}
```

git add .
git commit -m "added mobile header and main navbar"
git push

PRODUCTS PAGE/APP
"We pruned these from a data set at kaggle.com which is a great provider of
free sample data for use in all sorts of industries."

copy the following images (download zip) into the media folder:
(https://github.com/Code-Institute-Solutions/boutique_ado_images/tree/master/pics)

Create the products app, in CLI: `python3 manage.py startapp products`
then add this products app the the 'INSTALLED_APPS' list in "settings.py" by: `'products',`

"make a folder called fixtures inside the products app"
`mkdir products/fixtures`

"Fixtures are used to load data very quickly into a django database
so we don't have to do it all manually in the admin.
I'm gonna drag and drop a couple of JSON fixture files in here
one for categories and one for products."
copy these into the fixtures folder:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/tree/bf096a773ea7e32253e20f58c1d6139317f681be/products/fixtures)

copy the content in the products.json file, use a json formatter (https://jsonformatter.org/)
and paste the code there. it should make it more readable.

"With the fixture files in place and the product app created.
We need to create some models for the fixtures to go in."

go to products (app) > "models.py" and update to:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/bf096a773ea7e32253e20f58c1d6139317f681be/products/models.py)

`python3 manage.py makemigrations --dry-run`

should error
`pip3 install Pillow`

It's worth mentioning though that if you're not going to use the plan flag
you should specify the specific app that you're migrating
so you don't unintentionally apply migrations from other apps.

`python3 manage.py makemigrations --dry-run`
`python3 manage.py makemigrations`
`python3 manage.py migrate --plan`
`python3 manage.py migrate`

go to "admin.py" and update to:
```
from django.contrib import admin
from .models import Product, Category

# Register your models here.
admin.site.register(Product)
admin.site.register(Category)
```

"we've got our models created, our fixtures ready to go, our product images uploaded.
And our migrations applied. The only thing left to do is actually use these fixtures."

"we'll start with categories since the products need to know which category to go in."
`python3 manage.py loaddata categories`
`python3 manage.py loaddata products`

"To confirm let's go to the admin and check out a product."
`python3 manage.py runserver` then /admin in browser. log in > products - all there.

git add .
git commit -m "added products app, models and fixtures"
git push

`python3 manage.py runserver` /admin
"The first easy fix is to adjust the spelling of 'Categorys'.
Since it's incorrect here based on django just adding an 's' to the model name."

back in gitpod, products > "models.py"
"We can fix the spelling issue on the category model by adding a special metaclass to the model itself."
"this won't require migration since we're not making any
changes to the structure of the model so this simple addition will fix that issue."
Update it to:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/722adeed7048c1ec88cd3901c140119a51758c17/products/models.py)

products > "admin.py". update it to:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/722adeed7048c1ec88cd3901c140119a51758c17/products/admin.py)

git add .
git commit -m "Customized admin"

products > "views.py" and update it to be:
```
from django.shortcuts import render
from .models import Product

# Create your views here.

def all_products(request):
    """ A view to show all products, including sorting and search queries """

    products = Product.objects.all()

    context = {
        'products': products,
    }

    return render(request, 'products/products.html', context)
```

in products app, create a file named "urls.py", update it to be:
```
from django.urls import path
from . import views

urlpatterns = [
    path('', views.all_products, name='products')
]
```

"And this is a good reminder it actually looks like we aren't even using the 
`from django.contrib import admin` in either of these files.
So I'm going to remove it from both just to make sure that our code stays clean"
("urls.py" in "home" app too)

go to the boutique-ado project "urls.py", and add:
`path('products/', include('products.urls')),` to the list there.

Create the templates:
`mkdir -p products/templates/products`
create "products.html" in that "products" folder, update to be:
```
{% extends "base.html" %}
{% load static %}

{% block page_header %}
    <div class="container header-container">
        <div class="row">
            <div class="col"></div>
        </div>
    </div>
{% endblock %}

{% block content %}
    <div class="container">
        <div class="row">
            <div class="col">
                {{ products }}
            </div>
        </div>
    </div>
{% endblock %}
```
`python3 manage.py runserver` /products in browser. should display. although messy.

git add .
git commit -m "added products views and templates"
git push

update products.html to:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/e3c29afef63a8e5a8dae3fdc6b1277eb32206dbc/products/templates/products/products.html)
and base.css to:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/e3c29afef63a8e5a8dae3fdc6b1277eb32206dbc/static/css/base.css)

git add .
git commit -m "started product template"
git push

VIEW INDIVIDUAL PRODUCT DETAILS
in templates > includes > "main-nav.html", add: `{% url 'products' %}`
to inside the href of the "All Products" a tag, under the All Products section.

in home > templates > home > "index.html", add: `{% url 'products' %}`
to inside the href of the "Shop Now" a tag.

"To create the product details page we need a new view that will take the product_id as a
parameter and return the template including the product."

in products > "views.py", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/283ce70483507eda1ae31143c8450b9f52cdb5b1/products/views.py)

in products > "urls.py" add this:
`path('<product_id>', views.product_detail, name='product_detail'),` to the 'urlpatterns' (make sure comma on end of all paths)

create a template, so, duplicate "products.html" template, rename it to "product_detail.html" and update it to look like:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/283ce70483507eda1ae31143c8450b9f52cdb5b1/products/templates/products/product_detail.html)

in the "products.html" file, add `{% url 'product_detail' product.id %}` to both empty hrefs uninside the "{% if product.image %}" section. 

update "base.css" to add a media query at the bottom:
```
/* pad the top a bit when navbar is collapsed on mobile */
@media (max-width: 991px) {
    .header-container {
        padding-top: 116px;
    }

    body {
        height: calc(100vh - 116px);
    }
}
```

git add .
git commit -m "added product details functionality
git push

