# FAQ

Table of contents:
- [Django Migrations w/ Docker](#django-migrations-with-docker)

### Django Migrations with Docker

Normally you would run `python manage.py migrate` to run mingrations in typical Django migrations in your local machine. However, because our applications live inside [Docker containers](https://www.docker.com/resources/what-container), we need to somehow apply changes to the container image instead of your local machine. Use these commands to do so:

```bash
# Move into Ubyssey project dir
cd ~/ubyssey-dev

# Make sure to start docker containers. -d flag is for starting containers in the background
docker-compose up -d

# Attach current terminal session to Docker 
docker exec -t -i ubyssey-dev bash

# cd into ubyssey.ca project folder
cd ./ubyssey.ca

# Run migration
python manage.py migrate

# Exit from docker container
exit
```