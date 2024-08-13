# Creating Test Cases with Selenium

This guide will help you create and run test cases using Selenium in conjunction with Django's testing tools. Follow these steps to set up your test environment and generate the necessary data.

## 1. Add Relevant Data in the Admin

Before writing your test cases, ensure that all the relevant data is added to the Django admin. This will be the data your test cases will interact with.

## 2. Export the Database

Once you have added the necessary data in the admin, export the database to a JSON file.

```bash
python manage.py dumpdata > test_database_name.json
```

## 3. Write Your Test Cases

Refer to the Django testing documentation https://docs.djangoproject.com/en/5.0/topics/testing/tools/  and selenium documentation https://www.selenium.dev/selenium/docs/api/py/api.html
for detailed instructions on creating test cases


# Testing Setup

To ensure that the parameterized tests are working as expected, follow these steps:

1. **Rebuild Docker Compose**: 
   - First, rebuild your Docker Compose configuration to reflect the three new containers added to the `ubyssey-dev` repository.

2. **Run this command**:
  ```bash
  # Go to ubyssey.ca directory
  pip install -r requirements.txt
  ```
   


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
