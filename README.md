# Introduction

IMHO, most Django tutorials have two gaps:

* They do not use "modern" tools to manage dependencies.
* They do not explain how to deploy your application "publicly".

(The [Django Girls Tutorial](https://tutorial.djangogirls.org/en/) explains how to deploy Django to <https://www.pythonanywhere.com/>.
That tutorial is friendlier for novices, so I recommend doing it first.
Poetry and Render add more complexity, so the Django Girls Tutorial can be an intermediate step to do this tutorial.
Also, PythonAnywhere is an environment designed for teaching, while Render is an environment more similar to typical modern production environments.)

This project explains how to create a Django project using [Poetry](https://python-poetry.org/) and deploy to <https://render.com>.

This repository contains a starter project that you can use to follow the tutorial, and to base your projects on.

The repository contains the "blank" application that the Django tools create.
The blank application only contains a Django admin to manage users and nothing else.

# Deploying this application.

1. Fork this repository in GitHub.
2. Register for a new account in <https://render.com/>, with GitHub.
3. Go to [the Blueprints tab](https://dashboard.render.com/blueprints) in the Render administration website, then click "new Blueprint instance".
4. Click the "connect repository" button.
5. If you are prompted "where do you want to install Render?", you will be shown your user and the GitHub organizations you belong to.
   Select your user.
6. If you install on your personal account, choose "only select repositories" and select your fork of this repository.
   Then click "install".
7. You should be back to render, and your fork should be listed.
   Click "connect" for your repository.
8. Choose a Blueprint name, select the `main` branch, and click "apply".
   This can take a few minutes to complete.
9. Once the process completed, click "managed resources", then click the "mysite" resource.
10. Render should display a "deploy live" event. Click on "deploy".
11. This will show the logs of the deployment and from the application.
    The end show display "your service is live".
12. At the top of the page, click a link <https://mysite-xxx.onrender.com/>, where `xxx` varies.
13. This should open a "not found" page.
    Edit the URL to add `/admin` at the end and press enter.
    The admin login page displays.

At this point, your application is deployed.
However, you cannot access the admin site because the database is blank and contains no users.
To create a user:

1. Go back to the Render dashboard, then click the "mysite" service.
2. Select the "environment" section, then click "add environment variable".
3. The name of the variable is `ADMIN_PASSWORD`, click the "generate" button to generate a random value (or enter a *safe* password).
   You can always copy the password from this page (until you remove this variable in a later step).
   Click "save changes".
4. At the top of the page, select "manual deploy" then "deploy latest commit".
5. The logs will show the invocation of the `createsuperuser` Django command.
6. The deployment can take a few minutes, then the logs display "your service is live".
7. Visit the admin site again and login with the `admin` user and the password that you used in a previous step.

Once everything is configured, if you push changes to the repository, Render deploys the new changes automatically.

# Running the application locally

You can also run the application locally for development, because the application settings detect whether you are running the application locally or on Render, and change the configuration accordingly.
When running locally, debug mode is on, and the application uses an SQLite database instead of PostgreSQL to reduce the amount of dependencies to configure on your development environment.
Using different database types can cause issues with more complex applications.

# How this works

The following files deviate from a blank Django application so that the application can be deployed to Render.

* [`example/settings.py`](example/settings.py): A Django settings module with customizations for development and the Render deployment.
* [`build`](build): A Python script that Render runs to build the application.
  The script performs the following tasks:
  * Installs the application to the Python installation that Render runs.
  * Runs `manage.py collectstatic` to serve static files in production mode.
  * Runs Django database migrations.
  * If the `ADMIN_PASSWORD` variable is set, creates a super user.
* [`poetry.lock`](poetry.lock) and [`pyproject.toml`](pyproject.toml) use Poetry to package the application.
  These files control the dependencies that the application uses, and their versions.
  The Poetry "extras" feature is used to declare dependencies that are only required for production, but not for development.
* [`render.yaml`](render.yaml): contains the blueprint definition for Render.
  Render follows the blueprint to create the application and the database.

Refer to the comments on the files for further information.
