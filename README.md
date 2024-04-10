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

> [!NOTE]
> TODO: show how to make changes to the application and deploy updates.

> [!NOTE]
> TODO: demonstrate Dependabot.
