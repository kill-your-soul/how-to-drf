# Django REST API
## Prerequisites
1. [Git](https://git-scm.com/)
2. [Python](https://python.org/)
3. pip or pip3
4. [poetry](https://python-poetry.org/)

## Init new project

#### Make new project irectory
```shell
mkdir project
cd project
```

#### Init new project
```shell
poetry init
```

#### Installing libraries
```shell
poetry add django django djangorestframework django-debug-toolbar
```

#### Init git repo
```
git init
```
1. For windows
	```powershell
	ni .gitignore
	```
2. For linux
	```bash
	touch .gitignore
	```

#### Adding common words to `.gitignore`
```.gitignore
.venv/
db.sqlite3
**/__pycache__/**
```

#### Create README.md
1. For windows
	```powershell
	ni README.md
	```
2. For linux
	```bash
	touch README.md
	```

#### Install pre-commit
```shell
poetry add pre-commit
```

#### Add config to pre-commit to `.pre-commit-config.yaml` 
I use black to autoformatting code before commit

```yaml
repos:
- repo: https://github.com/psf/black
  rev: 23.3.0
  hooks:
    - id: black
      language_version: python3.10
```

#### Install pre-commit

```shell
poetry run pre-commit install
```

## Django
#### Start Django app
Our core  project that contain all our settings and etc.
```shell
poetry run django-admin startproject config .
```

#### Adding app to our project
This app will contain all our api endpoints
```shell
poetry run django-admin startapp api
```
Delete all files from our api folder, except `tests.py` and create file `urls.py`. This file will contain our endpoints

#### Adding another app to our project

```shell
poetry run djangoadmin startapp example
```

#### Adding our apps to config
Open `settings.py` and change it like this

```python
...

INSTALLED_APPS = [
	"django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
]

INSTALLED_APPS += [
	"rest_framework",
	"example",
	"api",
	"debug_toolbar"
]

MIDDLEWARE = [
	... ,
	"debug_toolbar.middleware.DebugToolbarMiddleware",
]

INTERNAL_IPS = ["127.0.0.1"] # for debug_toolbar
```
You can also change project language by editing `settings.py`

```python
LANGUAGE_CODE = "ru-ru"
```

#### Security
From file `settings.py` remove `SECRET_KEY` variable and replace it with

```python
import os
...
SECRET_KEY = os.environ.get("SECRET_KEY")
```

#### Github Actions
ONLY if you are using github

Crete folder `.github\workflows` and crete file in it `django.yml`
```yml
name: Django CI

on:
  push:
    branches: [ "master" ]
  pull_request:
    branches: [ "master" ]
  
jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      max-parallel: 4
      matrix:
        python-version: ['3.9', '3.10']

    steps:
    - uses: actions/checkout@v3
    - name: Set up Python ${{ matrix.python-version }}
      uses: actions/setup-python@v4
      with:
        python-version: ${{ matrix.python-version }}

    - name: Install Dependencies
      run: |
        python -m pip install --upgrade pip
        pip install poetry
        poetry install

    - name: Run Tests
      run: |
        poetry python manage.py test
```

#### Save your requirements to file

```shell
poetry export -f requirements.txt --output requirements.txt --without-hashes
```

#### Make migrations to database

```shell
poetry run python manage.py makemigrations
poetry run python manage.py migrate 
```

#### Run development server

```shell
poetry run python manage.py runserver
```
