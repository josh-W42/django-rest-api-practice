# Testing: Unit Tests

We'll be using the django test suite.

We would:

- setup tests per django app.
- run tests through docker compose

We'd use the following command:

```bash
docker-compose run --rm app sh -c "python manage.py test"
```
