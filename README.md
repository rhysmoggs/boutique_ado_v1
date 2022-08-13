
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

it should error
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

FILTER/Queries and Categories

Search queries:
in "base.html", on the GET form, add `{% url 'products' %}` to the empty action.
and in "mobile-top-header.html", on the GET form, add `{% url 'products' %}` to the empty action.

"This change means that when we submit a search query, it'll end up in the url as a get parameter.
We can access those url parameters in the 'all_products' view by checking whether request.get exists.
Since we named the text input in the form 'q', We can just check if q is in request.get
If it is, I'll set it equal to a variable called query.
If the query is blank it's not going to return any results. So if that's the case let's use
the Django messages framework to attach an error message to the request.
And then redirect back to the products url." sooo, to do this,

"We'll also need to import messages, redirect, and reverse up (top of page) in order for that to 
work and we'll talk more about this later. If the query isn't blank:
I'm going to use a special object from django.db.models called Q to generate a search query.
This deserves a bit of an explanation.
In django if you use something like product.objects.filter,
In order to filter a list of products, everything will be AND-ed together.
In the case of our queries that would mean that when a user submits a query,
in order for it to match the term, it would have to appear in both the product name and the product
description. Instead, we want to return results where the query was matched in either
the product name OR the description. In order to accomplish this OR logic, we need to use Q.
This is worth knowing because in real-world database operations, queries can become quite complex
and using Q is often the only way to handle them. Because of that, I'd strongly recommend
that you become familiar with this, and the other complex database functionality, by reading 
through the queries portion of the Django documentation."

go to products > "views.py" and update the 'all_products' view to be:
```
def all_products(request):
    """ A view to show all products, including sorting and search queries """

    products = Product.objects.all()
    query = None

    if request.GET:
        if 'q' in request.GET:
            query = request.GET['q']
            if not query:
                messages.error(request, "You didn't enter any search criteria!")
                return redirect(reverse('products'))
            
            queries = Q(name__icontains=query) | Q(description__icontains=query)
            products = products.filter(queries)

    context = {
        'products': products,
        'search_term': query,
    }

    return render(request, 'products/products.html', context)
```

"I'll set a variable ('queries') equal to a Q object, where the name contains the query
OR the description contains the query. The pipe (|) is what generates the OR statement.
And the 'i' in front of 'contains' makes the queries case insensitive.
With those queries constructed, I can now pass them to the filter method in order to actually 
filter the products. Now I'll add the query to the context. And in the template call it 'search_term',
and we'll start with it as 'none' at the top of this view to ensure we don't get an error
when loading the products page without a search term (query = None, at the top of the view)."

or, just copy this (includes imports at top of page):
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/e59745990a036961e36cc0d469458b5e9b5a80de/products/views.py)

git add .
git commit -m "added search functionality"
git push


in "main-nav.html", in the 'Clothing' update the a tags to be:

"In order to handle filtering by category, I'm gonna pass a 'category' parameter to the
products URL just like we're doing with 'q' for search queries.
These parameters will use the name field from the category model so we have
a programmatic way to access them.
Of course we could also use the category id.
But this makes the URLs a little more semantic which is nice for the user."

"So for the first drop-down menu clothing. This top link here will point to the products URL.
Followed by a question mark to indicate we're about to pass the category parameter.
Followed by activewear and essentials separated by a comma.
This syntax ensures we end up with a comma-separated string in the view."

```
<a href="{% url 'products' %}?category=activewear,essentials" class="dropdown-item">Activewear &amp; Essentials</a>
<a href="{% url 'products' %}?category=jeans" class="dropdown-item">Jeans</a>
<a href="{% url 'products' %}?category=shirts" class="dropdown-item">Shirts</a>
<a href="{% url 'products' %}?category=activewear,essentials,jeans,shirts" class="dropdown-item">All Clothing</a>
```

go to products > "views.py" and update the 'all_products' view to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/95eb9fc676c29902338d99c5d624b0b5771f0448/products/views.py)

"It's worth noting by the way, that this double underscore syntax is common when making queries in django.
Using it here means we're looking for the name field of the category model.
And we're able to do this because category and product are related with a foreign key.
An alternative way to do this would be to filter all categories down to those contained in this list
and then filter products based on the category ID instead of the name.
But this would take more queries and add unnecessary complexity.
It's faster and easier to just drill into the related model using this double underscore syntax."

update "main-nav.html" to be (just updating the link, similar to what we've done previously)
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/95eb9fc676c29902338d99c5d624b0b5771f0448/templates/includes/main-nav.html)

git add .
git commit -m "added category filtering"
git push


SORT PRODUCTS BY PRICE, RATING, CATEGORY

"we're gonna finish the main navbar by adding the ability to sort products by price rating and category.
In main_nav.html let's start in the all products menu by adding some links to the products view.
with two new get parameters. Sort and direction."

in "main-nav.html", update it to:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/673a36fdd4bb2c09f8843c6ad8cb6ae4a60dda01/templates/includes/main-nav.html)

go to poducts > "views.py", update all_products view to be:
Rewatch Full Stack Frameworks > Project - Boutique Ado > Product Sorting > "Sorting Products Part 1" video.

`python3 manage.py runserver`

git add .
git commit -m "added sorting"
git push


cont SORTING, CATEGORY SORTING
Rewatch Full Stack Frameworks > Project - Boutique Ado > Product Sorting > "Sorting Products Part 2" video.
update "products.html" to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/ec933ec96eeb42b40e0534e40d40fb38cc999940/products/templates/products/products.html)
and "product_detail" to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/ec933ec96eeb42b40e0534e40d40fb38cc999940/products/templates/products/product_detail.html)
and "base.css" to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/ec933ec96eeb42b40e0534e40d40fb38cc999940/static/css/base.css)
update products> "views.py" to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/ec933ec96eeb42b40e0534e40d40fb38cc999940/products/views.py)

git add .
git commit -m "added sorting and product counts to product page"
git push


cont CATEGORY SORTING. Source selector dropdown box.
Rewatch Full Stack Frameworks > Project - Boutique Ado > Product Sorting > "Sorting Products Part 3" video.

update "products.html" to be (adds js and jQuery at bottom):
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/656166307e469630d09e0eb17a0d17daa440e208/products/templates/products/products.html)

in products > "views.py", add `from django.db.models.functions import Lower` to the bottom of the from/imports list at the top of the page.

add 'back to top link button' on products page (adds html and bottom and then jquery to scrips section):
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/656166307e469630d09e0eb17a0d17daa440e208/products/templates/products/products.html)

and "base.css" to be (btt-button and btt-link classes):
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/656166307e469630d09e0eb17a0d17daa440e208/static/css/base.css)

git add .
git commit -m "Added sorting js and back to top link"
git push



THE SHOPPING BAG
`python3 manage.py startapp bag`
add `'bag'` to bottom of 'INSTALLED_BAGS' list in boutique_ado > "settings.py"
in bag > "views.py", add this view: 
```
def view_bag(request):
    """ A view that renders the bag contents page """

    return render(request, 'bag/bag.html')
```

`mkdir -p bag/templates/bag`

copy the info in home > "index.html" and paste into the new bag.html, then delete the code within the blockcontent
create "urls.py" in the bag main folder and add:
```
from django.urls import path
from . import views

urlpatterns = [
    path('', views.view_bag, name='view_bag')
]
```
in the project boutique-ado > "urls.py" file, add `path('bag/', include('bag.urls')),` to the list within "urlpatterns"

in templates > "base.html", add `{% url 'view_bag' %}` to the empty href to be :
`<a class="{% if grand_total %}text-info font-weight-bold{% else %}text-black{% endif %} nav-link" href="{% url 'view_bag' %}">`

in templates > includes > "mobile-top-header.html", add `{% url 'view_bag' %}` to the empty href to be :
`<a class="{% if grand_total %}text-primary font-weight-bold{% else %}text-black{% endif %} nav-link d-block d-lg-none" href="{% url 'view_bag' %}">`

test it by running it, clicking on shopping bag icon. should work.


ADD CONTENT TO THE BAG TEMPLATE:
in bag > templates> bag > "bag.html", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/9c2aa64f4edffb25e330d722ee66542e553edb80/bag/templates/bag/bag.html)

git add .
git commit -m "added shopping bag app, urls and template"

create "contexts.py" file in the main bag app. update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/9c2aa64f4edffb25e330d722ee66542e553edb80/bag/contexts.py)

update boutique-ado > "settings.py" to have `'bag.contexts.bag_contents',` at the bottom of the list inside the "TEMPLATES", "OPTIONS"
section (bottom of the list under 'django.contrib...' etc)
add two variables to the bottom of "settings.py" too:
```
FREE_DELIVERY_THRESHOLD = 50
STANDARD_DELIVERY_PERCENTAGE = 10
```

templates > "base.html", towards the bottom of the file, make sure that it's "free_delivery_threshold", NOT "free_shipping_threshold":
`<h4 class="logo-font my-1">Free delivery on orders over ${{ free_delivery_threshold }}!</h4>`.

git add .
git commit -m "added bag context and delivery logic"
git push


ADDING PRODUCTS
products > templates > products > "product_detail.html", updaet it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/21e3b111100f5c8daa4bc6e9565bde09980b33d8/products/templates/products/product_detail.html)

static > css > "base.css", just under the .btn-black class, add the following classes:
```
.btn-outline-black {
    background: white;
    color: black !important; /* use important to override link colors for <a> elements */
    border: 1px solid black;
}

.btn-outline-black:hover,
.btn-outline-black:active,
.btn-outline-black:focus {
    background: black;
    color: white !important;
}
```

bag > "views.py", add the "add_to_bag" view:
```
def add_to_bag(request, item_id):
    """ Add a quantity of the specified product to the shopping bag """

    quantity = int(request.POST.get('quantity'))
    redirect_url = request.POST.get('redirect_url')
    bag = request.session.get('bag', {})

    if item_id in list(bag.keys()):
        bag[item_id] += quantity
    else:
        bag[item_id] = quantity

    request.session['bag'] = bag
    print(request.session['bag'])
    return redirect(redirect_url)
```
and add 'redirect' to the list of imports at the top.

bag > "urls.py", add the "add_to_bag" view and add:
`path('add/<item_id>/', views.add_to_bag, name='add_to_bag'),` to the "urlpatterns"
(add comma at end of both, if not there already)

test it by clicking "add to bag" button on product_detail pages. in CLI, it should
print out the quantity

git add .
git commit -m "added add to be bag functionality"
git push

remove print statement from bag > "views.py" file
in bag > "contexts.py", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/4b28dfe82da5e5e24ed830f15ebe4f70deca8886/bag/contexts.py)

in bag > templates > bag > "bag.html", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/4b28dfe82da5e5e24ed830f15ebe4f70deca8886/bag/templates/bag/bag.html)

git add .
git commit -m "updated context processor and updated shopping bag template"
git push


products > "models.py", add `has_sizes = models.BooleanField(default=False, null=True, blank=True)` to the Products list,
in between description and price.
"Because this is a change to the structure of the model. We'll need to run migrations for this."

`python3 manage.py makemigrations --dry-run`
`python3 manage.py makemigrations`
`python3 manage.py migrate --plan`
`python3 manage.py migrate`

from video Full Stack Frameworks With Django > Project - Boutique Ado > Adding Products > "Adding Products Part 3"
"We're gonna dive into a very handy tool for exploring and making changes to the items in our database."
`python3 manage.py shell`
in it, in the CLI:
`from products.models import Product` ENTER
`kdbb = ['kitchen_dining', 'bed_bath']` ENTER
`clothes = Product.objects.exclude(category__name__in=kdbb)` ENTER
`clothes.count()` ENTER
`for item in clothes:` ENTER
    `item.has_sizes = True` ENTER
    `item.save()` ENTER ENTER
`Product.objects.filter(has_sizes=True)` ENTER
`Product.objects.filter(has_sizes=True).count()` ENTER
`exit()` ENTER

products > templates > "product_details.html", update it to (add a dropdown to select product size):
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/5e595d250f0d7a408a7ccd40bfa25d24c000034d/products/templates/products/product_detail.html)

`python3 manage.py runserver`

bag > templates > bag > "bag.html", update it to (add size to product info):
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/5e595d250f0d7a408a7ccd40bfa25d24c000034d/bag/templates/bag/bag.html)

`python3 manage.py runserver`

git add .
git commit -m "added sizes to product model and size selector to product template"
git push

update the products that don't require a size from the Special Offers tab on the website (cufflinks, pillow e.g. manually)
`python3 manage.py runserver` /admin > Products, arrange by name (top of table sorting)
pp5003270936 - Silver Superman Shield Cufflinks > Has Sizes = No > Save
pp5003960062 - Home Expressions Microfiber Twin XL Sheet Set > Has Sizes = No > Save
pp5005850180 - Sunbeam Comfy Toes Microplush Heated Throw > Has Sizes = No > Save
pp5005940299 - Vieste Green Stone Statement Necklace > Has Sizes = No > Save
pp5007250126	MaryJane's Home Cotton Clouds Square Decorative Pillow > Has Sizes = No > Save

bag > "views.py", update "add_to_bag" view to be:
```
def add_to_bag(request, item_id):
    """ Add a quantity of the specified product to the shopping bag """

    quantity = int(request.POST.get('quantity'))
    redirect_url = request.POST.get('redirect_url')
    size = None
    if 'product_size' in request.POST:
        size = request.POST['product_size']
    bag = request.session.get('bag', {})

    if size:
        if item_id in list(bag.keys()):
            if size in bag[item_id]['items_by_size'].keys():
                bag[item_id]['items_by_size'][size] += quantity
            else:
                bag[item_id]['items_by_size'][size] = quantity
        else:
            bag[item_id] = {'items_by_size': {size: quantity}}
    else:
        if item_id in list(bag.keys()):
            bag[item_id] += quantity
        else:
            bag[item_id] = quantity

    request.session['bag'] = bag
    return redirect(redirect_url)
```

bag > "contexts.py", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/b44963bb7b88c61ba39e67ef8a311181837e6b89/bag/contexts.py)

bag > templates > bag > "bag.html", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/b44963bb7b88c61ba39e67ef8a311181837e6b89/bag/templates/bag/bag.html)

git add .
git commit -m "Finished product size logic"
git push

from video Full Stack Frameworks With Django > Project - Boutique Ado >  Adding Products > Adding Products Part 5
(also explains js for buttons, very handy - but also a flaw, which is fixed later)

products > templates > products > "product_detail.html", update it to be (incliudes adding + and - buttons):
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/0a5335f2e95f58b10207ea57cedcdd063251a3ff/products/templates/products/product_detail.html)

create "includes" folder in the products > templates > products folder
"I'm doing this as an HTML file since it'll just be a script element we include at the end of the product detail template
and this avoids having to deal with additional static files just for a single JavaScript file."
create "quantity_input_script.html" file within that "includes" folder, then add the following:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/0a5335f2e95f58b10207ea57cedcdd063251a3ff/products/templates/products/includes/quantity_input_script.html)

git add .
git commit -m "Added quantity input +/- buttons"
git push

bag > templates > bag > "bag.html", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/7e5faf3f2fc5fe20e382f91ea0b86a28b4917359/bag/templates/bag/bag.html)

bag > "contexts.py", in the 'bag_contents' view, in the else statement, change `'quantity': item_data,` to:
`'quantity': quantity,`

static > css > "base.css", update the ".btt-link" class to be:
```
.btt-link,
.update-link,
.remove-item {
    cursor: pointer;
}
```

`python3 manage.py runserver`
git add .
git commit -m "Added quantity boxes to shopping bag"
git push

bag > "views.py", add a 'adjust_bag' view:
```
def adjust_bag(request, item_id):
    """ Adjust the quantity of the specified product to the specified amount """

    quantity = int(request.POST.get('quantity'))
    size = None
    if 'product_size' in request.POST:
        size = request.POST['product_size']
    bag = request.session.get('bag', {})

    if size:
        if quantity > 0:
            bag[item_id]['items_by_size'][size] = quantity
        else:
            del bag[item_id]['items_by_size'][size]
    else:
        if quantity > 0:
            bag[item_id] = quantity
        else:
            bag.pop[item_id]

    request.session['bag'] = bag
    return redirect(reverse('view_bag'))
```
and add 'reverse' to the list of imports at the top.

bag > "urls.py", add: `path('adjust/<item_id>/', views.adjust_bag, name='adjust_bag'),`

bag > templates > bag > "bag.html", add: `{% url 'adjust_bag' item.item_id %}` to the empty action in the form around
~line 52

bag > "views.py", add a 'remove_from_bag' view:
```
def remove_from_bag(request, item_id):
    """ Remove the item from the shopping bag """

    try:
        size = None
        if 'product_size' in request.POST:
            size = request.POST['product_size']
        bag = request.session.get('bag', {})

        if size:
            del bag[item_id]['items_by_size'][size]
            if not bag[item_id]['items_by_size']:
                bag.pop(item_id)
        else:
            bag.pop(item_id)

        request.session['bag'] = bag
        return HttpResponse(status=200)

    except Exception as e:
        return HttpResponse(status=500)
```
and update the 'adjust_bag' view section, 'if size...' to be:
```
if size:
        if quantity > 0:
            bag[item_id]['items_by_size'][size] = quantity
        else:
            del bag[item_id]['items_by_size'][size]
            if not bag[item_id]['items_by_size']:
                bag.pop(item_id)
```
and `bag.pop(item_id)` instead of `bag.pop[item_id]` for the "adjust_bag" view.
add "HttpResponse" to the list of imports at the top

bag > "urls.py", add: `path('remove/<item_id>/', views.remove_from_bag, name='remove_from_bag'),`



bag > templates > bag > "bag.html",  update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/51702653fda1c506bbe7057376a704f220d82c8b/bag/templates/bag/bag.html)

templates > "base.html",  add:
`<script src="https://code.jquery.com/jquery-3.6.0.min.js" integrity="sha256-/xUj+3OJU5yExlq6GSYGSHk7tPXikynS7ogEvDej/m4=" crossorigin="anonymous"></script>`
to the top of the "block corejs" section, underneath the fontawesome but above the rest in there.

"I'll also create an empty file called "`__init__ .py`" which will ensure that this directory is treated as a Python package,
making our bag tools module available for imports and to use in templates."
create folder "templatetags" in "bag" app, then create a file named "bag_tools.py" and another file named "`__init__.py`"

in the "bag_tools.py", add:
```
from django import template


register = template.Library()

@register.filter(name='calc_subtotal')
def calc_subtotal(price, quantity):
    return price * quantity
```

git add .
git commit -m "added adjust bag and remove from bag views, and calc subtotal template filter"
git push


TOASTS
create a folder named "toasts" inside templates > includes folder, then a file within it named "toast_success.html"
add the following to "toast_success.html":
```
<div class="toast custom-toast rounded-0 border-top-0" data-autohide="false">
    <div class="arrow-up arrow-success"></div>
    <div class="w-100 toast-capper bg-success"></div>
    <div class="toast-header bg-white text-dark">
        <strong class="mr-auto">Success!</strong>
        <button type="button" class="ml-2 mb-1 close text-dark" data-dismiss="toast" aria-label="Close">
            <span aria-hidden="true">&times;</span>
        </button>
    </div>
    <div class="toast-body bg-white">
        {{ message }}        
    </div>
</div>
```

create "toast_error.html" in the same location and paste the same code into that, but change the class
"arrow-success" to "arrow-danger", "bg-success" to "bg-danger" and the strong "Success!" to "Error!"

create "toast_info.html" in the same location and paste the same code into that, but change the class
"arrow-success" to "arrow-info", "bg-success" to "bg-info" and the strong "Success!" to "Alert!"

create "toast_warning.html" in the same location and paste the same code into that, but change the class
"arrow-success" to "arrow-warning", "bg-success" to "bg-warning" and the strong "Success!" to "Warning!"

templates > "base.html" file, update the message-container div at the bottom to be:
```
<div class="message-container">
    {% for message in messages %}
        {% with message.level as level %}
            {% if level == 40 %}
                {% include 'includes/toasts/toast_error.html' %}
            {% elif level == 30 %}
                {% include 'includes/toasts/toast_warning.html' %}
            {% elif level == 25 %}
                {% include 'includes/toasts/toast_success.html' %}
            {% else %}
                {% include 'includes/toasts/toast_info.html' %}
            {% endif %}
        {% endwith %}
    {% endfor %}
</div>
```
and, inside the "block postloadjs":
```
<script type="text/javascript">
    $('.toast').toast('show');
</script>
```

go to bag > "views.py" and update to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/e5323862cc7563f65526f0108f37c57ad92f7931/bag/views.py)

boutique-ado project > "settings.py", add `MESSAGE_STORAGE = 'django.contrib.messages.storage.session.SessionStorage'`
just under the 'TEMPLATES' section, and above the 'AUTHENTICATION_BACKENDS'

static > "base.css", add the following, just above the mediaqueries section:
```
/* ------------------------------- bootstrap toasts */

.message-container {
    position: fixed;
    top: 72px;
    right: 15px;
    z-index: 99999999999;
}

.custom-toast {
    overflow: visible;
}

.toast-capper {
    height: 2px;
}
```
`python3 manage.py runserver`
try to add a product to the shopping bag, see if a toast/pop-up message appears.

git add .
git commit -m "Added toasts"
git push

bag > "views.py", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/d2c3833acdc59434e38f2ba03b4b5135377f89f4/bag/views.py)

`python3 manage.py runserver`
should have messages pop up for every action, like add item to bag, remove etc.

static > "base.css", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/d2c3833acdc59434e38f2ba03b4b5135377f89f4/static/css/base.css)

templates > includes > toasts > "toast_success.html", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/d2c3833acdc59434e38f2ba03b4b5135377f89f4/templates/includes/toasts/toast_success.html)

git add .
git commit -m "Added notification Css, shopping bag preview, and additional messages"
git push


THE CHECKOUT APP
`python3 manage.py startapp checkout`
boutique_ado > "settings.py", add `'checkout',` to the bottom of the list of "INSTALLED_APPS"
checkoout > "models.py", update it ot be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/faaa31bcbd1bcc5fdf7d54c57d51c031df9d7460/checkout/models.py)

`python3 manage.py makemigrations --dry-run`
`python3 manage.py makemigrations`
`python3 manage.py migrate --plan`
`python3 manage.py migrate`

git add .
git commit -m "Created checkout app and modules"
git push

checkoout > "admin.py", update it ot be:
```
from django.contrib import admin
from .models import Order, OrderLineItem


class OrderLineItemAdminInline(admin.TabularInline):
    model = OrderLineItem
    readonly_fields = ('lineitem_total',)


class OrderAdmin(admin.ModelAdmin):
    inlines = (OrderLineItemAdminInline,)

    readonly_fields = ('order_number', 'date',
                       'delivery_cost', 'order_total',
                       'grand_total',)

    fields = ('order_number', 'date', 'full_name',
              'email', 'phone_number', 'country',
              'postcode', 'town_or_city', 'street_address1',
              'street_address2', 'county', 'delivery_cost',
              'order_total', 'grand_total',)

    list_display = ('order_number', 'date', 'full_name',
                    'order_total', 'delivery_cost',
                    'grand_total',)

    ordering = ('-date',)


admin.site.register(Order, OrderAdmin)
```

`python3 manage.py runserver` /admin

admin homepage > Orders > Add Order button > you've got control over stuff here.

SIGNALS (built-in django feature)

create new file named "signals.py" in the checkout app folder and add:
```
from django.db.models.signals import post_save, post_delete
from django.dispatch import receiver

from .models import OrderLineItem

@receiver(post_save, sender=OrderLineItem)
def update_on_save(sender, instance, created, **kwargs):
    """
    Update order total on lineitem update/create
    """
    instance.order.update_total()

@receiver(post_delete, sender=OrderLineItem)
def update_on_save(sender, instance, **kwargs):
    """
    Update order total on lineitem delete
    """
    instance.order.update_total()
```

checkout > "apps.py", update it to be:
```
from django.apps import AppConfig


class CheckoutConfig(AppConfig):
    name = 'checkout'

    def ready(self):
        import checkout.signals
```

in checkout app, create file "forms.py", add:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/eee6f2bc60385e58ab27b0732fba3e6a22a2c209/checkout/forms.py)

git add .
git commit -m "Added forms, admin and signals"
git push

checkout > "views.py", update it to be:
```
from django.shortcuts import render, redirect, reverse
from django.contrib import messages

from .forms import OrderForm


def checkout(request):
    bag = request.session.get('bag', {})
    if not bag:
        messages.error(request, "There's nothing in your bag at the moment")
        return redirect(reverse('products'))

    order_form = OrderForm()
    template = 'checkout/checkout.html'
    context = {
        'order_form': order_form,
    }

    return render(request, template, context)
```

create a new "urls.py" file in the checkout app. add:
```
from django.urls import path
from . import views

urlpatterns = [
    path('', views.checkout, name='checkout')
]
```
boutique-ado > "urls.py", add `path('checkout/', include('checkout.urls')),` to the 'urlpatterns'.

create new folder "templates" in checkout app, then "checkout" folder within that, then "checkout.html" within that
add the following to it:
```
{% extends "base.html" %}
{% load static %}

{% block extra_css %}
    <link rel="stylesheet" href="{% static 'checkout/css/checkout.css' %}">
{% endblock %}

{% block page_header %}
    <div class="container header-container">
        <div class="row">
            <div class="col"></div>
        </div>
    </div>
{% endblock %}

{% block content %}
    <div class="overlay"></div>
    <div class="container">
        <div class="row">
            <div class="col">
                <hr>
                <h2 class="logo-font mb-4">Shopping Bag</h2>
                <hr>
            </div>
        </div>

        <div class="row">
            <div class="col-12 col-lg-6">
                <p class="text-muted">Please fill out the form below to complete your order</p>
            </div>
        </div>
    </div>
{% endblock %}
```

create a new folder named "static" within the checkout app, a "checkout" folder within the newly created "static"
folder, then a "css" folder within that and finally a file named "checkout.css".

"Now that the basics of the template are done, we'll need to install a new package called django-crispy-forms
which will let us format all our forms using bootstrap styling automatically."

CLI: `pip3 install django-crispy-forms`

boutique_ado > "settings.py", under 'INSTALLED_APPS', add the following to the list:
```
    # Other
    'crispy_forms',
```
and between 'ROOT_URLCONF' and 'TEMPLATES', add: `CRISPY_TEMPLATE_PACK = 'bootstrap4'`
and update 'TEMPLATES' to be:
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
                'django.template.context_processors.media',
                'bag.contexts.bag_contents',
            ],
            'builtins': [
                'crispy_forms.templatetags.crispy_forms_tags',
                'crispy_forms.templatetags.crispy_forms_field',
            ]
        },
    },
]
```
`pip3 freeze > requirements.txt`

checkout > templates > checkout > "checkout.html", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/8486523459273dddf96932a4ae19dd9a83af679d/checkout/templates/checkout/checkout.html)


bag > templates > bag > "bag.html", add `{% url 'checkout' %}` to the "Secure Checkout" section,
within the empty href tag, underneath "Keep Shopping"

python3 manage.py runserver

git add .
git commit -m "Added checkout views and templates"
git push


STRIPE - create an account at (https://stripe.com/).
once logged in, copy the test API key/Seceet key info.

follow the guide for setting up Stripe Elements (https://stripe.com/docs/payments/quickstart), but as of right now,
you just need to add:
```
<!-- Stripe -->
<script src="https://js.stripe.com/v3/"></script>
```
to the bottom of the 'corejs block' in the templates > "base.html" file.

checkout > templates > checkout > "checkout.html" add the following to the bottom:
```
{% block postloadjs %}
    {{ block.super }}
    {{ stripe_public_key|json_script:"id_stripe_public_key" }}
    {{ client_secret|json_script:"id_client_secret" }}
    <script src="{% static 'checkout/js/stripe_elements.js' %}"></script>
{% endblock %}
```

go to checkout > "views.py" and update the checkout view to be:
```
def checkout(request):
    bag = request.session.get('bag', {})
    if not bag:
        messages.error(request, "There's nothing in your bag at the moment")
        return redirect(reverse('products'))

    order_form = OrderForm()
    template = 'checkout/checkout.html'
    context = {
        'order_form': order_form,
        'stripe_public_key': 'pk_test_0SMREd7Vdweb1MGRi8S0EycR00JVzSAs5O',
        'client_secret': 'test client secret',
    }

    return render(request, template, context)
```
create a "js" folder in checkout > static > checkout. Then "stripe_elements.js" file within it. fill it with:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/a75791ce63bfce9a05f614d7712199c893063ed9/checkout/static/checkout/js/stripe_elements.js)

checkout > static > checkout > css > "checkout.css", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/a75791ce63bfce9a05f614d7712199c893063ed9/checkout/static/checkout/css/checkout.css)

git add .
git commit -m "Added stripe elements"
git push


checkout > static > checkout > js > "stripe_elements.js", add this to the bottom of the current code:
```
// Handle realtime validation errors on the card element
card.addEventListener('change', function (event) {
    var errorDiv = document.getElementById('card-errors');
    if (event.error) {
        var html = `
            <span class="icon" role="alert">
                <i class="fas fa-times"></i>
            </span>
            <span>${event.error.message}</span>
        `;
        $(errorDiv).html(html);
    } else {
        errorDiv.textContent = '';
    }
});
```

checkout > "views.py", add `from bag.contexts import bag_contents`, under `from .forms import OrderForm`
and:
```
    current_bag = bag_contents(request)
    total = current_bag['grand_total']
    stripe_total = round(total * 100)
```
underneath the 'if not' statement, and above the order_form, to emulate this (but dont copy+paste yet)

`pip3 install stripe`
checkout > "views.py", update the imports at the top to now be:
```
from django.shortcuts import render, redirect, reverse
from django.contrib import messages
from django.conf import settings

from .forms import OrderForm
from bag.contexts import bag_contents

import stripe
```

boutique_ado > "settings.py", towards the bottom, add: `# Stripe` as the comment on top of 
```
FREE_DELIVERY_THRESHOLD = 50
STANDARD_DELIVERY_PERCENTAGE = 10
```
then add these underneath to that 'Stripe' list
```
STRIPE_CURRENCY = 'usd'
STRIPE_PUBLIC_KEY = os.getenv('STRIPE_PUBLIC_KEY', '')
STRIPE_SECRET_KEY = os.getenv('STRIPE_SECRET_KEY', '')
```

CLI: `export STRIPE_PUBLIC_KEY=<public key from stripe website>`
CLI: `export STRIPE_SECRET_KEY=<secret key from stripe website>`

"If you are not using gitpod you may need to look up instructions on setting environment variables
for your specific situation. Additionally, this will not be permanent. And you'll have to re-export them 
each time you start your workspace. If you're using gitpod you can make them permanent by going to your 
main workspaces page (gitpod page), clicking your account icon in the upper right corner. And going to settings
And entering them there under the 'Variables' section."

name: STRIPE_PUBLIC_KEY
value: public key from stripe website 
scope: specific project or all projects

name: STRIPE_SECRET_KEY
value: secret key from stripe website 
scope: specific project or all projects

checkout > "views.py", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/6b3837fb56fdb60655292badbb2dcf649a074ec7/checkout/views.py)

checkout > static > checkout > js > "stripe_elements.js", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/6b3837fb56fdb60655292badbb2dcf649a074ec7/checkout/static/checkout/js/stripe_elements.js)

python3 manage.py runserver

go to checkout, fill in form with fake details and "4242...etc" for the card detail.
go to Stripe's website. click on "Developers" at top right, "Events" on left menu, then it'll show the list
of events. Should have succeeeded there.

git add .
git commit -m "Added basic checkout functionality"
git push

checkout > "views.py", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/f5ff37a673daa3b9e04fbfade263316198fd96c9/checkout/views.py)

checkout > "urls.py", update "urlpatterns" to be:
```
urlpatterns = [
    path('', views.checkout, name='checkout'),
    path('checkout_success/<order_number>', views.checkout_success, name='checkout_success'),
]
```

create "checkout_success.html" in the checkout > templates > checkout folder, and add:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/f5ff37a673daa3b9e04fbfade263316198fd96c9/checkout/templates/checkout/checkout_success.html)

in checkout > "`__init__.py`" add: `default_app_config = 'checkout.apps.CheckoutConfig'`

checkout > "models.py", add ` or 0` to the end of:
`self.order_total = self.lineitems.aggregate(Sum('lineitem_total'))['lineitem_total__sum']`. check here:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/f5ff37a673daa3b9e04fbfade263316198fd96c9/checkout/models.py)

checkout > "signals.py", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/f5ff37a673daa3b9e04fbfade263316198fd96c9/checkout/signals.py)

git add .
git commit -m "Added checkout success logic"
git push

checkout > templates > checkout > "checkout_success.html":
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/4f4a39d898c2c347e0d0a0201e4c0d2d6ef1c500/checkout/templates/checkout/checkout_success.html)
then make sure ~line65:
`{{ item.product.name }}{% if item.product_size %} - Size {{ item.product.size|upper }}{% endif %}`
is updated to be:
`{{ item.product.name }}{% if item.product_size %} - Size {{ item.product_size|upper }}{% endif %}`
and ~line104:
`<p class="mb-0">{{ order.street_address1 }}</p>`
to be:
`<p class="mb-0">{{ order.street_address2 }}</p>`

run app. test, order, checkout. should list an order confirmation.

git add .
git commit -m "Added order summary to checkout success page"
git push

checkout > templates > checkout > "checkout.html", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/4f4a39d898c2c347e0d0a0201e4c0d2d6ef1c500/checkout/templates/checkout/checkout.html)
checkout > static > checkout > css > "checkout.css", add the following to the bottom:
```
#loading-overlay {
	display: none;
	position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(23, 162, 184, .85);
    z-index: 9999;
}

.loading-spinner {
	display: flex;
    align-items: center;
    justify-content: center;
    margin: 0;
    height: 100%;
}
```
checkout > static > checkout > js > "stripe_elements.js", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/4f4a39d898c2c347e0d0a0201e4c0d2d6ef1c500/checkout/static/checkout/js/stripe_elements.js)

test again, go to checkout and use stripes extra authentication card no:
"4000002500003155", should load up the popup with loading animation and overlay

git add .
git commit -m "Added loading overlay"
git push

create file "webhook_handler.py" in checkout folder, then add this to it:
```
from django.http import HttpResponse


class StripeWH_Handler:
    """Handle Stripe webhooks"""

    def __init__(self, request):
        self.request = request

    def handle_event(self, event):
        """
        Handle a generic/unknown/unexpected webhook event
        """
        return HttpResponse(
            content=f'Webhook received: {event["type"]}',
            status=200)
```

git add .
git commit -m "Added webhook handler"
git push

checkout > "webhook_handler.py", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/cdf3e76a67d03b6ed0e59d903869f04a0e1c4bb5/checkout/webhook_handler.py)

checkout > "urls.py", update it to be:
```
from django.urls import path
from . import views
from .webhooks import webhook

urlpatterns = [
    path('', views.checkout, name='checkout'),
    path('checkout_success/<order_number>', views.checkout_success, name='checkout_success'),
    path('wh/', webhook, name='webhook'),
]
```

create "webhooks.py" file in checkout folder, update it to be:
```
from django.conf import settings
from django.http import HttpResponse
from django.views.decorators.http import require_POST
from django.views.decorators.csrf import csrf_exempt

from checkout.webhook_handler import StripeWH_Handler

import stripe

@require_POST
@csrf_exempt
def webhook(request):
    """Listen for webhooks from Stripe"""
    # Setup
    wh_secret = settings.STRIPE_WH_SECRET
    stripe.api_key = settings.STRIPE_SECRET_KEY

    # Get the webhook data and verify its signature
    payload = request.body
    sig_header = request.META['HTTP_STRIPE_SIGNATURE']
    event = None

    try:
        event = stripe.Webhook.construct_event(
        payload, sig_header, wh_secret
        )
    except ValueError as e:
        # Invalid payload
        return HttpResponse(status=400)
    except stripe.error.SignatureVerificationError as e:
        # Invalid signature
        return HttpResponse(status=400)
    except Exception as e:
        return HttpResponse(content=e, status=400)

    print('Success!')
    return HttpResponse(status=200)
```

go to boutique_ado > "settings.py":
add `STRIPE_WH_SECRET = os.getenv('STRIPE_WH_SECRET', '')` to the bottom of the list of Stripe environment variables.


`python3 manage.py runserver`
copy the website url from the address bar
go to Stripe's website > Developers (top-right) > Webhooks (left menu) > Add an Endpoint button.
"Endpoint URL": paste_your_website_address_here/checkout/wh/
PREVIOUS ERROR, ALTHOUGH THE WEB ADDRESS/URL WAS CORRECT, IT WAS DIFFERENT WHEN COMPARING WH AND SERVER ADDRESS AT A LATER POINT.
( remeber, to try and do these steps if there's a ~400 error when testing, ignore for now (further info below)
    delete the current webhook off the Stripe website.
    "CLI: `export STRIPE_PUBLIC_KEY=<public key from stripe website>`
    CLI: `export STRIPE_SECRET_KEY=<secret key from stripe website>`
    go to gitpod CLI: `export STRIPE_WH_SECRET=paste_your_signing_secret_here`"
)
ALSO, MAKE SURE THE GITPOD WORKSPACE IS SHARED (BOTTOM, WHEN SHARING E.G. FOR OTHERS TO LOOK AT YOUR GITPOD)
Select events then tick the box for Select all events, then Add events button. Add entpoint button.
"Reveal" the 'Signing secret', and copy it.

go to gitpod CLI: `export STRIPE_WH_SECRET=paste_your_signing_secret_here`
`python3 manage.py runserver`

checkout > "webhooks.py", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/cdf3e76a67d03b6ed0e59d903869f04a0e1c4bb5/checkout/webhooks.py)

git add .
git commit -m "Added class methods to webhook handler and webhook view"
git push

 checkout > static > checkout > js > "stripe_elements.js", update it to be:
 ```
/*
    Core logic/payment flow for this comes from here:
    https://stripe.com/docs/payments/accept-a-payment
    CSS from here: 
    https://stripe.com/docs/stripe-js
*/

var stripePublicKey = $('#id_stripe_public_key').text().slice(1, -1);
var clientSecret = $('#id_client_secret').text().slice(1, -1);
var stripe = Stripe(stripePublicKey);
var elements = stripe.elements();
var style = {
    base: {
        color: '#000',
        fontFamily: '"Helvetica Neue", Helvetica, sans-serif',
        fontSmoothing: 'antialiased',
        fontSize: '16px',
        '::placeholder': {
            color: '#aab7c4'
        }
    },
    invalid: {
        color: '#dc3545',
        iconColor: '#dc3545'
    }
};
var card = elements.create('card', {style: style});
card.mount('#card-element');

// Handle realtime validation errors on the card element
card.addEventListener('change', function (event) {
    var errorDiv = document.getElementById('card-errors');
    if (event.error) {
        var html = `
            <span class="icon" role="alert">
                <i class="fas fa-times"></i>
            </span>
            <span>${event.error.message}</span>
        `;
        $(errorDiv).html(html);
    } else {
        errorDiv.textContent = '';
    }
});

// Handle form submit
var form = document.getElementById('payment-form');

form.addEventListener('submit', function(ev) {
    ev.preventDefault();
    card.update({ 'disabled': true});
    $('#submit-button').attr('disabled', true);
    $('#payment-form').fadeToggle(100);
    $('#loading-overlay').fadeToggle(100);
    stripe.confirmCardPayment(clientSecret, {
        payment_method: {
            card: card,
            billing_details: {
                name: $.trim(form.full_name.value),
                phone: $.trim(form.phone_number.value),
                email: $.trim(form.email.value),
                address:{
                    line1: $.trim(form.street_address1.value),
                    line2: $.trim(form.street_address2.value),
                    city: $.trim(form.town_or_city.value),
                    country: $.trim(form.country.value),
                    state: $.trim(form.county.value),
                }
            }
        },
        shipping: {
            name: $.trim(form.full_name.value),
            phone: $.trim(form.phone_number.value),
            address: {
                line1: $.trim(form.street_address1.value),
                line2: $.trim(form.street_address2.value),
                city: $.trim(form.town_or_city.value),
                country: $.trim(form.country.value),
                postal_code: $.trim(form.postcode.value),
                state: $.trim(form.county.value),
            }
        },
    }).then(function(result) {
        if (result.error) {
            var errorDiv = document.getElementById('card-errors');
            var html = `
                <span class="icon" role="alert">
                <i class="fas fa-times"></i>
                </span>
                <span>${result.error.message}</span>`;
            $(errorDiv).html(html);
            $('#payment-form').fadeToggle(100);
            $('#loading-overlay').fadeToggle(100);
            card.update({ 'disabled': false});
            $('#submit-button').attr('disabled', false);
        } else {
            if (result.paymentIntent.status === 'succeeded') {
                form.submit();
            }
        }
    });
});
 ```

 checkout > "views.py", update it to be:
 (https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/06d30a8846a2867503bff714f796830caf46c3a0/checkout/views.py)

 checkout > "urls.py", update "urlpatterns" to be:
 ```
urlpatterns = [
    path('', views.checkout, name='checkout'),
    path('checkout_success/<order_number>', views.checkout_success, name='checkout_success'),
    path('cache_checkout_data/', views.cache_checkout_data, name='cache_checkout_data'),
    path('wh/', webhook, name='webhook'),
]
 ```

 checkout > static > checkout > js > "stripe_elements.js", update it to be:
 (https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/06d30a8846a2867503bff714f796830caf46c3a0/checkout/static/checkout/js/stripe_elements.js)

 checkout > "webhook_handler.py", update 'handle_payment_intent_succeeded' to be:
 ```
def handle_payment_intent_succeeded(self, event):
        """
        Handle the payment_intent.succeeded webhook from Stripe
        """
        intent = event.data.object
        print(intent)
        return HttpResponse(
            content=f'Webhook received: {event["type"]}',
            status=200)
 ```

 `python3 manage.py runserver`
order, checkout and go through all that process.
in Stripe, go to Developers > Webhooks, click on "payment_intent.succeed" and should list the info there??

git add .
git commit -m "Added checkout form data caching in payment intent"
git push

checkout > "webhook_handler.py", update 'handle_payment_intent_succeeded' to be:
```
def handle_payment_intent_succeeded(self, event):
        """
        Handle the payment_intent.succeeded webhook from Stripe
        """
        intent = event.data.object
        pid = intent.id
        bag = intent.metadata.bag
        save_info = intent.metadata.save_info

        billing_details = intent.charges.data[0].billing_details
        shipping_details = intent.shipping
        grand_total = round(intent.charges.data[0].amount / 100, 2)

        # Clean data in the shipping details
        for field, value in shipping_details.address.items():
            if value == "":
                shipping_details.address[field] = None

        order_exists = False
        attempt = 1
        while attempt <= 5:
            try:
                order = Order.objects.get(
                    full_name__iexact=shipping_details.name,
                    email__iexact=billing_details.email,
                    phone_number__iexact=shipping_details.phone,
                    country__iexact=shipping_details.address.country,
                    postcode__iexact=shipping_details.address.postal_code,
                    town_or_city__iexact=shipping_details.address.city,
                    street_address1__iexact=shipping_details.address.line1,
                    street_address2__iexact=shipping_details.address.line2,
                    county__iexact=shipping_details.address.state,
                    grand_total=grand_total,
                    original_bag=bag,
                    stripe_pid=pid,
                )
                order_exists = True
                break
            except Order.DoesNotExist:
                attempt += 1
                time.sleep(1)
        if order_exists:
            return HttpResponse(
                content=f'Webhook received: {event["type"]} | SUCCESS: Verified order already in database',
                status=200)
        else:
            order = None
            try:
                order = Order.objects.create(
                    full_name=shipping_details.name,
                    email=billing_details.email,
                    phone_number=shipping_details.phone,
                    country=shipping_details.address.country,
                    postcode=shipping_details.address.postal_code,
                    town_or_city=shipping_details.address.city,
                    street_address1=shipping_details.address.line1,
                    street_address2=shipping_details.address.line2,
                    county=shipping_details.address.state,
                    original_bag=bag,
                    stripe_pid=pid,
                )
                for item_id, item_data in json.loads(bag).items():
                    product = Product.objects.get(id=item_id)
                    if isinstance(item_data, int):
                        order_line_item = OrderLineItem(
                            order=order,
                            product=product,
                            quantity=item_data,
                        )
                        order_line_item.save()
                    else:
                        for size, quantity in item_data['items_by_size'].items():
                            order_line_item = OrderLineItem(
                                order=order,
                                product=product,
                                quantity=quantity,
                                product_size=size,
                            )
                            order_line_item.save()
            except Exception as e:
                if order:
                    order.delete()
                return HttpResponse(
                    content=f'Webhook received: {event["type"]} | ERROR: {e}',
                    status=500)
        return HttpResponse(
            content=f'Webhook received: {event["type"]} | SUCCESS: Created order in webhook',
            status=200)
```

checkout > "models.py", add the following two fields to the Order model:
```
original_bag = models.TextField(null=False, blank=False, default='')
stripe_pid = models.CharField(max_length=254, null=False, blank=False, default='')
```

`python3 manage.py makemigrations --dry-run`
`python3 manage.py makemigrations`
`python3 manage.py migrate --plan`
`python3 manage.py migrate`

checkout > "admin.py", add `'original_bag', 'stripe_pid'` to the "readonly_fields" and the "fields" area.

checkout > templates > checkout > "checkout.html", update the fieldset ~line104 to be:
```
<fieldset class="px-3">
    <legend class="fieldset-label small text-black px-2 w-auto">Payment</legend>
    <!-- A Stripe card element will go here -->
    <div class="mb-3" id="card-element"></div>
    <!-- Used to display form errors -->
    <div class="mb-3 text-danger" id="card-errors" role="alert"></div>
    <!-- Pass the client secret to the view so we can get the payment intent id -->
    <input type="hidden" value="{{ client_secret }}" name="client_secret">
</fieldset>
```

checkout > "views.py", update from ~line51 to line55
```
order = order_form.save(commit=False)
pid = request.POST.get('client_secret').split('_secret')[0]
order.stripe_pid = pid
order.original_bag = json.dumps(bag)
order.save()
```
check link below for reference only, don't copy and paste. just make the above the same:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/b5e178737596a1a1cf5be50345dc770b119918fd/checkout/views.py)

checkout > "webhook_handler.py", make sure:
```
original_bag=bag,
stripe_pid=pid,
```
are present on the bottom of the lists.
also, add these to the top:
```
from .models import Order, OrderLineItem
from products.models import Product

import stripe
import json
```

`python3 manage.py runserver`, test by going through checkout, using 4242...etc for card payment
check Stripe website > Developers > Webhooks > should Succeed there.

git add .
git commit -m "Completed webhook handler"
git push

checkout > templates > checkout > "checkout.html", swap the order of the delivery section to be:
```
                        {{ order_form.phone_number | as_crispy_field }}
                        {{ order_form.street_address1 | as_crispy_field }}
                        {{ order_form.street_address2 | as_crispy_field }}
                        {{ order_form.town_or_city | as_crispy_field }}
                        {{ order_form.county | as_crispy_field }}
                        {{ order_form.postcode | as_crispy_field }}
                        {{ order_form.country | as_crispy_field }}
```
checkout > "forms.py", update (in "placeholders") `'county': 'County',` to be `'county': 'County, State or Locality',`


"Please ensure you download the correct version of django-countries to follow this video.
Installing without a version specified, will install the latest version which is incompatible with
Python 3.8.12 (used by Gitpod).To ensure you can follow along without errors, please use the command
`pip3 install django-countries==7.2.1` when installing django-countries, instead of the command 
in the video (pip3 install django-countries). This is the latest stable version of the django-countries 
package."

`pip3 install django-countries==7.2.1`
`pip3 freeze > requirements.txt`

checkout > "models.py", add `from django_countries.fields import CountryField` to the top, between django.conf
and products.models, then updated the class Order `country = models.CharField(max_length=40, null=False, blank=False)`
to be `country = CountryField(blank_label='Country *', null=False, blank=False)` instead.

`python3 manage.py makemigrations --dry-run`
`python3 manage.py makemigrations`
`python3 manage.py migrate --plan`
`python3 manage.py migrate`

`python3 manage.py runserver`

checkout > static > checkout > css > "checkout.css", on top of the "#loading-overlay" class, add:
```
select,
select option {
    color: #000000;
}

select:invalid,
select option[value=""] {
    color: #aab7c4 !important;
}
```

checkout > "forms.py", remove `'country': 'Country',` from the placeholders dictionary, and update
the bottom section to be:
```
self.fields['full_name'].widget.attrs['autofocus'] = True
        for field in self.fields:
            if field != 'country':
                if self.fields[field].required:
                    placeholder = f'{placeholders[field]} *'
                else:
                    placeholder = placeholders[field]
                self.fields[field].widget.attrs['placeholder'] = placeholder
            self.fields[field].widget.attrs['class'] = 'stripe-style-input'
            self.fields[field].label = False
```

git add .
git commit -m "Updated checkout form"
git push


Profile App

`python3 manage.py startapp profiles`

boutique_ado > "settings.py", add `'profiles',` under `'checkout',` in the list of
"INSTALLED_APPS"

profiles > "models.py", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/95864bc75ef940aa002976163d7885612c6ca04b/profiles/models.py)
"And then creating a user profile model which has a one-to-one field attached to the user.
This is just like a foreign key except that it specifies that each user can only have one profile.
And each profile can only be attached to one user."

checkout > "models.py", add `from profiles.models import UserProfile` under `from products.models import Product`
then, add the following to the "Order" class/model, under the currently first "order_number" and above "full_name":
```
user_profile = models.ForeignKey(UserProfile, on_delete=models.SET_NULL,
                                     null=True, blank=True, related_name='orders')
```

`python3 manage.py makemigrations --dry-run`
`python3 manage.py makemigrations`
`python3 manage.py migrate --plan`
`python3 manage.py migrate`

profiles > "views.py", update it to be:
```
from django.shortcuts import render


def profile(request):
    """ Display the user's profile. """

    template = 'profiles/profile.html'
    context = {}

    return render(request, template, context)
```


in profiles, create a file named "urls.py". update it to be:
```
from django.urls import path
from . import views

urlpatterns = [
    path('', views.profile, name='profile')
]
```

boutique_ado > "urls.py", add `path('profile/', include('profiles.urls')),`
to the list, under "path('checkout/', include('checkout.urls')),"

in profiles app, create a file and name it "templates/profiles/profile.html". add:
```
{% extends "base.html" %}
{% load static %}

{% block extra_css %}
    <link rel="stylesheet" href="{% static 'profiles/css/profile.css' %}">
{% endblock %}

{% block page_header %}
    <div class="container header-container">
        <div class="row">
            <div class="col"></div>
        </div>
    </div>
{% endblock %}

{% block content %}
    <div class="overlay"></div>
    <div class="container">
        <div class="row">
            <div class="col">
                <hr>
                <h2 class="logo-font mb-4">My Profile</h2>
                <hr>
            </div>
        </div>
{% endblock %}
```

in profiles app, create a file and name it "static/profiles/css/profile.css". add:

`python3 manage.py runserver` and add /profile to address. Should load the basic profile page.

git add .
git commit -m "Add profile app"
git push


Update allauth templates

templates > allauth > account > "base.html", update it to be:
```
{% extends "base.html" %}

{% block content %}
    <div class="container header-container">
        <div class="overlay"></div>
        <div class="row">
            <div class="col-12 col-md-6">
                <div class="allauth-form-inner-content">
                    {% block inner_content %}
                    {% endblock %}
                </div>
            </div>
        </div>
    </div>
{% endblock %}
```

templates > allauth > account > "account_inactive.html", update to to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/master/templates/allauth/account/account_inactive.html)

templates > allauth > account > "email_confirm.html", update to to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/master/templates/allauth/account/email_confirm.html)

templates > allauth > account > "email.html", update to to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/master/templates/allauth/account/email.html)

templates > allauth > account > "login.html", update to to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/master/templates/allauth/account/login.html)

templates > allauth > account > "logout.html", update to to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/master/templates/allauth/account/logout.html)

templates > allauth > account > "password_change.html", update to to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/master/templates/allauth/account/password_change.html)

templates > allauth > account > "password_reset.html", update to to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/master/templates/allauth/account/password_reset.html)

templates > allauth > account > "login.html", update to to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/master/templates/allauth/account/password_reset_done.html)

templates > allauth > account > "password_reset_done.html", update to to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/master/templates/allauth/account/password_reset_done.html)

templates > allauth > account > "password_reset_from_key.html", update to to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/master/templates/allauth/account/password_reset_from_key.html)

templates > allauth > account > "password_reset_from_key_done.html", update to to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/master/templates/allauth/account/password_reset_from_key_done.html)

templates > allauth > account > "password_set.html", update to to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/master/templates/allauth/account/password_set.html)

templates > allauth > account > "signup.html", update to to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/master/templates/allauth/account/signup.html)

templates > allauth > account > "signup_closed.html", update to to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/master/templates/allauth/account/signup_closed.html)

templates > allauth > account > "verification_sent.html", update to to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/master/templates/allauth/account/verification_sent.html)

templates > allauth > account > "verified_email_required.html", update to to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/master/templates/allauth/account/verified_email_required.html)



static > css > "base.css", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/eb4e5fd74558387f25a740820235f33a33834066/static/css/base.css)


`python3 manage.py runserver`
try to login via website's 'My Account' > 'Login' (try the username you created on project startup), should give an error:
"RelatedObjectDoesNotExist at /accounts/login/   .User has no userprofile."
one way to fix it, in gitpod:
profiles > "models.py", update this area to be as below, commenting out sections.

```
# if created:
UserProfile.objects.create(user=instance)
# Existing users: just save the profile
# instance.userprofile.save()
```
go back to the website. log in again. no error should occurr.
Then, go back to profiles > "models.py" and change the previously commented out section back to how it was.
Repeat these 2 steps for every username created before arriving to this part of the project setup.

"Another way you could do this if you had many users who needed profiles created would be to do it through
the shell by getting all the users. And then creating a profile for them in a loop but since I just have 
my one admin user this works fine"

log out of website profile. Go to My account > Register. create a new username.
go back to gitpod console, it should log the email confirmation link there.
copy the link from "/accounts/confirm-email..." onwards. paste into the url of browser.
It will take you to the Confirm Email page. Click 'Confirm' button. Then log in.

templates > "base.html", update My Profile href link section ~line77 `<a href="" class="dropdown-item">My Profile</a>` to be:
`<a href="{% url 'profile' %}" class="dropdown-item">My Profile</a>`
profiles > "views.py", update it to be:
```
from django.shortcuts import render, get_object_or_404

from .models import UserProfile


def profile(request):
    """ Display the user's profile. """
    profile = get_object_or_404(UserProfile, user=request.user)

    template = 'profiles/profile.html'
    context = {
        'profile': profile,
    }

    return render(request, template, context)
```

profiles > templates > profiles > "profile.html", update the block content at the bottom to include profile,
so it is:
```
{% block content %}
    <div class="overlay"></div>
    <div class="container">
        <div class="row">
            <div class="col">
                <hr>
                <h2 class="logo-font mb-4">My Profile</h2>
                <hr>
            </div>
        </div>
        {{ profile }}
{% endblock %}
```
go to My Account > My Profile on website. should display username there.

git add .
git commit -m "Updated allauth templates and tested profiles"
git push


Build user profile form.

create "forms.py" file in profiles app, then add:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/149abda4318106704ac8d3118b8e5a63ef070619/profiles/forms.py)

profiles > "views.py". update it to be:
```
from django.shortcuts import render, get_object_or_404

from .models import UserProfile
from .forms import UserProfileForm


def profile(request):
    """ Display the user's profile. """
    profile = get_object_or_404(UserProfile, user=request.user)

    form = UserProfileForm(instance=profile)
    orders = profile.orders.all()

    template = 'profiles/profile.html'
    context = {
        'form': form,
        'orders': orders,
    }

    return render(request, template, context)
```

then, profiles > templates > profiles > "profile.html", update the "block content" at the bottom to be:
```
{% block content %}
    <div class="overlay"></div>
    <div class="container">
        <div class="row">
            <div class="col">
                <hr>
                <h2 class="logo-font mb-4">My Profile</h2>
                <hr>
            </div>
        </div>
        <div class="row">
            <div class="col-12 col-lg-6">
                <p class="text-muted">Default Delivery Information</p>
                <form class="mt-3" action="{% url 'profile' %}" method="POST" id="profile-update-form">
                    {% csrf_token %}
                    {{ form|crispy }}
                    <button class="btn btn-black rounded-0 text-uppercase float-right">Update Information</button>
                </form>
            </div>
            <div class="col-12 col-lg-6">
                <p class="text-muted">Order History</p>
                {{ orders }}
            </div>
        </div>
{% endblock %}
```

`python3 manage.py runserver`, check the profile. some adjustments to be made.

profiles > "models.py", update the section to be (rearraged order and delete asterics from Country):
```
    user = models.OneToOneField(User, on_delete=models.CASCADE)
    default_phone_number = models.CharField(max_length=20, null=True, blank=True)
    default_street_address1 = models.CharField(max_length=80, null=True, blank=True)
    default_street_address2 = models.CharField(max_length=80, null=True, blank=True)
    default_town_or_city = models.CharField(max_length=40, null=True, blank=True)
    default_county = models.CharField(max_length=80, null=True, blank=True)
    default_postcode = models.CharField(max_length=20, null=True, blank=True)
    default_country = CountryField(blank_label='Country', null=True, blank=True)
```

profiles > static > profiles > css > "profile.css" and add:
```
#profile-update-form .form-control {
    color: #000;
}

#profile-update-form input::placeholder {
    color: #aab7c4;
}

#id_default_country,
#id_default_country option:not(:first-child) {
    color: #000;
}

#id_default_country option:first-child {
    color: #aab7c4;
}
```

profiles > templates > profiles > "profile.html", add this to the bottom, under block content:
```
{% block postloadjs %}
    {{ block.super }}
    <script type="text/javascript" src="{% static 'profiles/js/countryfield.js' %}"></script>
{% endblock %}
```

create a new folder in profiles > static > profiles named "js", and file named "countryfield.js" in there.
Add the following to the js file:
```
let countrySelected = $('#id_default_country').val();
if(!countrySelected) {
    $('#id_default_country').css('color', '#aab7c4');
}
$('#id_default_country').change(function() {
    countrySelected = $(this).val();
    if(!countrySelected) {
        $(this).css('color', '#aab7c4');
    } else {
        $(this).css('color', '#000');
    }
});
```

profiles > "views.py", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/149abda4318106704ac8d3118b8e5a63ef070619/profiles/views.py)

templates > includes > toasts > "toast_success.html", update ~line17 the line `{% if grand_total %}` to be:
`{% if grand_total and not on_profile_page %}`


git add .
git commit -m "Added profile form, views and updated template"
git push


Add Order Histroy to user Profile

profiles > templates > profiles > "profile.htnml", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/3196f480e8e7c22c8f6bf3badc351616268cd365/profiles/templates/profiles/profile.html)

profiles > static > profiles > css > "profile.css", add this to the top of the styling:
```
.order-history {
    max-height: 416px; /* height of profile form + submit button */
    overflow-y: auto;
}
```

profiles > "views.py", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/3196f480e8e7c22c8f6bf3badc351616268cd365/profiles/views.py)

profiles > "urls.py", update it to be:
```
from django.urls import path
from . import views

urlpatterns = [
    path('', views.profile, name='profile'),
    path('order_history/<order_number>', views.order_history, name='order_history'),
]
```

checkout > templates > checkout > "checkout_success.html", update the final div/row to be:
```
        <div class="row">
			<div class="col-12 col-lg-7 text-right">
                {% if from_profile %}
                <a href="{% url 'profile' %}" class="btn btn-black rounded-0 my-2">
					<span class="icon mr-2">
						<i class="fas fa-angle-left"></i>
					</span>
					<span class="text-uppercase">Back to Profile</span>
				</a>
                {% else %}
				<a href="{% url 'products' %}?category=new_arrivals,deals,clearance" class="btn btn-black rounded-0 my-2">
					<span class="icon mr-2">
						<i class="fas fa-gifts"></i>
					</span>
					<span class="text-uppercase">Now check out the latest deals!</span>
				</a>
                {% endif %}
			</div>
		</div>
```
see this for reference, at the bottom:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/3196f480e8e7c22c8f6bf3badc351616268cd365/checkout/templates/checkout/checkout_success.html)

checkout > "admin.py" > add `'user_profile',` inside the 'OrderAdmin' > 'fields' section, before 'date
and after 'order_number'.

checkout > "views.py", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/be009dd6c8db8c9cbc18aa5b8dd8a04daa194ed0/checkout/views.py#L143)

`python3 manage.py runserver`
Clear your delivery info in your My Profile page. Save the clear form. Go through ordering an item.
Checkout and place order. When successful, it should list that order in the 'Order Histroy' section
in My Profile and your 'Default Delivery Information' should be populated with what you entered during
checkout.
Click on Order Histroy, Order Number link. It should take to order_history page.

checkout > "views.py", in the checkout view, update it to be:
(do not copy and paste entire document. just the checkout view section, as there are errors elsewhere)
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/3196f480e8e7c22c8f6bf3badc351616268cd365/checkout/views.py)

`python3 manage.py runserver` and test by trying to buy another item. the form at checkout should automatically
be filled in. if 'Full Name' and/or 'Email address' aren't filled in, update in /admin, Users section (url in browser)

git add .
git commit -m "Profiles - added order history and updated checkout views"
git push


checkout > "webhook_handler.py", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/be009dd6c8db8c9cbc18aa5b8dd8a04daa194ed0/checkout/webhook_handler.py)

to test this, go to:
checkout > static > checkout > js > "stripe_elements.js" and comment out ~line110 `form.save()`, comment it out.
run server, try to order and go through checkout. it should fail to load the screen but still submit the form/order.
check in /admin for that order, and it should also be displayed on My Profie, Order History.
checkout > static > checkout > js > "stripe_elements.js" and re-enable ~line110 `form.save()` so it's back to being active.

git add .
git commit -m "Updated webbook handler to handle profiles"
git push


Finalize payment system, by sending confirmation email

checkout > templates > checkout, create a folder named "confirmation_emails"
in "confirmation_emails" folder, create two files named "confirmation_email_subject.txt" and "confirmation_email_body.txt"

in "confirmation_email_subject.txt", add: `Boutique Ado Confirmation for Order Number {{ order.order_number }}`
in "confirmation_email_body.txt", add:
```
Hello {{ order.full_name }}!

This is a confirmation of your order at Boutique Ado. Your order information is below:

Order Number: {{ order.order_number }}
Order Date: {{ order.date }}

Order Total: ${{ order.order_total }}
Delivery: ${{ order.delivery_cost }}
Grand Total: ${{ order.grand_total }}

Your order will be shipped to {{ order.street_address1 }} in {{ order.town_or_city }}, {{ order.country }}.

We've got your phone number on file as {{ order.phone_number }}.

If you have any questions, feel free to contact us at {{ contact_email }}.

Thank you for your order!

Sincerely,

Boutique Ado
```

checkout > "webhook_handler.py", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/f6c3de32aa152b98da174daba13412388258b9b8/checkout/webhook_handler.py)

boutique_ado > "settings.py", to the "Stripe" section, add the following:
`DEFAULT_FROM_EMAIL = 'boutiqueado@example.com'`

test by running the website, placing an order. The test email should be back in gitpods terminal/console.

git add .
git commit -m "Completed confirmation emails"
git push


products > create file named "forms.py" and add:
```
from django import forms
from .models import Product, Category


class ProductForm(forms.ModelForm):

    class Meta:
        model = Product
        fields = '__all__'

    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        categories = Category.objects.all()
        friendly_names = [(c.id, c.get_friendly_name()) for c in categories]

        self.fields['category'].choices = friendly_names
        for field_name, field in self.fields.items():
            field.widget.attrs['class'] = 'border-black rounded-0'
```

git add .
git commit -m "Add product form"
git push

products > "views.py", add `from .forms import ProductForm` to the top along with other imports,
and this to the bottom:
```
def add_product(request):
    """ Add a product to the store """
    form = ProductForm()
    template = 'products/add_product.html'
    context = {
        'form': form,
    }

    return render(request, template, context)
```

products > "urls.py", add `path('add/', views.add_product, name='add_product'),` to the urlpatterns,
and update the product_detail url to be `path('<int:product_id>/', views.product_detail, name='product_detail'),`

products > templates > products, create a file named "add_product.html" and add to it:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/90bfac3ad4932550b4457a698f7df8ab79a681bc/products/templates/products/add_product.html)

`python3 manage.py runserver` and then /products/add/ in the browser to check.

git add .
git commit -m "Add product view, url and template"
git push

products > "views.py", update the "add_product" to be:
```
def add_product(request):
    """ Add a product to the store """
    if request.method == 'POST':
        form = ProductForm(request.POST, request.FILES)
        if form.is_valid():
            form.save()
            messages.success(request, 'Successfully added product!')
            return redirect(reverse('add_product'))
        else:
            messages.error(request, 'Failed to add product. Please ensure the form is valid.')
    else:
        form = ProductForm()
        
    template = 'products/add_product.html'
    context = {
        'form': form,
    }

    return render(request, template, context)
```

profiles > "views.py", "profile" to be:
```
def profile(request):
    """ Display the user's profile. """
    profile = get_object_or_404(UserProfile, user=request.user)

    if request.method == 'POST':
        form = UserProfileForm(request.POST, instance=profile)
        if form.is_valid():
            form.save()
            messages.success(request, 'Profile updated successfully')
        else:
            messages.error(request, 'Update failed. Please ensure the form is valid.')
    else:
        form = UserProfileForm(instance=profile)
    orders = profile.orders.all()

    template = 'profiles/profile.html'
    context = {
        'form': form,
        'orders': orders,
        'on_profile_page': True
    }

    return render(request, template, context)
```

`python3 manage.py runserver` and then /products/add/ in the browser to check.
add a product to the website. search for it. then try clicking on Add to Bag. error should occur.
to fix:

templates > includes > toasts > "toast_success.html", update the content within the `<div class="col-3 my-1">`
~line22 to be: 
```
                            {% if item.product.image %}
                            <img class="w-100" src="{{ item.product.image.url }}" alt="{{ item.product.name }}">
                            {% else %}
                            <img class="w-100" src="{{ MEDIA_URL }}noimage.png" alt="{{ item.product.name }}">
                            {% endif %}
```

bag > templates > bag > "bag.html", update the content within the `<td class="p-3 w-25">` ~line41 to be:
```
                                        {% if item.product.image %}
                                        <img class="img-fluid rounded" src="{{ item.product.image.url }}" alt="{{ item.product.name }}">
                                        {% else %}
                                        <img class="img-fluid rounded" src="{{ MEDIA_URL }}noimage.png" alt="{{ item.product.name }}">
                                        {% endif %}
```

templates > "base.html", ~line75, update the line `<a href="" class="dropdown-item">Product Management</a>`,
to be `<a href="{% url 'add_product' %}" class="dropdown-item">Product Management</a>`

templates > includes > "mobile-top-header.html",  ~line38, update the Product Management line
`<a href="{% url 'account_logout' %}" class="dropdown-item">Product Management</a>`, to be
`<a href="{% url 'add_product' %}" class="dropdown-item">Product Management</a>`

`python3 manage.py runserver` and then /products/add/ in the browser. add a product to the website.
add an image to the product. search for it. then Add to Bag. should work.
delete the newly made products from the /admin > Products etc.
(if it errors, clear website data and restart server)
delete image from media (in gitpod) too.

git add .
git commit -m "Finished add product functionality"
git push


EDIT PRODUCTS

products > templates > products > create a new file named "edit_product.html", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/de5e400ea7efd03403e289435b2b821fd6f5748a/products/templates/products/edit_product.html)

products > "views.py" and add the following to the bottom:
```
def edit_product(request, product_id):
    """ Edit a product in the store """
    product = get_object_or_404(Product, pk=product_id)
    if request.method == 'POST':
        form = ProductForm(request.POST, request.FILES, instance=product)
        if form.is_valid():
            form.save()
            messages.success(request, 'Successfully updated product!')
            return redirect(reverse('product_detail', args=[product.id]))
        else:
            messages.error(request, 'Failed to update product. Please ensure the form is valid.')
    else:
        form = ProductForm(instance=product)
        messages.info(request, f'You are editing {product.name}')

    template = 'products/edit_product.html'
    context = {
        'form': form,
        'product': product,
    }

    return render(request, template, context)
```

products > "urls.py", add: `path('edit/<int:product_id>/', views.edit_product, name='edit_product'),` to the urlspatterns.

`python3 manage.py runserver` and then /products/edit/1 in the browser. alter the form and 'Update Product'.
Should work.

git add .
git commit -m "Add ability to edit products"
git push


EDIT PRODUCTS
products > "urls.py", add: `path('delete/<int:product_id>/', views.delete_product, name='delete_product'),` to the urlspatterns.

products > "views.py" and add the following to the bottom:
```
def delete_product(request, product_id):
    """ Delete a product from the store """
    product = get_object_or_404(Product, pk=product_id)
    product.delete()
    messages.success(request, 'Product deleted!')
    return redirect(reverse('products'))
```
and update the add_product to be (so there's no need to search for a newly created product):
```
def add_product(request):
    """ Add a product to the store """
    if request.method == 'POST':
        form = ProductForm(request.POST, request.FILES)
        if form.is_valid():
            product = form.save()
            messages.success(request, 'Successfully added product!')
            return redirect(reverse('product_detail', args=[product.id]))
        else:
            messages.error(request, 'Failed to add product. Please ensure the form is valid.')
    else:
        form = ProductForm()
        
    template = 'products/add_product.html'
    context = {
        'form': form,
    }

    return render(request, template, context)
```
to test Delete:
`python3 manage.py runserver` and then 'Product Management'. Add a new product. Take note of the product id
(number) in the url when creating the new product (e.g. .../product/177/). then, change the url to be
.../product/delete/177/ . Once Enter is pressed, a message should confirm deletion.

Adding Edit and Delete links:
products > templates > products > "products.html" and add the following under ~line84(`{% endif %}`), so, still
within the div col:
```
                                            {% if request.user.is_superuser %}
                                                <small class="ml-3">
                                                    <a href="{% url 'edit_product' product.id %}">Edit</a> | 
                                                    <a class="text-danger" href="{% url 'delete_product' product.id %}">Delete</a>
                                                </small>
                                            {% endif %}
```
so it should look like (dont copy and paste whole doc):
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/c71609890aea8a7c4e534e25d1906cc4111d7725/products/templates/products/products.html)

products > templates > products > "product_detail.html" and add the following under ~line44(`{% endif %}`) and above
`<p class="mt-3">{{ product.description }}</p>`:
```
                    {% if request.user.is_superuser %}
                        <small class="ml-3">
                            <a href="{% url 'edit_product' product.id %}">Edit</a> | 
                            <a class="text-danger" href="{% url 'delete_product' product.id %}">Delete</a>
                        </small>
                    {% endif %}
```
so it should look like (dont copy and paste whole doc):
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/c71609890aea8a7c4e534e25d1906cc4111d7725/products/templates/products/product_detail.html)

`python3 manage.py runserver` and then Edit/Delete should be under all products. Don't test on current products.
Create a new product. then Edit/Delete link, as there is no defensive programming in place.

git add .
git commit -m "Add ability to edit products"
git push


Only allow superusers to edit/delete (securing the views)
products > "views.py", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/e439fefda305c7b21644b59d470dc310b5b27797/products/views.py)

profiles > "views.py", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/e439fefda305c7b21644b59d470dc310b5b27797/profiles/views.py)

git add .
git commit -m "Securing the views"
git push


Style the Image area on the Add/Edit product form.
"So Django forms work with what are called widgets and this little widget that handles the image field 
on our model is called a clearable file input. Conveniently because Django is just Python we can 
inherit this class and customize it."

products > create a new file named "widgets.py", then fill it with:
```
from django.forms.widgets import ClearableFileInput
from django.utils.translation import gettext_lazy as _


class CustomClearableFileInput(ClearableFileInput):
    clear_checkbox_label = _('Remove')
    initial_text = _('Current Image')
    input_text = _('')
    template_name = 'products/custom_widget_templates/custom_clearable_file_input.html'
```

products > templates > products, create a file named "custom_widget_templates/custom_clearable_file_input.html"
in "custom_clearable_file_input.html", using the following as a guide:
(https://github.com/django/django/blob/main/django/forms/templates/django/forms/widgets/clearable_file_input.html)
so it eventually ends up like this (just copy+paste this, but use above link as reading for django base):
(https://github.com/ckz8780/boutique_ado_v1/blob/bef1c8f38ea2774f75b18c15d5927b5409083cf9/products/templates/products/custom_widget_templates/custom_clearable_file_input.html)

products > "forms.py" and update it to be:
(https://github.com/ckz8780/boutique_ado_v1/blob/bef1c8f38ea2774f75b18c15d5927b5409083cf9/products/forms.py)

git add .
git commit -m "Fixing the image field"
git push

static > css > "base.css", paste the following above the "fixed top navbar only on medium and up" styles
```
/* Product Form */

.btn-file {
    position: relative;
    overflow: hidden;
}

.btn-file input[type="file"] {
    position: absolute;
    top: 0;
    right: 0;
    min-width: 100%;
    min-height: 100%;
    opacity: 0;
    cursor: pointer;
}

.custom-checkbox .custom-control-label::before {
    border-radius: 0;
    border-color: #dc3545;
}

.custom-checkbox .custom-control-input:checked~.custom-control-label::before {
    background-color: #dc3545;
    border-color: #dc3545;
    border-radius: 0;
}
```

to remove the crispy forms "image" info and add js to improve image uploading msg:
products > templates > products > "edit_product.html", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/f5880efee43b3b9ea1276a09ca972f4588001c59/products/templates/products/edit_product.html)
and also,
products > templates > products > "add_product.html", update it to be:
(https://github.com/Code-Institute-Solutions/boutique_ado_v1/blob/f5880efee43b3b9ea1276a09ca972f4588001c59/products/templates/products/add_product.html)

run server, add a new product, add image, update it through Edit and remove image, update. then delete product. should all work.

git add .
git commit -m "Fixing the image field further"
git push


Creating a Heroku App

Log In to heroku website, create new app, choose closest region (Europe for me), name the app.
Still in heroku website, go to 'Resources' tab, then in 'Add-ons', type postgres and select 'Heroku Postgres'
from the dropdown choices.
Plan name = 'Hobby Dev - Free', then 'Submit Order Form' button
Follow this:
"To use Postgres we need to go back to gitpod and install dj_database_url, and psycopg2.
So I'll do that now with `pip3 install dj_database_url`
And `pip3 install psycopg2-binary`
Now I can freeze the requirements with `pip3 freeze > requirements.txt`"

in gitpod, go to boutique-ado > "settings.py", add `import dj_database_url` to the bottom of the list of imports.
then, comment out the 'DATABASES' section, and add:
```
DATABASES = {
    'default': dj_database_url.parse()
}
```
"And give it the database URL from Heroku, which you can either get from your Config Variables in your app 'Settings' tab.
or from the command line by typing Heroku config." so that it looks like:
```
DATABASES = {
    'default': dj_database_url.parse('your_postgres_db_URL_here')
}
```
"With that saved, we're ready to connect to our new Heroku database and run migrations.
Remember that because we're connecting to Postgres now, we need to run all these migrations again and we can see that with the show
migrations command since none of them are marked off."

`python3 manage.py showmigrations`
`python3 manage.py migrate`

"Now to import all our product data, we can use our fixtures again by first loading in the categories and then the products.
To load the categories, I'll use python3 manage.py loaddata categories. And then do the same for products.
And remember it's important to do them in that order because the products depend on the categories already existing."
so,
`python3 manage.py loaddata categories`
`python3 manage.py loaddata products`

"Finally I'll just give myself a super user to log in with."
`python3 manage.py createsuperuser`

"Before I commit I'm gonna remove the Heroku database config. And uncomment the original so our database URL doesn't end up in version control."
So,
Un-comment the 'DATABASES' section in boutique_ado > "settings.py" that we commented out earlier. (make it active now)
and delete:
```
DATABASES = {
    'default': dj_database_url.parse()
}
```

git add .
git commit -m "Creating a Heroku App and setting up postgres db"
git push


DEPLOYING TO HEROKU

boutique_ado > "settings.py", update the 'DATABASES' section to be:
```
if 'DATABASE_URL' in os.environ:
    DATABASES = {
        'default': dj_database_url.parse(os.environ.get('DATABASE_URL'))
    }
else:
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.sqlite3',
            'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
        }
    }
```

`pip3 install gunicorn`
`pip3 freeze > requirements.txt`

create a file named "Procfile" (outside of all the apps. so, create it where requirements.txt, gitignore etc are)
and add this to it:
`web: gunicorn boutique_ado.wsgi:application` (no empty space below)

in CLI `heroku login -i`, and log in with heroku info
`heroku config:set DISABLE_COLLECTSTATIC=1` (if only one heroku app) OR
`heroku config:set DISABLE_COLLECTSTATIC=1 --app your-heroku-app-name-here` (if more than one heroku app on website)
e.g.
`heroku config:set DISABLE_COLLECTSTATIC=1 --app rhysmoggs-boutique-ado`, for this example

boutique_ado > "settings.py", update `ALLOWED_HOSTS = []` to be `ALLOWED_HOSTS = ['your-heroku-app-name-here.heroku.com', 'localhost']`
e.g.
`ALLOWED_HOSTS = ['rhysmoggs-boutique-ado.heroku.com', 'localhost']` for this example

`ACCOUNT_EMAIL_VERIFICATION = 'none'`

git add .
git commit -m "Deploy to Heroku"
git push

git push heroku main