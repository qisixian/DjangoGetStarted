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
init Django OAuth Toolkit
```bash
pip install django-oauth-toolkit
pip freeze > requirements.txt
# after Add oauth2_provider to INSTALLED_APPS in settings.py:
# Comment: 在Java里代码里的entity类和数据库同步好像是一件很难的事，但是django里一条命令就做完了
python manage.py migrate
```
Create a user
```bash
python manage.py createsuperuser
```
Username: admin
Email address: qisixian1998@gmail.com
Password: 123456

## Using Django OAuth Toolkit

lets create an application. http://127.0.0.1:8000/oauth/applications/

### Authorization Code flow：

create application "Authorization Grant Type" choose "authorization-code"

got a client id and a client secret

start the Authorization code flow

http://127.0.0.1:8000/oauth/authorize/?response_type=code&client_id=urwdrTHl4mMvov5vQjUfikASDX0rELjsMFNbIJ9I&redirect_uri=http://127.0.0.1:8000/noexist/callback

got a code

```bash
# 这条命令一直通不过，最后我去数据库里改了一下code的过期时间，然后通过了
# 这个server用的是标准时间（-8小时之后的），但是并不冲突
# 这个Code的有效时间只有一分钟，而且只能使用一次然后就消失了
curl -X POST \
    "http://127.0.0.1:8000/oauth/token/" \
    -d "client_id=urwdrTHl4mMvov5vQjUfikASDX0rELjsMFNbIJ9I" \
    -d "client_secret=oUDqY59XRjBOYcw8qFIx3dZIU7nXPSQLTPDJwfWH1oftYrz60h4NK3nQKYI8dwUjMLs06yPCPMgAFIcwY4hgFZDvpcXkYX9Cx75lqOjhclohhSHwYhie6k0YA2PTe3Z6" \
    -d "code=HQd38Q0vYyhGtEpC1fviUsvAuxaStX" \
    -d "redirect_uri=http://127.0.0.1:8000/noexist/callback" \
    -d "grant_type=authorization_code"
```
return:
{
    "access_token": "MvDQ4wXir2N2OtQT4XxKU0OKqz71Sq",
    "expires_in": 36000,
    "token_type": "Bearer",
    "scope": "read write",
    "refresh_token": "4LISrazhNMY5fjtCdxp3BEluVdTB0v"
}

To access the user resources we just use the access_token
```bash
curl \
    -H "Authorization: Bearer MvDQ4wXir2N2OtQT4XxKU0OKqz71Sq" \
    -X GET http://localhost:8000/resource
```

### Client Credential flow
create application "Authorization Grant Type" choose "client-credentials"

got client id and client secret

We need to encode client_id and client_secret as HTTP base authentication encoded in base64

in python terminal
```python
import base64
client_id = "f2DfYFBUG8sVDLDmzJyfkCnGGrk3jmXJ9ATl5MWI"
secret = "xcjJgdoOWFgEG08ULL2LSVoyQUTB8KDxFD1vukwLlWskEBg3YNqiRwmClBrAaTL7fvUzqif4aCdwmucN1yPfcjAbnEi3YZYPj6GRQs4iDEabBs2UOTll2co1E1FeDabS"
credential = "{0}:{1}".format(client_id, secret)
base64.b64encode(credential.encode("utf-8"))
```

got a credential

```bash
export CREDENTIAL=ZjJEZllGQlVHOHNWRExEbXpKeWZrQ25HR3JrM2ptWEo5QVRsNU1XSTp4Y2pKZ2RvT1dGZ0VHMDhVTEwyTFNWb3lRVVRCOEtEeEZEMXZ1a3dMbFdza0VCZzNZTnFpUndtQ2xCckFhVEw3ZnZVenFpZjRhQ2R3bXVjTjF5UGZjakFibkVpM1laWVBqNkdSUXM0aURFYWJCczJVT1RsbDJjbzFFMUZlRGFiUw==
curl -X POST \
    "http://127.0.0.1:8000/oauth/token/" \
    -H "Authorization: Basic ${CREDENTIAL}" \
    -H "Cache-Control: no-cache" \
    -H "Content-Type: application/x-www-form-urlencoded" \
    -d "grant_type=client_credentials"
```
return:
{
    "access_token": "3tm9JnfyzLAIzutBfsYxmtWUshiDel",
    "expires_in": 36000, 
    "token_type": "Bearer",
    "scope": "read write"
}


## Run Project
```
# python3 -m venv venv
# source venv/bin/activate
pip install -r requirements.txt
python manage.py runserver
```


## Reference:
https://django-oauth-toolkit.readthedocs.io/en/latest/getting_started.html