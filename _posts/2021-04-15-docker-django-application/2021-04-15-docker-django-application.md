---
layout: post
title: "Docker Django Application"
date: "2021-04-15"
description: ""
coverimage: Docker_Django_Application_revised.jpg
tags: docker django
published: true
posttype: article
categories: tutorial
---
```
ssh-copy-id -n root@192.248.158.136
```

The `-n` is for a dry run. 

This will copy the public key allowing us to connect to the server with our private key.
```
ssh-copy-id root@192.248.158.136
```

However we need our private key on the box to talk to the private github reppo.

```
scp id_rsa root@192.248.158.136:/root/.ssh/
```


Make a directory in the traditional location 

```
mkdir -p /var/www
```

Pull across blank repo
```
git clone git@github.com:cleverberry/docker-django-evilsaint.com.git  /var/www/django
```

I am not going to pull across the existing project as I will need to files as I convert this. 

```
root@docker-django-evilsaint:~# cd /tmp/
root@docker-django-evilsaint:/tmp# git clone git@github.com:cleverberry/django-evilsaint.com
```


```
:::docker
# pull the official base image
FROM python:3.8.3-alpine

# set work directory
WORKDIR /usr/src/django-app

# set environment variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

# install dependencies
RUN pip install --upgrade pip
COPY ./requirements.txt .
RUN pip install -r requirements.txt

# copy project
COPY . .
```


So, we started with an Alpine-based Docker image for Python 3.8.3. We then set a working directory along with two environment variables:

> PYTHONDONTWRITEBYTECODE: Prevents Python from writing `pyc` files to disc (equivalent to python -B option)

> PYTHONUNBUFFERED: Prevents Python from buffering stdout and stderr (equivalent to python -u option)



Next I want to create a directory for me to put my app files into. 
```
mkdir /usr/src/django-app
cp /tmp/django-evilsaint.com/app /usr/src/django-app
```

Let's go back to our project root and have a look at what we have. 


<img src="/static/7d849d04-b590-4c8f-af96-0ee0a2ac856c.png" class="img-fluid" alt="">


Next, we need to add a docker-compose.yml file to the project root:

<img src="/static/d5b1bbdb-a07e-41e2-b771-f32f8edbb513.png" class="img-fluid" alt="">


```
:::python hc 8
version: '3.7'

services:
  web:
    build: ./app
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - ./app/:/usr/src/django-app/
    ports:
      - 8000:8000
    env_file:
      - ./.env
````


We have created a volume as per the Django app directory we created earlier. This is where we will dump our app source
```
- ./app/:/usr/src/django-app/
```

Next we have specified an environmental file
```
-./.env
```

Here is what I have placed in mine
```
DEBUG=True
SECRET_KEY=sdiofujsdioaaiWfjRsdTlHjriloarejtiop9dfjuaopjkfiopsdhujZfopieqriarosdXhfpAAsaofkcikoSaSshfgopiswujrfopsdhjiuagTUOphtsudjfiLGHNFohiqeprtghejrfds
DJANGO_ALLOWED_HOSTS=localhost 127.0.0.1 [::1]
SQL_ENGINE=django.db.backends.postgresql_psycopg2
SQL_DATABASE=the_evilsaint_com_034998jer903203ksldjsl
SQL_USER=mysaint
SQL_PASSWORD=iloveyou_abc123_princess_princess
SQL_HOST=localhost
SQL_PORT=5432
```

So now we reference these in the settings and fill in some defaults

Edit the settings file inside this path
```
nano /usr/src/django-app/
```

This will become your database block
```
DATABASES = {
    "default": {
        "ENGINE": os.environ.get("SQL_ENGINE", "django.db.backends.sqlite3"),
        "NAME": os.environ.get("SQL_DATABASE", os.path.join(BASE_DIR, "db.sqlite3")),
        "USER": os.environ.get("SQL_USER", "evilsaint"),
        "PASSWORD": os.environ.get("SQL_PASSWORD", "!5_V3ry_53cur3"),
        "HOST": os.environ.get("SQL_HOST", "localhost"),
        "PORT": os.environ.get("SQL_PORT", "5432"),
    }
}
```

See how these are used for the remaining variables. 

<img src="/static/dfc07527-77c2-430b-ab01-e5c2e4c2c06c.png" class="img-fluid" alt="">

Using the psycopg2 package we need to add some dependencies to our docker file. 

```
# install psycopg2 dependencies
RUN apk update \
    && apk add postgresql-dev gcc python3-dev musl-dev
```


## Update Composer With Database, Gunicorn, Nginx