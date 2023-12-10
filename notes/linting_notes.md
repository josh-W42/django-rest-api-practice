# How to add Linting:

In this tutorial, we're using the flake8 package to apply linting, since we cannot take advantage of linting in VScode.

- Install flake8 package.
- Run flake8 through docker compose.

```bash
docker-compose run --rm app sh -c "flake8"
```

No output would mean there's no linting suggestions / errors.

If there's errors, it's better to work from the bottom up.
