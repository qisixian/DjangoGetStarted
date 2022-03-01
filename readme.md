## Command Recording

out of project using django-admin to create project
```bash
django-admin startproject DjangoGetStarted
cd DjangoGetStarted
```
In the project

init venv
```bash
python3 -m venv venv
source venv/bin/activate
python -m pip install --upgrade pip
python -m pip install Django
pip freeze > requirements.txt
```
init git
```bash
# add file .gitignore
git init
git add all
```
set up a custom user model (This model behaves identically to the default user model, but you’ll be able to customize it if need)
```bash
python manage.py startapp users
# after add servral code
# 下面两个命令跟从代码同步数据库结构变更有关
python manage.py makemigrations
python manage.py migrate
```


## Run Project
```
# python3 -m venv venv
# source venv/bin/activate
pip install -r requirements.txt
python manage.py runserver
```


## Reference:
https://django-oauth-toolkit.readthedocs.io/en/latest/getting_started.html