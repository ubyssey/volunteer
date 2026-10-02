# Setting up Wagtail and getting started

## Table of contents
* [Starting up the server](#starting-up-the-server)
* [Setting up the Wagtail CMS](#setting-up-the-wagtail-cms)
* [Getting started on front-end development](#getting-started-on-front-end-development)
  * [Building static files with Gulp](#how-to-build-static-files-with-gulp)
* [Media files](#media-files)

&nbsp;

## Starting up the server

Now we need to set up the server. Go back to the VS Code window running in the `django` container. (If you need to reopen it, then open the `ubyssey-dev` folder in VS Code, open the command palette, and do "Dev Containers: Reopen in Container.") Your terminal in VS Code should now be in the proper container.

Alternatively, you can open a local terminal on your computer and run `docker exec -t -i ubyssey-dev_devcontainer-django-1 bash`.

In the container terminal, run:

```bash
cd ubyssey/static_src
npm install -g gulp
npm install
gulp
cd /app
python manage.py migrate
python manage.py createsuperuser
# Enter whatever email and password you want; this is only for the server running locally on your computer.
python manage.py runserver
```

This sets up `gulp`, which compiles the CSS, and sets up Wagtail. You should now be able to develop inside the Docker container. However, before you will be able to see your development version of the site on [localhost:8000](localhost:8000), you must configure the Wagtail CMS (content management system).

&nbsp;

## Setting up the Wagtail CMS

Going to [localhost:8000](localhost:8000) without setting up Wagtail, you will see a blank site that says 'Welcome to your new Wagtail site'.

You can see Wagtail by going to [http://localhost:8000/admin/](http://localhost:8000/admin/). Make sure your server is running. Login with the email and password you created in the last step.

### Create a home page

1. Go to [http://127.0.0.1:8000/admin/pages/](http://127.0.0.1:8000/admin/pages/)
3. Click the + next to "Root" at the top
4. Add a home page
5. Name your new page "The Ubyssey"
6. Next to "Save draft" at the bottom, click the arrow and hit "Publish"
7. Go to [http://127.0.0.1:8000/admin/sites/edit/1/](http://127.0.0.1:8000/admin/sites/edit/1/)
8. Change the root page to The Ubyssey

### Create an author

1. Go to [http://127.0.0.1:8000/admin/pages/3/](http://127.0.0.1:8000/admin/pages/3/)
2. Click the + next to "The Ubyssey" at the top
3. Add an author management page named "Authors"
4. Publish it
5. Back on the list of pages, click the + on the right side next to "Authors" (_not_ the + at the top)
6. Now click the + at the top
7. Name the new author whatever you want, and publish it.

### Create a section
1. Go to [http://127.0.0.1:8000/admin/pages/3/](http://127.0.0.1:8000/admin/pages/3/)
2. Click the + at the top
3. Add a section page named "News" and publish it

You should now be able to see an empty site resembling the live Ubyssey site at [localhost:8000](localhost:8000). You should also see an empty Stove interface at [localhost:8000](localhost:8000). Try creating a new article using the panel on the right side; you'll need a title, author, and section. You can then click on it and play around in the manuscript editor. 

Once you're ready to publish it to your local server, click on "View in Wagtail" on the top-right, and then publish it with the button on the bottom. You should see it on [localhost:8000](localhost:8000), in which case you're all set!

## Future Development

You always want to develop in your `django` container. How exactly you get here might depend on your VS Code config, but you can always open the `ubyssey-dev` folder in VS Code locally on your computer and open it in a dev container from the command palette. Within the container, the code all lives in the `/app` folder.

You'll need to run two different processes in two different terminals; you can use the VS Code integrated terminals, which you can show and hide by doing ``ctrl+` `` (or ``cmd+` `` on a Mac). The terminal prompt should look like `root@c7270aed905d:/app#` but with a different random alphanumeric container ID.

In one terminal, run:

```bash
cd /ubyssey/static_src
gulp
```

This will leave `gulp` running in the background. Once you see `Starting 'watchTask'...` you're all set; just leave this running.

Now create another terminal with the + button in the top right of the VS Code terminal pane. You should see the same terminal prompt as before. Run:

```bash
python manage.py runserver
```

You're all set to develop now. Once you're done, just go to each of these two terminals and do `ctrl+c` to end these processes. (In this case, it's also `ctrl` on a Mac, not `cmd`!)

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
