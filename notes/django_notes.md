# Creating a Django Project w/ Docker Compose:

Run the following command to create a new django project inside our docker container in the current directory.

```bash
docker-compose run --rm app sh -c "django-admin startproject app ."
```

To start the development server run the following command:

```bash
docker-compose up
```
