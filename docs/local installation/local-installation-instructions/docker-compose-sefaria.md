---
title: Run Sefaria with Docker-Compose
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
## Run with `docker-compose`

> 🚧 Note
>
> This approach is **experimental** and **has not yet been fully tested**. If this is a method that would help you, please [contact us](page:contact-us) and let us know.

##### 1\) Install Docker and Docker Compose

Follow the instructions [here](https://docs.docker.com/docker-for-mac/install/) to install Docker and [here](https://docs.docker.com/compose/install/) to install Docker Compose.

Windows users might need special support, see the [official Docker documentation](https://docs.docker.com/desktop/setup/install/windows-install/) for more. 

##### 2\) Run the project

In your terminal run:

```
docker-compose up
```

This will build the project and run it. You should now have all the proper services set up! Now, let's add some texts.

##### 3\) Connect to mongo and add texts:

Connect to mongo running on port 27018. We use 27018 instead of the standard port 27017 to avoid conflicts with any mongo instance you may already have running.

Follow instructions in section 8 below to download the mongo dump.

Restore the mongodump to the dockerized mongo instance with the following command:

```
mongorestore --host localhost:27018
```

##### 4\) Update your local settings file:

Copy the local settings file:

```
cp sefaria/local_settings_example.py sefaria/local_settings.py
```

Replace the following values:

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'sefaria',
        'USER': 'admin',
        'PASSWORD': 'admin',
        'HOST': 'postgres',
        'PORT': '',
    }
}


MONGO_HOST = "db"
```

Optionally, you can replace the cache values as well:

```
MULTISERVER_REDIS_SERVER = "cache"
REDIS_HOST = "cache"
```

and the respective values in CACHES

##### 5\) Connect to the django container and run migrations:

In a new terminal window run:

```
docker exec -it sefaria-project-web-1 bash
```

This will connect you to the django container. Now run:

```
python manage.py migrate
```

##### 6\) Run webpack:

In a new terminal window, run:

```
docker exec -it sefaria-project-node-1 bash
```

This will connect you to the django container. Now run:

```
    npm run build-client
```

or 

```
    npm run watch-client
```

##### 7\) Visit the site:

In your browser go to http\://localhost:8000

If the server isn't running, you may need to run `docker-compose up` again.
