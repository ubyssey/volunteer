# Setting up Wagtail and getting started

## Table of contents
* [Starting up the server](#starting-up-the-server)
* [Setting up the Wagtail CMS](#setting-up-the-wagtail-cms)
* [Getting started on front-end development](#getting-started-on-front-end-development)
  * [Building static files with Gulp](#how-to-build-static-files-with-gulp)
* [Media files](#media-files)

&nbsp;

## Starting up the server

In the Terminal of VSCode, run the local server:

```bash
#change to the directory your project is located in
cd ubyssey.ca
python manage.py runserver
```

If at this step you encounter a message saying that you have `x amount of unapplied migrations`, please refer to the **Performing Django migrations on the Docker container** section on the [Docker](/installation/docker.md) page. Come back to this page after you have applied the migrations to your Docker database.

You should now be able to develop inside the Docker container. However, before you will be able to see your development version of the site on [localhost:8000](localhost:8000), you must configure the Wagtail CMS (content management system).


&nbsp;

## Setting up the Wagtail CMS

Going to [localhost:8000](localhost:8000) without setting up Wagtail, you will see a blank site that says 'Welcome to your new Wagtail site'.

You can see Wagtail by going to [http://localhost:8000/admin/](http://localhost:8000/admin/). Make sure your server is running.

The username is `volunteer@ubyssey.ca`, password is `volunteer`

1. Create a new root page (other than the currently existing 'Welcome to your new Wagtail site!'). Your new page should not be a child page of the default 'Welcome' site.
2. Change the site root to your newly created page in Settings -> Sites. [Wagtail Docs](https://docs.wagtail.org/en/v0.7/core_components/sites.html) here.

You should now be able to see an empty site resembling the live Ubyssey site at [localhost:8000](localhost:8000).

&nbsp;

## Getting started on front-end development

To 'compile' static files such as CSS during development, run `gulp buildDev ` in the folder where gulp is installed.

### How to build static files with Gulp
Building static files with gulp is a step in front-end development which is somewhat analogous to compiling.

If you see that there is no CSS styling applied to the HTML in (http://localhost:8000/), you may need to set-up `ubyssey.ca/ubyssey/static/`

In the Docker container (i.e. in the VSCode workspace Terminal):
1) Install the required Node packages with npm:

    ```bash
    cd /workspaces/ubyssey.ca/ubyssey/static_src/
    npm install -g gulp
    ```

2) To automatically build the static files whenever you make changes in your static files run this following command in the background:

    ```bash
    cd /workspaces/ubyssey.ca/ubyssey/static_src/
    gulp
    ```

_If you run into any error while installing npm or gulp, remove `ubyssey.ca/ubyssey/static_src/node_modules` by running `rm -rf node_modules`.

&nbsp;

## Media Files

Download and unzip the [sample media folder](https://storage.googleapis.com/ubyssey/dropbox/media.zip) to `ubyssey-dev/ubyssey.ca/media/`. This will make it so the images attached to the sample articles are viewable.
