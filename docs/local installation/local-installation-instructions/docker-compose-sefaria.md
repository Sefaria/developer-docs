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
# How to Run Sefaria With `docker-compose`

<Callout icon="🚧" theme="warn">
  ### Please note:

  **This approach is** **experimental** **and** **has not yet been fully tested**. If working with docker-compose is a method that would help you, please [contact us.](page:contact-us)
</Callout>

<br />

### 1) Install Docker and Docker Compose

Begin by installing Docker (instructions [here](https://docs.docker.com/docker-for-mac/install/)) and Docker Compose (instructions [here](https://docs.docker.com/compose/install/)). If you're working with Windows, you might need special support. For further information on this, see the [official Docker documentation](https://docs.docker.com/desktop/setup/install/windows-install/).

### 2) Run the project

In your terminal, run the following:

```
docker-compose up
```

This will build the project and run it. Once you've run the above, you should have all the proper services set up. The next step is to add some texts.

### 3) Connect to Mongo and add texts.

Connect to Mongo, running on port 27018. Sefaria uses 27018 instead of the standard port (27017) in order to avoid conflicts with any Mongo instances you may already have running.

_Please note: You can find instructions for downloading the Mongo dump below, in section 8.&#x20;_

Once you've connected Mongo, restore the mongodump to the dockerized Mongo instance with the following command:

```
mongorestore --host localhost:27018
```

### 4) Update your local settings file:

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

##### 5) Connect to the django container and run migrations:

In a new terminal window run:

```
docker exec -it sefaria-project-web-1 bash
```

This will connect you to the django container. Now run:

```
python manage.py migrate
```

##### 6) Run webpack:

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

##### 7) Visit the site:

In your browser go to http\://localhost:8000

If the server isn't running, you may need to run `docker-compose up` again.
