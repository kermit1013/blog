---
created: 2024-02-14T22:42
updated: 2024-02-20T00:33
title: Python these days
---
## install `python`
```shell
brew install python
```

## install `poetry`
```shell
// Ubuntu
curl -sSL https://install.python-poetry.org | python3 -

// OSX
curl -sSL https://install.python-poetry.org | sed 's/symlinks=False/symlinks=True/' | python3 -
```
  

##  alias `python3` to `python`
On macOS, there are several ways to alias `python3` to `python`. Here are the best options, each with its own pros and cons:

**1. Using `.bashrc` or `.zshrc` (Recommended for permanent alias):**

- Edit your shell configuration file:
    
    - Open Terminal and run `nano ~/.bashrc` (for Bash) or `nano ~/.zshrc` (for Zsh).
    
- Add the following line:

```shell
alias python="python3"
```

- Save the file and reload your shell configuration:
    
    - `source ~/.bashrc` (for Bash) or `source ~/.zshrc` (for Zsh).

> [!NOTE] Quick Tips
> ```
> echo 'alias python="python3"' >> ~/.bashrc



> [!NOTE] How to solve Pylance 'missing imports' in vscode
>In my case, the fastest solution when imports are not missing is to launch vscode from the virtual environment. Basically, activate the venv as always, and then `code .`


## Project initialize
![[Pasted image 20240220002837.png]]
### create project folder
> mkdir loudy-webserver
> cd loudy-webserver/
### poetry init

>poetry init
>poetry add Django
>poetry add djangorestframework

### entering virtual environment
>poetry shell

### folder of config
>poetry run django-admin startproject config .

### folder of apps
>mkdir apps 
>cd apps
>mkdir film
>python manage.py startapp film apps/ film


> [!NOTE] Tips
> If you want to structure project like this way, you need to added this line into `settings.py`
> `sys.path.insert(0, os.path.join(BASE_DIR, 'apps'))`

### running 
>python manage.py makemigrations
>python manage.py migrate
>python manage.py runserver 0.0.0.0:8000

## DRF Integrate Swagger doc
````bash
poetry add drf-spectacular
````

in `config/settings.py` add
```python
REST_FRAMEWORK = {
   "DEFAULT_SCHEMA_CLASS": "drf_spectacular.openapi.AutoSchema",
}
...

SPECTACULAR_SETTINGS = {
    "TITLE": "DRF Todo List",
    "DESCRIPTION": "This is a todo list for DRF practice.",
    "VERSION": "1.0.0",
    "SERVE_INCLUDE_SCHEMA": False,
}
```

in `config/urls.py` add
```python
from drf_spectacular import views as doc_views

urlpatterns = [
...

path("api/schema.json", doc_views.SpectacularJSONAPIView.as_view(), name="schema"),

path("api/swagger/", doc_views.SpectacularSwaggerView.as_view()),

path("api/redoc/", doc_views.SpectacularRedocView.as_view()),

]
```