## Setup Instruction for Docker :whale:

*Note: the following setup was done on an Ubunutu Linux, but should work on windows systems as well*

**Note for Windows: From what we've seen, Windows Docker Toolbox seem to have a poor performance. We recommend Windows users to set up the environment natively as described in [Windows Setup tutorial](/installation/windows.md)**

### Install Python

On Mac and Linux, python & pip comes with the system.  
For windows, you need to download them manually.  
[Download Python 2.7](https://www.python.org/downloads/release/python-2715/) then [install pip](https://pip.pypa.io/en/stable/installing/) by following the links

*Note: you may need to add python AND pip to your path, follow the instructions [here](https://stackoverflow.com/questions/3701646/how-to-add-to-the-pythonpath-in-windows).*

### Install Docker

Install `docker 18.06.x` Follow the instructions [from official Docker doc](https://docs.docker.com/). This will require you to create a docker account if you do not already have one.

Afterward install `docker-compose 1.10.x` from [here](https://docs.docker.com/compose/install/) (If not already installed with docker).

*If setting up on linux, all docker and docker-compose commands should be preceeded with `sudo`.* To enable docker without `sudo`, follow this [official post-installation doc](https://docs.docker.com/install/linux/linux-postinstall/).

## Set up development folder

Before we proceed, we recommend creating a dedicated directory (folder) for Ubyssey related projects. In this tutorial we will refer to this dir as `ubyssey-dev` (you can use any other name that makes sense to you). Let's create the directory at the location you want the code to live in:

*Note: copy and paste **all** commands in terminal:*

##### How to find the terminal:  
Win: Click on Start btn > Type "cmd" > Click on "Command Prompt"  
Mac: Open Spotlight search or Applications folder > Type "terminal")

```bash
# Install virtualenv if you don't have it
pip install virtualenv

# Create a new virtual environment
cd ~
virtualenv ubyssey-dev
cd ubyssey-dev

# Activate the virtualenv
source bin/activate
```

We recommend working inside a virtualenv, but it's not required.

#### Note for Mac

Docker has a [a known CPU overusage issue](https://github.com/docker/for-mac/issues/1759) for macOS that may make your fan go wild.

Jason, one of our volunteers, found [a trick](https://github.com/docker/for-mac/issues/1759) to fix the issue!

For performance boost, there's a a popular tool called [docker-sync](http://docker-sync.io/).

## Fork repositories

We will now download `ubyssey.ca` and `dispatch` projects. Follow [our forking instructions](/installation/forking-the-repo.md) first to copy the project under your GitHub username. Run these commands inside `ubyssey-dev` dir (note: to hcheck which dir you are in, try `pwd` command):

```bash
# Inside ubyssey-dev dir
# Change urls to your cloned repo
git clone https://github.com/<YOUR-USERNAME>/ubyssey.ca.git
git clone https://github.com/<YOUR-USERNAME>/dispatch.git
```

We have stored the config files for Docker in `local-dev`dir inside `ubyssey.ca` project. To make enable Docker we first move the files to appropriate location:

```bash
# cd into your Ubyssey project dir
cd ~/ubyssey-dev
cp -r ./ubyssey.ca/local-dev/. .
```

We now need to set the local settings for django.

```bash
cp -r ubyssey.ca/_settings/settings-local.py ubyssey.ca/ubyssey/settings.py
```

Build and run the docker containers. This command can take several minutes, so be patient.

```bash
docker-compose up
```

#### Note for Windows: 
docker-compose requires Docker to be running in the background. If docker-compose fails run Docker Toolbox first and try again*

*Note: The database may fail to initialize. Simply re-run the above command and it should work.*

To see currently running docker containers, run the following command in a separate terminal.

```bash
docker ps
```

**NOTE: Keep this terminal window with docker running open for following commands**

### Setup the mysql container with a database

**NOTE: Again, following commands should be done in a separate terminal window than before**

Connect to the ubyssey_db docker container.

```bash
docker exec -t -i ubyssey_db bash
```

Setup the local database in ubyssey_db docker container.

```bash
# password is ubyssey
mysql -u root -p
create database ubyssey;
quit;
```

Populate the database.

```bash
apt update
apt-get install curl
# password is ubyssey.
# You may not be prompted for the password, and the curl operation may appear to have hanged. Simply type the password and press enter.
curl https://storage.googleapis.com/ubyssey/dropbox/ubyssey.sql | mysql -u root ubyssey -p
```

Your db container is up and running! Type `exit` to exit from this container

### Perform django migrations on the docker container

Connect to the `ubyssey-dev` docker container

```bash
docker exec -t -i ubyssey-dev bash
```

Run migrations on the mysql database

```bash
cd ubyssey.ca
python manage.py migrate
```

Once the database has been populated, and migrations have been applied,
you should be able to proceed to `localhost:8000` and `localhost:8000/admin` to view ubyssey.ca and dispatch running from your ubyssey-dev docker container.

### Media Files

Download and unzip the [sample media folder](https://storage.googleapis.com/ubyssey/dropbox/media.zip) to `ubyssey-dev/ubyssey.ca/media/`. This will make it so the images attached to the sample articles are viewable.

#### Dispatch

You can see Dispatch by going to `http://localhost:8000/admin/`

Username is `volunteer@ubyssey.ca`, password is `volunteer`


### Extra docker info

#### When in doubt, you may need to clear docker's cache and remove all docker images.

```bash
# will remove docker cache and clear all images
docker system prune -a
```

then you can rebuild your docker images using

``` bash
# from ubyssey-dev dir
docker-compose up
```


