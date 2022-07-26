
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

