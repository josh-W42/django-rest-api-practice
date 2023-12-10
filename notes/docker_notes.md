# Docker

### Why use docker?

- Consistent dev and prod environment.
- Easier collaboration
  I didn't know this, but you can leverage docker while collaborating
  with other developers to ensure that everyone is using the exact same
  dev environment. NO MORE IT WORKS ON MY MACHINE!
- Capture all dependencies as code.
- Python requirements
- OS dependencies
- Easier Cleanup
  When a project is complete, you can just remove the docker file
  everything is gone!

### Why use Docker + Django

#### Many Benefits:

- A lot of the above.
- It just saves time

#### Drawbacks

- VSCode is unable to access interpreter
- More difficult to use integrated features (Linting + Interactive Debugger)
- To address the above, it is suggested that we use the terminal to apply things like linting for example.

### How to configure Docker?

1. Create Dockerfile - All OS deps for the project
2. List steps for creating an image:
3. Choose a base image (python)
4. Install deps
5. Setup Users (linux users or administrators)

6. Set up Docker Compose

- This defines how docker should be used
- Defines our services (app, app/recipes)
- Sets up Port mappings.
- Sets up Volume mappings.

Example on how to use Docker Compose

```bash
docker-compose run --rm app sh -c "python manage.py collectstatic"
```

Explanation:

- `docker-compose` runs a docker Compose command.
- `run` will start a specific container defined in config.
- `--rm` removes the container. (once it's finished running, important if you're just running a single command as this will prevent a build up of commands on your system0)
- `app` is the name o the app.
- `sh -c` passes in a shell command. (used mainly for one command to run in shell)
- Command to run inside container. (the bit in parenthesis)

### Additional Files Explanation

- Requirements.txt

  - We have to specify the required python deps and what versions we want them to be downloaded at.
  - Here we specify the minimum version and maximum versions.
  - Here we stop the max version to stop before the next major version, as it could contain breaking changes. However, we'll have all of the patch versions we'll need.

- Dockerfile

Image

- We start by defining the image "FROM"...
- we could input any base image available on the docker hub page.
- alpine is name of the docker image
- We're using the python 3.9 version.
- alpine3.13 is the tag used for that image.
- alpine is a lightweight version of linux and it is recommended for docker images because it is very strict. (May have changed over time)

Maintainer

- This could be a developer or a website but the best practice is to define that to inform other developers who is maintaining the container.

ENV - PYTHON UNBUFFERED

- This is recommended when you're running python in a container.
- It essentially tells python that you dont want to buffer the output, instead printed it directly to the console.
- Prevents delays

COPY ./requirements.txt /tmp/requirements.txt

- Copy our requirements from local directory to docker image.

COPY ./app /app

- Copy our local django app to the django app on the docker image

WORKDIR /app

- Specifies the app directory as the working directory
- We do this so when we run commands for our app we can exclude most of the PATH to the app.

EXPOSE 8000

- Expose this PORT to our machine when we run the container.
- We can connect to the django development server through this PORT.

1. RUN python -m venv /py && \
2. /py/bin/pip install --upgrade pip && \
3. /py/bin/pip install -r /tmp/requirements.txt && \
   _NEW_
   if [ $DEV = "true" ]; \
    then /py/bin/pip install -r /tmp/requirements.dev.txt ; \
    fi && \
   _NEW_
4. rm -rf /temp && \
5. adduser \
6. --disabled-password \
7. --no-create-home \
8. django-user

- Runs a command when the alpine image is building
- It's a single command that creates one image layer and does a few things.

1. -> Creates a new virtual environment to store deps. (Sometimes you might not need this because it's in docker but in some cases where deps could cause issues then this is a safeguard against that)
2. -> upgrades pip for the virtual env
3. Install requirements file.
4. -> Remove the tmp directory. (we do this to keep this docker image lightweight, saves speed during deployment)
5. It is NOT recommended run the application as the ROOT USER; in the event this application becomes compromised then the attacker would have root level access and no restrictions. That's bad :)
6. we're not specifying password, so no one has to enter a password to access a container. (not nessasary in this case)
7. We specify that we do not want to create a home directory for the user, because it is for this use case and we want to the container to be as lightweight as possible.
8. This is when we specify the new user's name.

- _NEW_ This section essentially just downloads the packages we have listed in our requirements.dev.txt file if we're in a DEV environment.

ENV PATH="/py/bin:$PATH"
Updates the PATH environment variable; for reference the PATH environment variable in linux is defines all the executables that can be run. So when we run python commands it will run directly from our virtual environment.

USER django-user
Should be the last line of this dockerfile. It specifies the user that we will be switching to.

#### Building the app using the docker command.

```bash
docker build .
```

## Docker Compose

#### Explaining yml file:

1.  version: '3.9'

2.  services:
3.  app:
4.                           build:
5.                             context: .
6.                           ports:
7.                             - '8000:8000'
8.                           volumes:
9.                             - ./app:/app
10. command: >
11.       sh -c "python manage.py runserver 0.0.0.0:8000"

12. Version of the docker compose syntax to use.
13. docker compose file typically consist of one or more services.
14. Name of application
15. see bellow
16. Build a docker file at the current directory.
17. see bellow
18. Maps PORT 8000 on our local machine to the PORT 8000 in docker container.
19. see bellow
20. Volumes are a way of mapping directories from our system to our container.
21. see bellow.
22. The command to be run. Can be overwritten when we use `docker-compose run`

#### Building the app using docker-compose

```bash
  docker-compose build
```
