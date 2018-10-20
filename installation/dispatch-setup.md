# Dispatch Development Setup

### Docker

Run docker-compose

```bash
# Move to your Ubyssey project dir
cd ~/ubyssey-dev
# Start docker containers
docker-compose up
```

### Mac / Windows

Uninstall the production version of Dispatch \(this was installed when you set up the `ubyssey.ca` repo\)

```bash
pip uninstall dispatch
```

Clone the dispatch repo and install it in "develop" mode

```bash
git clone https://github.com/ubyssey/dispatch.git

cd dispatch
pip install -e .[dev]
python setup.py develop
```

From the ubyssey.ca repo, run the "migrate" command to bring your database schema up to date:

```bash
python manage.py migrate
```

Use `yarn` to run the Dispatch Webpack build:

```bash
# Front-end manager app
cd dispatch/dispatch/static/manager

# Install dependencies
yarn setup

# Run Webpack in watch mode
yarn start
```


### Testing

```bash
# Link the tests to local files instead of package files
python setup.py develop

# Run tests
# make sure your current working directory is ubyssey-dev/dispatch
dispatch-admin test

# Generate coverage report
dispatch-admin coverage
```


