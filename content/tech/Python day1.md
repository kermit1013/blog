---
created: 2024-02-14T22:42
updated: 2024-02-15T17:31
---
brew install python

  
On macOS, there are several ways to alias `python3` to `python`. Here are the best options, each with its own pros and cons:

**1. Using `.bashrc` or `.zshrc` (Recommended for permanent alias):**

- Edit your shell configuration file:
    
    - Open Terminal and run `nano ~/.bashrc` (for Bash) or `nano ~/.zshrc` (for Zsh).
    
- Add the following line:

```
alias python="python3"
```

- Save the file and reload your shell configuration:
    
    - `source ~/.bashrc` (for Bash) or `source ~/.zshrc` (for Zsh).

curl -sSL https://install.python-poetry.org | sed 's/symlinks=False/symlinks=True/' | python -

 How to solve Pylance 'missing imports' in vscode

In my case, the fastest solution when imports are not missing is to launch vscode from the virtual environment. Basically, activate the venv as always, and then `code .`


mkdir loudy-webserver
cd loudy-webserver/
poetry init
poetry add Django
poetry add djangorestframework
poetry shell
poetry run django-admin startproject config .

mkdir apps 
cd apps
mkdir film
python manage.py startapp film apps/ film



python manage.py makemigrations\n
python manage.py migrate
python manage.py runserver