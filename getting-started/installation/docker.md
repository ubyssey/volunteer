## Setup Instruction for Docker

*Note: the following setup was done on an Ubunutu Linux.*

#### 0: Install Docker

Install `docker 18.06.x` Follow the instructions [from official Docker doc](https://docs.docker.com/). This will require you to create a docker account if you do not already have one.

Afterward install `docker-compose 1.10.x` from [here](https://docs.docker.com/compose/install/) (If not already installed with docker)

*If setting up on linux, all docker and docker-compose commands should be preceeded with `sudo`.* To enable docker without `sudo`, follow this [official post-installation doc](https://docs.docker.com/install/linux/linux-postinstall/)

Before we proceed, we recommend creating a dedicated directory (folder) for Ubyssey related projects. In this tutorial we will refer to this dir as `ubyssey-dev` (you can use any other name that makes sense to you). Let's create the directory at the location you want the code to live in:

We recommend working inside a virtualenv, but it's not required.

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

We will now download `ubyssey.ca` and `dispatch` projects. Run these commands inside `ubyssey-dev` dir (note: to hcheck which dir you are in, try `pwd` command):

```bash
# Inside ubyssey-dev dir
git clone https://github.com/ubyssey/ubyssey.ca.git
git clone https://github.com/ubyssey/dispatch.git
```

We have stored the config files for Docker in `local-dev`dir inside `ubyssey.ca` project. To make enable Docker we first move the files to appropriate location:

```bash
# cd into your Ubyssey project dir
cd ~/ubyssey-dev
cp -r ./ubyssey.ca/local-dev/. .
```

Build and run the docker containers. This command can take several minutes, so be patient.

```bash
docker-compose up
```

*The database may fail to initialize. Simply re-run the above command and it should work.*

To see currently running docker containers, run the following command in a different terminal.

```bash
docker ps
```
## The following should be done in a separate terminal

#### 1: Setup the mysql container with a database

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

#### 2: Perform django migrations on the docker container

Connect to the `ubyssey-dev` docker container

```bash
docker exec -t -i ubyssey-dev bash
```

Run migrations on the mysql database

```bash
cd ubyssey.ca
cp _settings/settings-local.py ubyssey/settings.py
python manage.py migrate
```

Once the database has been populated, and migrations have been applied,
you should be able to proceed to `localhost:8000` and `localhost:8000/admin` to view ubyssey.ca and dispatch running from your ubyssey-dev docker container.

##### Dispatch

You can see Dispatch by going to `http://localhost:8000/admin/`

Username is `volunteer@ubyssey.ca`, password is `volunteer`


#### Extra docker info

##### When in doubt, you may need to clear docker's cache and remove all docker images.

```bash
# will remove docker cache and clear all images
docker system prune -a
```

then you can rebuild your docker images using

``` bash
# from ubyssey-dev dir
docker-compose up
```


