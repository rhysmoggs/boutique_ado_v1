

install django, but not the latest version `pip3 install 'django<4'` in CLI

create project in CLI still, `django-admin startproject boutique_ado .`
`touch .gitignore` to create this file, if not present already (it is in CI's template)
add to thie file:
```
*.sqlite3
*.pyc
__pycache__/
```
if not already in ".gitignore" file


