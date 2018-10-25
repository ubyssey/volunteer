# FAQ

Table of contents:
- [CSS Not loading](#css-not-loading)
- [Debugging Django w/ Docker](#debugging-django-with-docker)
- [Django Migrations w/ Docker](#django-migrations-with-docker)

### CSS not loading

![css_not_loading](https://user-images.githubusercontent.com/9669739/47315444-73224980-d5f9-11e8-86ad-d8fa91404413.png)

There are often a case when CSS is not being loaded properly on development. Why is this happening :thinking:? If we look at Network tab in Developer Tools, we see that many `GET` requests are ending up as `404`.

![404_get](https://user-images.githubusercontent.com/9669739/47315630-03608e80-d5fa-11e8-9422-ea8263a7420b.png)

This is happening due to version difference between **`VERSION` variable in `settings.py`** and **`version` field in `package.json`**. We compile our static asset with [Gulp](https://gulpjs.com/), and it looks at `version` field in `package.json` as defined in [this line from gulpfile.js](https://github.com/ubyssey/ubyssey.ca/blob/eb4b406b462fdee5b36790fc22642f6e97f418ec/ubyssey/static/gulpfile.js#L17). However, Django looks at `VERSION` variable in its `settings.py` file.

To fix this issue, make sure [VERSION variable here](https://github.com/ubyssey/ubyssey.ca/blob/develop/_settings/settings-local.py#L10) and [version field here](https://github.com/ubyssey/ubyssey.ca/blob/develop/ubyssey/static/package.json#L3) are matching in your machine.

### Debugging Django with Docker

We will use `pdb` to debug python code ([quick intro](https://github.com/spiside/pdb-tutorial)). Add the following line to where you want to debug the code:

```python
import pdb; pdb.set_trace()
```

This will stop the code execution, but now we need to attach our terminal to the running django container. Before we do so, make sure the following two lines are in `docker-compose.yml` under `django` container config that allows the container to be attached for debugging:

```docker
stdin_open: true
tty: true
```

If those are in right place, run `docker attach ubyssey-dev ` to start debugging!


### Django Migrations with Docker

Whenever we want to update our database scheme, Django require us to do this through [migrations](https://docs.djangoproject.com/en/2.1/topics/migrations/)  :snake:

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
