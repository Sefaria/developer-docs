---
title: Local Installation Instructions
excerpt: >-
  Learn about how set up a local Sefaria environment in order to run a local
  copy of the application and web server.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
<Callout icon="🚧" theme="warn">
  ### Recommended: Use API Instead of Local Install

  If you're interested in working with Sefaria's data, we recommend using our [public API.](ref:getting-started-with-your-api) If you prefer a more in-depth interaction with our code, the following installation instructions explain how to do so.

  **This page is under review**. While we are working to make it as accurate as up to date as possible, there may still be some inaccuracies.
</Callout>

<br />

_Please note: if you are a developer interested in contributing code to Sefaria, we suggest first making a fork of this repository by clicking the "Fork" button when logged in to GitHub._

# Getting Started

Begin by cloning the [Sefaria-Project](https://github.com/Sefaria/Sefaria-Project) repository to a directory on your computer. Once cloning is complete, follow these steps:

_Note for macOS users: Ensure you have installed&#x20;_`Homebrew`_&#x20;like this:_

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

You can run the Sefaria-Project in two ways: docker-compose and local install.

We recommend using local installation. However if you are interested in exploring the experimental `docker-compose` approach, please see further information [here](https://developers.sefaria.org/docs/docker-compose-sefaria).

<Callout icon="❗️" theme="error">
  ### Please note:

  The following installation instructions are optimized for users of macOS. We strongly encourage Windows users to either explore our experimental [docker-compose set up method](https://developers.sefaria.org/docs/docker-compose-sefaria) or retrieve our data entirely via the API.
</Callout>

***

## Running Sefaria-Project Locally

To run Sefaria-Project locally, complete the following steps:

### 1) Install Python 3.12+

_Python 3.12 is the minimum supported version. Earlier versions (3.9–3.11) will encounter dependency compatibility errors during&#x20;_`pip install`_._

#### For Linux and macOS

Most UNIX systems come with a Python interpreter pre-installed. However, this is generally still Python 2. The recommended way to get Python 3 without disrupting any software the OS is dependent on is by using Pyenv. You can find instructions for this process [here](https://github.com/pyenv/pyenv#installation) and [here](https://opensource.com/article/19/5/python-3-default-mac#what-we-should-do).

_Please note: Below, we'll discuss how to configure virtual environments to work with Pyenv so you can isolate your Sefaria stack sompletely._

#### For Windows:

The Pyenv repository linked above also provides options for those working in Windows.

In order to simply install Python:

- Read the [Official Python documentation on Windows](https://docs.python.org/3.7/using/windows.html)
- Go to the [Python Download Page](https://www.python.org/downloads/release/python-375/) and download and install Python.
- Add the python directory to your OS' PATH variable if the installer has not done so.

### 2) Install virtualenv (optional but recommended):

If you work on many Python projects, you may want to keep Sefaria's Python installation separate by using Virtualenv. However, if you're happy having Sefaria requirements in your main Python installation, you can skip this step.

#### Working With Pyenv (Recommended)

You can use both virtualenv functionality and pyenv simultaneously. This allows you to further isolate your code and requirements from other projects.

#### For Unix & Windows

To work with pyenv in either Unix or Windows operation systems, use [these intructions](https://github.com/pyenv/pyenv-virtualenv#installing-as-a-pyenv-plugin).

#### macOS:

To work with pyenv in macOS, use [these instructions](https://github.com/pyenv/pyenv-virtualenv#installing-with-homebrew-for-macos-users).

#### How to Create a [pyenv virtualenv](https://github.com/pyenv/pyenv-virtualenv#using-pyenv-virtualenv-with-pyenv).

In your Sefaria directory, run `pyenv local [venv-name]`. This will create a `.python-version` and write the version name provided to the file (e.g. `3.12/envs/sefaria-venv`). This should serve to activate the virtualenv whenever you are in the Sefaria directory.

_Note: If, after following the installation and configuration instructions, running_`python -V`_&#x20;still displays the system version, you may have to manually add the shims directory to your path._ If that does not correct the issue, you should check your bash init file (.zshrc, .bashrc, or the like) for the line `eval "$(pyenv init -)"` (not inside an if statement) and change it to `eval "$(pyenv init --path)"` and restart the shell.

_Note: If you are using an IDE like PyCharm, you can (and should) configure the interpreter options on your Sefaria-Project to point to the Python executable of this virtualenv (e.g._`~/.pyenv/versions/3.12.x/envs/sefaria-venv/bin/python3.12`_)_

##### Classic virtualenv

Install [virtualenv](http://pypi.python.org/pypi/virtualenv), then enter these commands:

_Note: You may need to install Pip (see below) first in order to install virtualenv_

_Note: You can perform this step from anywhere in your command line, but it might be easier and tidier to run this step from the root of your project directory that you just cloned. e.g_`~/web-projects/Sefaria-Project $`

```
virtualenv venv --distribute
source venv/bin/activate
```

Now you should see `(venv)` in front of your command prompt. The second command sets your shell to use the Python virtual environment that you have just created. This is something that you have to run every time you open a new shell and want to run the Sefaria demo. You can always tell if you're in the virtual environment by checking if `(venv)` is at the beginning of your command prompt.

#### 3) Pip:

If you used pyenv, Pip should be available via the pyenv version of Python.

###### Unix

If you don't already have it in your Python installation, install [Pip](https://pip.pypa.io/en/stable/installing/). Then use it to install the required Python packages.

###### Windows

Use instructions [here](http://www.tylerbutler.com/2012/05/how-to-install-python-pip-and-virtualenv-on-windows-with-powershell/) and then make sure that the scripts subfolder of the python installation directory is also in PATH.

_Note: this step (and_**_most_**_&#x20;of the following command line instructions) must be run from the Sefaria-Project root directory_

Run the following command:

```
pip install -r requirements.txt
```

<Callout icon="🚧" theme="warn">
  ### Warning

  Please note, sometimes the `psycopg2` package can cause installation issues, and it is not usually needed for a local installation.

  If you run into trouble, there is a recommended change in the `requirements.txt` file. Just comment out the existing line and uncomment the recommended line.

  If that doesn't work, we recommend _temporarily_ commenting out the `psycopg2` line in `requirements.txt` and then re-running `pip install -r requirements.txt`. (The pinned version in the file may differ from `2.8.6`.)
</Callout>

If you are _not_ using virtualenv, you may have to run it with sudo: `sudo pip install -r requirements.txt`

_Note: You'll probably need to install the Python development libraries as well:_

###### On Debian systems:

```
sudo apt-get install python-dev python3-dev libpq-dev
```

###### On Fedora systems:

```
sudo dnf install python3-devel libpq-devel
```

_Note: If you see an error that_`pg_config executable not found`_, you need to install PostgreSQL. If on macOS you see an error while&#x20;_`Building wheel for psycopg2`_&#x20;for_`linker command failed with exit code 1`_, you may need to add the path to OpenSSL_

```
export LDFLAGS="-L/usr/local/opt/openssl/lib"
export CPPFLAGS="-I/usr/local/opt/openssl/include"
```

After installing the Python development libraries or other dependencies, run `pip install -r requirements.txt` again.

#### 4) Install gettext

`gettext` is a GNU utility that Django uses to manage localizations.

###### On macOS:

```
brew install gettext
```

On some macOS systems `gettext` will still not run after installation and `django manage.py makemessages` will fail. In such a case, one easy solution is to add (replace x's with your gettext version number) to your .bashrc (or its equivalent on your system):

```
export TEMP_PATH=$PATH
export PATH=$PATH:/usr/local/Cellar/gettext/0.xx.x/bin
```

###### On Debian systems

```
sudo apt-get install gettext
```

#### 5) Create a local settings file:

_Note: this step must be run from the Sefaria-Project root directory_

```
cd sefaria
cp local_settings_example.py local_settings.py
vim local_settings.py
```

Replace the placeholder values with those matching your environment. For the most part, you should only have to specify values in the top part of the file where it directs you to change the given values.

Note: Among the placeholder values that need to be replaced, set the `DATABASES` default `NAME` field to a path (including a file name) where Django can create a sqlite database. Using `/path/to/Sefaria-Project/db.sqlite` is sufficient, as we git-ignore all `.sqlite` files.

You can name your local database (`sefaria` will be the default created by `mongorestore` below). You can leave `SEFARIA_DB_USER` ad `SEFARIA_DB_PASSWORD` blank if you don't need to run authentication on Mongo.

#### 6) Create a log directory:

Create a directory called `log` under the root project folder (i.e. `Sefaria-Project/`). To do this, run `mkdir log` from the project's root directory.

The result should look like `Sefaria-Project/log`.

Make sure that the server user has write access to it by using a command such as `chmod 777 log`.

#### 7) Get Mongo running:

If you don't already have it, [install MongoDB](https://www.mongodb.com/docs/manual/administration/install-community/). Our current data dump requires MongoDB version 4.4 or later. After installing Mongo according to the instructions, you should have a mongo service automatically running in the background.

Otherwise, run the mongo daemon with:

```
mongod
```

(Only use sudo if necessary, as it may result in a locked socket file being created that may prevent mongo from running later on).

Or use your os service manager to have it start on startup.

Mongo usually sets all the correct paths for itself to run properly nowadays, but see [here](https://www.mongodb.com/docs/manual/tutorial/manage-mongodb-processes/#start-mongod-processes) for details on setting a specific path for it to store data in.

#### 8) Put some texts in your database:

MongoDB dumps of our database are available to download.

The recommended dump (which is a more manageable size) is available [here](https://storage.googleapis.com/sefaria-mongo-backup/dump_small.tar.gz).

A complete dump is also available [here](https://storage.googleapis.com/sefaria-mongo-backup/dump.tar.gz). The complete dump includes the `history` collections, which contains a complete revision history of every text in our library. For many applications, this data is not relevant. We recommend using the smaller dump unless you're specifically interested in texts revision history within Sefaria.

Unzip the file and extract the `dump` folder. If you don't have an app for unzipping, this can be done from Command Prompt by navigating to the folder containing the download and using `tar -xf dump.tar.gz` or `tar -xf dump_small.tar.gz` (depending on which dump you downloaded).

This `dump` must be restored as a MongoDB database.

From MongoDB version 4.4, mongorestore comes separately from the MongoDB server, so if you don't already have it, you'll also need to download and unzip the [Database Tools](https://www.mongodb.com/try/download/database-tools?tck=docs_databasetools). You may then need to add its \bin\ directory to your PATH environment variables.

Once you have the unzipped `dump`from the folder which contains `dump`, run:

```
mongorestore --drop
```

This will create (or overwrite) a mongo database called `sefaria`.

If you used the recommended dump, `dump_small.tar.gz`, create an empty collection inside the `sefaria` database called `history`. This can be done using the mongo client shell or a GUI you have installed, such as MongoDB Compass.

#### 9) Set up Django's local server

Sefaria is using Google's reCAPTCHA to verify the user is not a bot. For a deployment, you should register and use your own reCAPTCHA keys ([https://pypi.org/project/django-recaptcha/#installation](https://pypi.org/project/django-recaptcha/#installation)).<br />For local development, the default test keys would suffice. The warning can be suppressed by uncommenting the following in the local\_settings.py file:

```
SILENCED_SYSTEM_CHECKS = ['captcha.recaptcha_test_key_error']
```

`manage.py` is used to run and manage the local server. It is located in the root directory of the `Sefaria-Project` codebase.

Django auth features run on a separate database. To init this database and set up Django's auth system, switch to the root directory of the `Sefaria-Project` code base, and run (from the project root):

```
python manage.py migrate
```

#### 10) Install Node:

_Note: Older versions of_`Node`_&#x20;and&#x20;_`npm`_&#x20;ran into a file name length limit on Windows OS. This problem should be mitigated in newer versions on Windows 10._

Node is now required to run the site. Even if you choose to have Javascript run only on the client, we are also using [Webpack](https://webpack.js.org/) to bundle our Javascript.

Installing Node and npm from the main installers on Node's homepage may cause permission issues. For that reason, it is recommended to use one of the alternative methods (with a preference for a version manager like nvm) listed [here](https://nodejs.org/en/download/package-manager/).

###### Debian, Ubuntu, and Linux Mint

You are better off using `apt-get nodejs` or following the instructions [here](https://github.com/nodesource/distributions/blob/master/README.md). They will install both Node and npm.

###### macOS

use `brew install node` or [`nvm`](https://nodejs.org/en/download/package-manager/#nvm)

Now download the required Javascript libraries and install some global tools for development with the `setup` script (from the project root).

```
npm install
npm run setup
```

If the second command fails, you may have to install using `sudo npm run setup`.

#### Run Webpack

To get the site running, you need to bundle the Javascript with Webpack. Run:

```
npm run build-client
```

to bundle once. To watch the Javascript for changes and automatically rebuild, run:

```
npm run watch-client
```

#### Server-side rendering with Node:

Sefaria uses React.js. To render HTML server-side, we use a Node.js server. For development, the site is fully functional without server-side rendering. For deploying in a production environment, however, server-side HTML is very important for bots and SEO.

##### Install Redis

To use server-side rendering, you must also install Redis Cache. `brew install redis` or `sudo apt-get install redis`.

To run redis: `redis-server`. On macOS: `brew services start redis`

##### Configure Django and Node to use Redis as a shared datastore

**Django**

Update your `local_settings.py` file and replace the `CACHES` variable with the `CACHES` variable meant for server-side rendering, commented out in `local_settings_example.py`.

Also, make sure the following are set:

```
SESSION_CACHE_ALIAS = "default" #declares where Django's session engine will store data
USER_AGENTS_CACHE = 'default' #declares where the Django user agent middleware will store data (cannot be JSON encoded!)
SHARED_DATA_CACHE_ALIAS = 'shared' #Tells the application where to store data placed in cache by `sefaria.system.cache` shared-cache functions

USE_NODE = True
NODE_HOST = "http://localhost:3000" #Or whatever host and port you set Node up to listen on

```

**Node**

The following environment variables, defined in `./node/local_settings.js`, can be set to configure the node instance:

| Env Var       | Default     | Description                                                    |
| ------------- | ----------- | -------------------------------------------------------------- |
| `NODEJS_PORT` | `3000`      | The port to be used by the NodeJs service                      |
| `DEBUG`       | `false`     | Determines whether the NodeJs service should run in debug mode |
| `REDIS_HOST`  | `127.0.0.1` | The Redis instance to point Node to when running               |
| `REDIS_PORT`  | `6379`      | The Default port Redis listens on                              |

These variables can be set via command line explicitly, set up to be defined when your machine's shell runs, or set up in your IDE settings for running the node server.

For development, you can run the Node server using nodemon with:

```
npm start
```

To run webpack with server-side rendering, use:

```
npm run build
```

or

```
npm run watch
```

#### 11) Run the development server:

```
python manage.py runserver
```

You can also make it publicly available by specifying 0.0.0.0 for the host:

```
python manage.py runserver 0.0.0.0:8000    
```

## Command Line Interface

The shell script `cli` will invoke a Python interpreter with the core models loaded, and can be used as a standalone interface to texts or for testing.

```
$ ./cli
>>> p = LinkSet(Ref("Genesis 13"))
>>> p.count()
226
```

<br />
