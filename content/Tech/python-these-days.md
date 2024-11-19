---
created: 2024-02-14T22:42
updated: 2024-03-08T02:50
title: Python these days
---
## Install `python`
```shell
brew install python
```

## Install `poetry`
```shell
// Ubuntu
curl -sSL https://install.python-poetry.org | python3 -

// OSX
curl -sSL https://install.python-poetry.org | sed 's/symlinks=False/symlinks=True/' | python3 -
```
  

##  Alias `python3` to `python`
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
> 
> cd loudy-webserver/
### poetry init

>poetry init
>
>poetry add Django
>
>poetry add djangorestframework

### entering virtual environment
>poetry shell

### folder of config
>poetry run django-admin startproject config .

### folder of apps
>mkdir apps 
>
>cd apps
>
>mkdir film
>
>python manage.py startapp film apps/ film



> [!NOTE] Tips
> If you want to structure project like this way, you need to added this line into `settings.py`
> `sys.path.insert(0, os.path.join(BASE_DIR, 'apps'))`


> [!NOTE] 後記
> 最後還是莫名會遇到 naming confilcts 的問題，最快的解決方法還是在根目錄(與`manage.py`同層)，
> 直接`python manage.py [app name]` 再將資料夾搬進apps資料夾下。

### running 
>python manage.py makemigrations
>
>python manage.py migrate
>
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


## Sqlite GUI

While sqlite itself does not provide any network endpoints you can use a web gui such as [sqlite-web](https://github.com/coleifer/sqlite-web).

Assuming you have python installed you can install it with

```bash
pip install sqlite-web
```

Once installed, start the web interface with:

```undefined
sqlite_web my_database.db
```

By default it runs on [http://127.0.0.1:8080/](http://127.0.0.1:8080/) ie is only accesible from the localhost.

To make it visible on your LAN/WAN start it with:

```css
sqlite_web --host 0.0.0.0 my_database.db
```

While running with `--host 0.0.0.0` might be fine on your own home or corporate network, it's a really bad idea to do it on a public host (even though the app allows setting a password). For remote access on a public host you will probably want to use the default binding on `127.0.0.1` and then forward the port over ssh or a vpn tunnel.


## Setup python-redis

```shell
poetry add channels_redis
```

```shell
docker run --rm -p 6379:6379 redis:7
```

```python
# django_channels/settings.py
# Channels
# 新增 channel layers 設定
CHANNEL_LAYERS = {
    "default": {
        "BACKEND": "channels_redis.core.RedisChannelLayer",
        "CONFIG": {
            "hosts": [("127.0.0.1", 6379)],
        },
    },
}
```

## Django cors
```
poetry add django-cors-headers
```

在settings.py的INSTALLED_APPS加上corsheaders:

```
INSTALLED_APPS = (
    ...
    'corsheaders',
    ...
)
```

在MIDDLEWARE_CLASSES加上:

```
MIDDLEWARE_CLASSES = (
    ...
    'corsheaders.middleware.CorsMiddleware',
    'django.middleware.common.CommonMiddleware',
    ...
)
```


> [!NOTE] 
> 如果設定了USE_ETAGS=True,那麼CordMiddleware必須要在Django CommonMiddleware之前, 否則CORS headers就會因304 not-modified responses被遺失了.


設定方式是在settings.py中設定如下:

```
CORS_ORIGIN_ALLOW_ALL = True # 允許所有跨站請求, 且whitelist不會被使用
```

CORS_ORIGIN_ALLOW_ALL的預設值是False, 我們可以設定white list:

```
CORS_ORIGIN_WHITELIST = (
    'localhost:9000', # 本機的不同port
    'mysite.com',
    ...
)
```


## WTF is   `*Args` and `**Kwargs` 
![[Pasted image 20240308024730.png]]
https://www.youtube.com/watch?v=4jBJhCaNrWU

### Conclusion
`*args` handled tuple or list (Iterable)
`**kwargs` handled dictionary(key & value pairs)
