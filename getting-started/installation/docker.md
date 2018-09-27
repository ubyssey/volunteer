## Setup Instruction for Docker

*Note: the following setup was done on an ubunutu linux computer.*

#### 0: Install Docker

Install docker 18.06.x and docker-compose 1.10.x.

Follow the instructions here. This will require you to create a docker account if you do not already have one.
https://docs.docker.com/

*If setting up on linux, all docker and docker-compose commands should be preceeded with sudo.*

Create `ubyssey-dev` folder at the location you want to code in, then run these commands

```bash
# Inside ubyssey-dev dir
git clone https://github.com/ubyssey/ubyssey.ca.git
git clone https://github.com/ubyssey/dispatch.git
```

cd into the root directory for both your ubyssey.ca and dispatch repos, this is */ubyssey-dev* if you followed the ubyssey Mac|Windows installation instructions, and run the following command

```bash
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
sudo docker exec -t -i ubyssey_db bash
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
#You may not be prompted for the password, and the curl operation may appear to have hanged. Simply type the password and press enter.
curl https://storage.googleapis.com/ubyssey/dropbox/ubyssey.sql | mysql -u root ubyssey -p
```

Your db container is up and running! Type `exit` to exit this container

#### 2: Perform django migrations on the docker container

Connect to the ubyssey-dev docker container

```bash
sudo docker exec -t -i ubyssey-dev bash
```

Run migrations on the mysql database
```bash
cd ubyssey.ca
cp _settings/settings-local.py ubyssey/settings.py
python manage.py migrate
```
Once the database has been populated, and migrations have been applied,
you should be able to proceed to localhost:8000 and localhost:8000/admin
to view ubyssey.ca and dispatch running from your ubyssey-dev docker container.

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
sudo docker-compose up
```


