# `evanmarshall.dev` Django Portfolio Web App

## Notes on Project Setup

### Setup Development Environment

- Create a new directory for the project and navigate to it, run: `mkdir <project-name> && cd <project-name>`. Make sure to replace both the `<`, `>`, and `project-name` with the name of your project. This will be assumed for the rest of the instructions.

- Create a virtual environment (ve) within the project directory (eg. **_dj-portfolio_**), run: `vecr venv` and append with desired ve name (default: **_venv_**). This command is an bash alias/function and it will create a **_venv_** folder in your working directory. Inside this directory, you’ll find several files, including a copy of the Python standard library. Later, when you install new dependencies, they’ll also live in this directory.

- Activate the virtual environment, run: `veact`. This will activate a virtual environment named **_venv_** by using Python’s built-in `venv` module. You can run: `echo $VIRTUAL_ENV` and/or `which python` to verify activation of your ve.

- Install Python package Django using **_pip_**, run: `piip <package-name>`. Append the bash alias/function with the package name (This can be used for installing any package with **_pip_**).

### Start the Django Project

A Django web application is made up of a project and its constituent apps.

- Create a new Django project within the project directory while the ve is running, run: `djs <project_name>`. By running the above command, you’ve told Django to create one `dj_portfolio` folder in the `dj-portfolio` directory. Your directory structure should look something like this:

```shell
dj-portfolio/
│
├── dj_portfolio/
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
│
├── venv/
│
└── manage.py
```

- Run the Django development server using `djrun`.

### Add the First App

- To create a new Django app within the Django project, run `djca <app-name>` and append with the app name. This command will create a `pages` directory (If you name the app **_pages_**) which contains the following files:

```shell
├── pages/
│   ├── __init__.py # tells Python to treat the directory as a Python package.
│   ├── admin.py # contains settings for the Django admin pages.
│   ├── apps.py # contains settings for the application configuration.
│   ├── models.py # contains a series of classes that Django’s ORM converts to database tables.
│   ├── tests.py # contains test classes.
│   └── views.py # contains functions and classes that handle what data is displayed in the HTML templates.
```

- Once you’ve created the app, you need to install it in your project (eg. In `dj-portfolio/dj_portfolio/settings.py`. To include an app in your Django example project, you need to add a reference to its **_configuration class_** at the beginning of the `INSTALLED_APPS` list in `settings.py`. By adding `pages.apps.PagesConfig`, you let Django know that the `pages` app that you just created exists.:

```python
INSTALLED_APPS = [
    "pages.apps.PagesConfig", # Add this line.
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
]
```

### Create a View

- A view in Django is a collection of functions or classes inside the `views.py` file in an app’s directory. Each function or class handles the logic that the app processes each time the user visits a different URL. Navigate to the `views.py` file in the `pages/` directory and add the code below:

```python
from django.shortcuts import render

def home(request):
    return render(request, "pages/home.html", {})
```

In this piece of code, you define a view function named `home()`. When you call this function, it’ll render an HTML file named `home.html`. That file doesn’t exist yet, but you’ll create it soon.

> [!NOTE]
> A view function takes at least one argument, `request`. The `request` object is created whenever a page loads, and it contains metadata about the page request.

Now that you’ve created the view function, you need to create the HTML template to display to the user. The `render()` function looks for HTML templates inside a directory called `templates/` in your app directory. Since templates of different apps can have the same names, it’s also best practice to add a subdirectory with the app’s name inside the `templates/` directory.

- Create the `template/` directory, a subdirectory named `pages/`, and subsequently a file named `home.html` inside it:

```shell
mkdir -p pages/templates/pages
touch pages/templates/pages/home.html
```

- Add your code to `home.html`.

### Add a Route

Now that you’ve created the template that your `home()` view function expects, you need to tell Django when to serve the view. For this, you need to create a new route to the project.

- Your `dj_portfolio/` folder contains a file named `urls.py`. In this file, you’ll include a URL configuration for the pages app:

```python
from django.contrib import admin
from django.urls import path, include # Add include.

urlpatterns = [
    path("admin/", admin.site.urls),
    path("", include("pages.urls")), # Add this line.
]
```

- By adding another `path()` to `urlpatterns`, you create a new route for your Django project. Whenever you visit the **_root_** path of your project, the routes of the pages application will load. The `pages.urls` module doesn’t exist yet, so you’ll need to create it: `touch pages/urls.py`.

- Inside `urls.py`, you need to import the `path` object as well as the app’s `views` module. Then you define a list of URL patterns that correspond to the various view functions. At the moment, you’ve only created one view function, so you only need to include one route:

```python
from django.urls import path
from pages import views

urlpatterns = [
    path("", views.home, name="home"),
]
```

### Add Third Party Framework to App

For this example we will use Bootstrap's CSS Framework. Before you get started with the Bootstrap styles, you’ll create a base template that you can import to each subsequent view. This template is where you’ll subsequently add the Bootstrap style imports.

- Create a directory named `templates/` in the `dj-portfolio/` folder. Inside this new directory, create a file named `base.html`:

```shell
mkdir templates/
touch templates/base.html
```

As you saw previously, each Django project can consist of multiple apps that handle separate logic, and each app contains its own `templates/` directory to store HTML templates related to the application. For templates that the _whole_ project shares, it’s a good idea to create a `templates/` directory in the **_root_** directory.

- Inside `base.html`, add the following lines of code:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>{% block title %}My Personal Portfolio{% endblock title %}</title>
    <link
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.1/dist/css/bootstrap.min.css"
      rel="stylesheet"
    />
  </head>
  <body class="container">
    {% block page_content %}{% endblock page_content %}
  </body>
</html>
```

All future templates that you create will extend `base.html` and inherit the Bootstrap styling on every page without having to import the styles again.

Another interesting bit in the `base.html` parent template is the use of `{% block %}` template tags. With this template tag, you can define content blocks that you can use or override in child templates that extend the parent template.

- To see this inheritance in action, you need to adjust the `home.html` template:

```html
{% extends "base.html" %} {% block page_content %}
<h1>Hello, World!</h1>
{% endblock page_content %}
```

To extend a parent template, you must use the `{% extends %}` tag at the beginning of a child template.

- Before you can see the inheritance and the new styled application in action, you need to tell your Django example project that `base.html` exists. The default settings register templates/ directories in each app, but not in the **_root_** directory itself. In `dj_portfolio/settings.py`, update `TEMPLATES`:

```python
TEMPLATES = [
    {
        "BACKEND": "django.template.backends.django.DjangoTemplates",
        "DIRS": [
            BASE_DIR / "templates/",
        ], # Add everything in the DIRS array.
        "APP_DIRS": True,
        "OPTIONS": {
            "context_processors": [
                "django.template.context_processors.debug",
                "django.template.context_processors.request",
                "django.contrib.auth.context_processors.auth",
                "django.contrib.messages.context_processors.messages",
            ]
        },
    }
]
```

The constant `BASE_DIR` is already defined in `settings.py`, and it points to your project’s root directory. Next, you **_join_** the path with the forward slash operator (`/`) from `pathlib` to point to the `templates/` directory and add it to the "`DIRS`" list.

> [!NOTE]
> Whenever you want create templates or import scripts that you intend to use in all of your Django apps inside a project, you can add them to this project-level directory and extend them inside your app templates.

### Add the Projects App
