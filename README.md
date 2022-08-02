
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

