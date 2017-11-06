## Running the server

This is a quick guide for getting your development server running **after** you've completed the [setup process](/getting-started/installation.md).

### macOS

First, open a new Terminal window.

Start your MySQL server if it isn't running already:

```bash
mysql.server start
```

Change into the `ubyssey-dev` directory (the location depends on where you saved it):

```bash
cd ~/ubyssey-dev
```

Activate the virtual environment:
```bash
source bin/activate
```

Change into the ubyssey.ca directory:
```bash
cd ubyssey.ca
```

Run the server!
```bash
python setup.py runserver
```

**Building the static files**

Now, select `Shell > New Tab` from the Terminal menu bar. This will open a new shell in the `ubyssey.ca` folder. 

From this folder, navigate to `ubyssey/static`

```bash
cd ubyssey/static
```

Start the `gulp` script, which watches for changes to your source files and rebuilds all the static assets:

```bash
gulp
```

If you need to cancel the `gulp` script, simply press `control + c`.

### Windows

First, make sure your MySQL server is running. The easiest way to do this is with MySQL Workbench.

Next, open a new Command Prompt window and change into the `ubyssey-dev` directory.

```bash
cd ubyssey-dev
```

Activate the virtual environment:

```bash
.\Scripts\activate
```

Change into the `ubyssey.ca` directory:

```bash
cd ubyssey.ca
```

Run the server!

```bash
python manage.py runserver
```

**Building the static files**

Now, open a **new** Command Prompt window and navigate to the static files directory:

```bash
cd ubyssey-dev/ubyssey.ca/ubyssey/static
```

Start the `gulp` script, which watches for changes to your source files and rebuilds all the static assets:

```bash
gulp
```

If you need to cancel the `gulp` script, simply press `control + c`.

