# Automated Setup Instructions using Docker

(This is a Work In Progress. Please contact the current web devs if anything is confusing!)

## Setup Basics

1. Install [git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git), [Docker](https://docs.docker.com/engine/), [Visual Studio Code](https://code.visualstudio.com/download), and the [Remote Development plugin for Visual Studio Code](https://code.visualstudio.com/docs/remote/remote-overview).

2. In your preferred development folder:
```
git clone https://github.com/ubyssey/ubyssey.ca.git
cd ubyssey.ca
docker build . -t ubyssey/ubyssey.ca:latest
```
&nbsp;<span style="font-size:0.9em;">*(Or replace ubyssey/ubyssey.ca:latest with name of your choice)*</span>

3. Again, in your preferred directory, clone either the below repo, or else clone a fork you made of it:
```
git clone https://github.com/ubyssey/ubyssey-dev.git
```
&nbsp;<span style="font-size:0.9em;">*(If you changed the docker image's name, make sure to also change it in docker-compose.yml in /ubyssey-dev/.devcontainer/)*</span>

4. Set up a persistent Docker volume. (This is used to persist the database contents between Docker containers, should you ever need to delete yours)
```
docker volume create --name=ubyssey_db_volume
```
5. Use the Remote Development plugin to open the ubyssey-dev.git directory as a container
6. If the database container isn't set up yet, connect to it:

```bash
docker exec -t -i ubyssey_db bash
```
This container may be named something other than ubyssey_db. If so, type `docker ps` to find what it is named. You can also connect to it without typing terminal commands if you download Docker Desktop.

Once connected, setup the local database in the container.

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
6. You should now be able to develop inside the Docker container and see your development version of the site on [localhost:8000](localhost:8000)

## What does setting up like this accomplish?

Starting out as a new coder on an ongoing coding project is generally a difficult and time-consuming process. Even if your general computer knowledge is strong, knowledge of the specific tools involved may still be weak, and there’s almost inevitably a lot of learning to do. Furthermore, because you’re likely to have your own computer, we have little control over what you have installed, creating the risk of wasting time doing a lot of individualized troubleshooting.

Because the Ubyssey depends so much upon volunteer work from students who are busy with school work, it is necessary coder **onboarding be made as fast as possible**, so volunteers do not get caught up on the “boring stuff” of simply getting a local development environment set up.  We therefore distribute **virutalized development machines** using containerization technology, specifically, Docker. This technology is often used in the tech industry alongside DevOps practices. The site and all its dependencies are packaged together in a “container”. Containers are a virtualization technology that can start up faster than traditional VMs, because many containers can share a single Linux kernel. The containerized website can run the same on any individual computer as it runs on the production environment.

## Software

### Docker

Install Docker (version `docker 19.03.x` is used in these instructions). Follow the instructions [from official Docker doc](https://docs.docker.com/). This will require you to create a docker account if you do not already have one.

Afterward install docker-compose (these instructions use `docker-compose 1.25.x`) from [here](https://docs.docker.com/compose/install/) (If not already installed with docker).

&nbsp;<span style="font-size:0.9em;">*(2020/06/29 - Possibly outdated note!) If setting up on linux, all docker and docker-compose commands should be preceeded with `sudo`.* To enable docker without `sudo`, follow this [official post-installation doc](https://docs.docker.com/install/linux/linux-postinstall/).*</span>

### Visual Studio Code

This sort of workflow is best done using [Visual Studio Code](https://code.visualstudio.com/), available for all major platforms, because Microsoft has developed a [Remote Development plugin](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack) designed to facilitate this.

### (Possibly outdated) Note for Mac

Docker has a [a known CPU overusage issue](https://github.com/docker/for-mac/issues/1759) for macOS that may make your fan go wild.

Jason, one of our volunteers, found [a trick](https://github.com/docker/for-mac/issues/1759) to fix the issue!

For performance boost, there's a a popular tool called [docker-sync](http://docker-sync.io/).

## Various How-Tos

### How to clone repositories
This is the github link (https://github.com/ubyssey). 

Clone the ubyssey.ca and the dispatch repositories to where-ever you prefer to work. This is typically done with the following terminal commands. If you forked the repository, use the URL of your fork. If you are working on a different branch, use the -b flag to specify which branch.

```bash
git clone https://github.com/ubyssey/ubyssey.ca.git
git clone https://github.com/ubyssey/dispatch.git
```

### How to build Docker images from scratch
To build a docker image from a Dockerfile, run in the directory containing the Dockerfile:
```
docker build . -t <dockerhub account>/<image name>:<tag>
```
Or:
```
docker build . -t <some other cloud hub>/<cloud account>/<image name>:<tag>
```
&nbsp;<span style="font-size:0.9em;">*(The first works because Docker Hub will be assumed if the cloud hub isn’t specified)*</span>

You can name the ***image*** whatever you like, but if you want to push it to the cloud, you should follow the above convention of  **\<dockerhub account>/\<image name>:\<tag>**

In our project:
* A Dockerfile is located in /ubyssey.ca/.
* There is also one in /dispatch/ too, but it layers Dispatch on top of the image built by the one in /ubyssey.ca/. To get this to work correctly, **make sure you name the image built from the /ubyssey.ca/ Dockerfile the same name as is in the /dispatch/** Dockerfile’s FROM instruction


### How to build static files with Gulp
Building static files with gulp is a step in front-end development which is somewhat analogous to compiling.

If you see that there is no CSS styling applied to the HTML in (http://localhost:8000/), you may need to set-up `ubyssey.ca/ubyssey/static/`

In the Docker container:
1) Install the required Node packages with npm:

```bash
cd /workspaces/ubyssey.ca/ubyssey/static/
npm install
```

2) Install a global version of gulp \(if you don't have it already\) and build the static files:

```bash
npm install -g gulp
gulp buildDev
```

_If you run into any error while installing npm or gulp, remove `ubyssey.ca/ubyssey/static/node_modules` by running `rm -rf node_modules`

### How to setup the mysql container with a database

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

_If you run into **operation errors**, drop the schemas in mysql database and repopulate it._

## Performing django migrations on the docker container

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

## Media Files

Download and unzip the [sample media folder](https://storage.googleapis.com/ubyssey/dropbox/media.zip) to `ubyssey-dev/ubyssey.ca/media/`. This will make it so the images attached to the sample articles are viewable.

## Using Dispatch

You can see Dispatch by going to `http://localhost:8000/admin/`

Username is `volunteer@ubyssey.ca`, password is `volunteer`

## Extra docker info

To see currently running docker containers, run the following command in a separate terminal.

```bash
docker ps
```

### When in doubt, you may need to clear docker's cache and remove all docker images.

```bash
# will remove docker cache and clear all images
docker system prune -a
```

then you can rebuild your docker images using

``` bash
# from ubyssey-dev dir
docker-compose up
```

### Using pdb with Docker
**Note: pdb is a command line debugging tool for python

First we need to update our `docker-compose.yml` to allow our `ubyssey-dev` container to connect with pdb. Make sure the django container looks like the following.

```yaml
  django:
    build: .
    command: bash -c "cd ubyssey.ca && python manage.py runserver 0.0.0.0:8000"
    volumes:
      - ./dispatch/dispatch:/dispatch/dispatch
      - .:/ubyssey.ca
      - ~/.config/:/root/.config
    volumes_from:
      - gulp
      - yarn
    ports:
      - "8000:8000"
      #######
      # PDB #
      #######
      - "4444:4444"
    depends_on:
      db:
        condition: service_healthy
    container_name: ubyssey-dev
    #######
    # PDB #
    #######
    stdin_open: true
    tty: true
  ```

Now kill any running docker containers, and rerun them with `docker-compose up`.

Add `import pdb` to the .py file you are interested in debugging, and set a breakpoint with `pdb.set_trace()`.

To step through the source, open a new terminal and connect to the `ubyssey-dev` container.

```bash
docker attach ubyssey-dev
```

the pdb output will start to display here when you hit your breakpoint.

For more information on pdb and how to use it, there is a helpful post at [RealPython](https://realpython.com/python-debugging-pdb/).
