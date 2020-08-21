# FAQ

Table of contents:
- [FAQ](#faq)
    - [CSS not loading](#css-not-loading)
    - [Debugging Django with Docker](#debugging-django-with-docker)
    - [Django Migrations with Docker](#django-migrations-with-docker)
    - [Node-sass binding error](#node-sass-binding-error)
    - [Admin not loading](#admin-not-loading)
    - [How to access Docker MySQL console](#how-to-access-docker-mysql-console)

### CSS not loading

![css_not_loading](https://user-images.githubusercontent.com/9669739/47315444-73224980-d5f9-11e8-86ad-d8fa91404413.png)

There are often a case when CSS is not being loaded properly on development. Why is this happening :thinking:? If we look at Network tab in Developer Tools, we see that many `GET` requests are ending up as `404`.

![404_get](https://user-images.githubusercontent.com/9669739/47315630-03608e80-d5fa-11e8-9422-ea8263a7420b.png)

This is happening due to your local static file not built correctly.

To fix this issue, try to run `gulp buildDev` again in your **static_src** folder and clear your browser's cache!

### Debugging Django with Docker

(This trick works if you run docker-compose in detached mode (`docker-compose up -d`))

You can use `pdb` to debug python code ([quick intro](https://github.com/spiside/pdb-tutorial)) (think of `gdb` for your python code). Add the following line to where you want to debug the code:

```python
import pdb; pdb.set_trace()
```

This will stop the code execution, but now we need to attach our terminal to the running django container. Before we do so, make sure the following two lines are in `docker-compose.yml` under `django` container config that allows the container to be attached for debugging:

```docker
stdin_open: true
tty: true
```

If those are in right place, run the code until execution hit the `pdb.set_trace()` line.

Run `docker attach --detach-keys z ubyssey-dev` to start debugging, and exit out of the debugging session by pressing `z` button


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

### Node-sass binding error

You might run into an error that look something like this in gulp container while using our Docker setup:

```
Error: Missing binding /Users/Dan/Library/Application Support/Atom/dev/packages/source-preview-sass/node_modules/node-sass/vendor/darwin-x64-47/binding.node
Node Sass could not find a binding for your current environment: OS X 64-bit with Node.js 6.x
Found bindings for the following environments:

    * OS X 64-bit with Node.js 6.x
      This usually happens because your environment has changed since running `npm install`.
      Run `npm rebuild node-sass` to build the binding for your current environment.
```

In that case, change a line in your `docker-compose.yml` file where it says:

```yaml
gulp:
  build: .
    command: bash -c "cd ubyssey.ca/ubyssey/static && yarn && gulp"
```

to

```yaml
gulp:
  build: .
    command: bash -c "cd ubyssey.ca/ubyssey/static && yarn install --force && gulp"
```

### Admin not loading

When the admin page looks blank, that means the dispatch version is out of sync with ubyssey.ca repo

![blank_admin](https://user-images.githubusercontent.com/9669739/54364105-7a3e0880-4629-11e9-8bb8-b0218f13c852.png)

Solve this by going into dispatch folder, and running `python setup.py develop`

### How to access Docker MySQL console

In order to inspect local DB in docker, you need to attach command line session to running docker container. Run the following:

```bash
docker exec -it ubyssey_db bash -l
```

Once you're in, run the usual mysql command to run the console

```bash
# Pass is ubyssey
mysql -u root -p
```
