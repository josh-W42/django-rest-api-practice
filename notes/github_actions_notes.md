# Github Actions Exp:

#### Inside checks.yml

```yml
--- # This line specifies that this is a yml file
name: Checks # Name that will appear in github actions

# The trigger - any push of changes to the github project.
on: [push]

# The processes that should occur
jobs:
  # ID of the job - could be referenced elsewhere in gh-actions.
  test-lint:
    # Name of the action
    name: Test and Lint
    # The "Runner" OS that will run this job on.
    run-on: ubuntu-20.04
    steps:
      # Name of the step
      - name: Login to Docker Hub
        # Allows you to use another predefined action called "docker/login-action@v1"
        uses: docker/login-action@v1
        # Add in credentials.
        with:
          # use github secrets we've defined before.
          username: ${{ secrets.DOCKERHUB_USER }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
      # Action provided by gh that will checkout the code. (So we can apply unit tests and linting)
      - name: Checkout
        uses: actions/checkout@v2
      - name: Test
        run: docker-compose run --rm app sh -c "python manage.py test"
      - name: Lint
        run: docker-compose run --rm app sh -c "flake8"
      # If any of the above fails then the action will fail as well
```
