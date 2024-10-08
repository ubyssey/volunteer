This guide will help you create and run test cases using Selenium in conjunction with Django's testing tools. Follow these steps to set up your test 
environment and generate the necessary data.

## How to write good test cases?
Generally test cases should be written for the following to reasons mainly:
1. To test a part of the website that has broken frequently. This is called regression testing.
2. To test most critical aspects of website which if broken could be costly for the buisness of the website.

## Create test cases
### 1. Add Relevant Data in the Admin

Before writing your test cases, ensure that all the relevant data is added to the Django admin. This will be the data your test cases will interact with.

### 2. Export the Database

Once you have added the necessary data in the admin, export the database to a JSON file.

```bash
python manage.py dumpdata > test_database_name.json
```

### 3. Write Your Test Cases

Add the database to the `fixtures` folder in `tests_ubyssey` directory. Then you can start the static live server that Django provides which runs in the 
background and feed it the database you created as a fixture. Then you can do the test setup similar to what is done in `tests.py` file in `tests_ubyssey` 
directory. For more details kindly refer to the [Django testing documentation](https://docs.djangoproject.com/en/5.0/topics/testing/tools/)  and [selenium 
documentation](https://www.selenium.dev/selenium/docs/api/py/api.html) for detailed instructions on creating test cases.


# Testing Setup

To ensure that the parameterized tests are working as expected, follow these steps:

1. **Rebuild Docker Compose**: 
   - First, rebuild your Docker Compose configuration to reflect the three new containers added to the `ubyssey-dev` repository.

2. **Run this command**:
  ```bash
  # Go to ubyssey.ca directory
  pip install -r requirements.txt
  ```

3. **Collect static files**
   - If you don't have a directory `ubyssey.ca/static`, then navigate to `ubyssey.ca/config/settings/base.py` and uncomment out the line `'whitenoise.runserver_nostatic'` in `INSTALLED_APPS`. Then run the command `python manage.py collectstatic` in the `ubyssey.ca` directory in terminal. This will create the directory `ubyssey.ca/static` and populate it with the project's static files. These static files have to exist in order to run the tests because the test server is run with `DEBUG=False` which changes where the static files are expected to be. 
   


##  Run the Tests

### Testing in Chrome

1. **Start the Chrome Container**:
   - In Docker Desktop, start the `selenium/standalone-chrome:latest` container.

2. **Run the Tests**:
   - Set the browser environment variable to Chrome and run the tests:
     ```bash
     # Go to the ubyssey.ca dir
     export BROWSER=chrome && python manage.py test
     ```

### Testing in Firefox

1. **Start the Firefox Container**:
   - In Docker Desktop, start the `selenium/standalone-firefox:latest` container.

2. **Run the Tests**:
   - Set the browser environment variable to Firefox and run the tests:
     ```bash
     # Go to the ubyssey.ca dir
     export BROWSER=firefox && python manage.py test
     ```

### Testing in Edge

1. **Start the Edge Container**:
   - In Docker Desktop, start the `selenium/standalone-edge:latest` container.

2. **Run the Tests**:
   - Set the browser environment variable to Edge and run the tests:
     ```bash
     # Go to the ubyssey.ca dir
     export BROWSER=edge && python manage.py test
     ```

By following these steps, you will be able to verify that the parameterized tests work correctly across different browsers.
