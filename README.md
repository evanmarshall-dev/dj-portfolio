# `evanmarshall.dev` Django Portfolio Web App

## Notes on Project Setup

1. Create a new directory for the project and navigate to it.
2. Create a virtual environment within the project directory. Append with desired venv name (default: venv) run: `vecr venv`.
3. Activate the virtual environment, run: `veact`.
4. Install Python package Django using pip, run: `piip Django`.
5. Start a new Django project within the current directory, run: `djs <project_name>`.
6. Run the Django development server using `djrun`.
7. Create a new Django app within the project. Append with app name: run `djca <app-name>`.
8. Once you’ve created the app, you need to install it in your project (eg. In `dj-portfolio/dj_portfolio/settings.py`, add the following line of code under `INSTALLED_APPS`: `INSTALLED_APPS = ["pages.apps.PagesConfig","django.contrib.admin",...]`).
9. Create view (Collection of functions or classes that handles logic that app processes each time user visits a URL). When you call view function it renders the file specified in return statement.
10. Create template. The render function looks for said template in the templates folder within the app directory. Best practice is to create a folder within templates named after the app (eg. pages) and place all HTML files there.
11. Now that the template is created that home function expects, we need to tell django to serve the view by adding a route. This is done in the `urls.py` within the app directory (eg. Within the `dj_portfolio` dir).
12. Create `urls.py` within the app directory (eg. `pages/urls.py`) and add the route for the home view function.
