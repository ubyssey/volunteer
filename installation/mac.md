## Setup Instructions for Mac OS

*Note: Current recommended setup is to follow [Docker Instructions](installation/docker.md)*

First, make sure you have Homebrew installed:

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

You should also install pip if you haven't already:

```bash
sudo easy_install pip
```
### Database

Dispatch requires a MySQL database to store information. Install mysql with Homebrew.

```bash
brew install mysql
```

Now run the server and create a fresh database:

```bash
mysql.server start
echo "CREATE DATABASE ubyssey" | mysql -u root
```

_If you're seeing "ERROR 1045 \(28000\): Access denied for user 'root'@'localhost'", try:_

```bash
echo "CREATE DATABASE ubyssey" | mysql -u root -p
```

Next, populate the database with sample data:

```bash
curl https://storage.googleapis.com/ubyssey/dropbox/ubyssey.sql | mysql -u root ubyssey
```

_If you're seeing "curl: \(23\) Failed writing body" try:_

```bash
curl https://storage.googleapis.com/ubyssey/dropbox/ubyssey.sql | tac | tac | mysql -u root ubyssey
```

_If you're seeing "ERROR 1045 \(28000\): Access denied for user 'root'@'localhost'", try:_

```bash
curl https://storage.googleapis.com/ubyssey/dropbox/ubyssey.sql | mysql -u root ubyssey -p
```

### Getting the code

We recommend working inside a virtualenv, but it's not required.

```bash
# Install virtualenv if you don't have it
pip install virtualenv

# Create a new virtual environment
virtualenv ubyssey-dev
cd ubyssey-dev

# Activate the virtualenv
source bin/activate
```

First, fork ubyssey.ca repository by following [this tutorial](/installation/forking-the-repo.md). Afterward clone the `ubyssey.ca` repository:

```bash
# Change the url to your cloned repo
git clone https://github.com/<YOUR-USERNAME>/ubyssey.ca
cd ubyssey.ca
```

Install the required Python packages with pip:

```bash
pip install -r requirements.txt
```

_Note: you might get an error saying that _`libjpeg`_ is required. You can install it with Homebrew:_

```bash
brew install libjpeg zlib
```

_Note: if _`mySQL-python`_ fails to install, you might be missing some of Apple's Command Line Tools. Install _[_Xcode_](https://itunes.apple.com/ca/app/xcode/id497799835?mt=12)_ from the Mac App Store, then:_

```bash
xcode-select --install
```

### Project settings

Copy the sample settings file into the project root:

```bash
cp _settings/settings-local.py ubyssey/settings.py
```

### Static files

Install the required Node packages with npm:

```bash
cd ubyssey/static
npm install
```

Install a global verison of gulp \(if you don't have it already\) and build the static files:

```bash
npm install -g gulp
gulp build-dev
```

### Media Files

Download and unzip the [sample media folder](https://storage.googleapis.com/ubyssey/dropbox/media.zip) to `ubyssey.ca/media/`. This will make it so the images attached to the sample articles are viewable.

### Running the server

Go back to the root of the project ("ubyssey.ca" directory)

Now start the server!

```bash
python manage.py runserver
```

_If you're seeing "django.core.exceptions.ImproperlyConfigured: Error loading MySQLdb module: No module named MySQLdb", run _`pip install MySQL-python`_ and try again._

### Admin Panel

You can log in to the admin panel at [http://localhost:8000/admin/](http://localhost:8000/admin/):

**Email:** volunteer@ubyssey.ca

**Password:** volunteer

