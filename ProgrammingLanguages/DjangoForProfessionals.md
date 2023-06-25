# 2 hello-world
## Code Snippets
```bash
# Docker commands
docker run hello-world
docker info
docker build .
#   the dot indicates that the Dockerfile is in the current working directory
docker-compose up -d
docker-compose down
```

Dockerfile
```dockerfile
# Pull base image
FROM  python:3.10.6-slim-bulleye
#   slim indecates using the smaller variant of Python interpreter
#   bulleye refers to a Debian release

# Set environment variables
ENV PIP_DISABLE_PIP_VERSION_CHECK 1
#   disables the version check of pip
ENV PYTHONDONTWRITEBYTECODE 1
#   do not write .pyc files
ENV PYTHONUNBUFFERED 1
#   console output is not buffered by Docker

# Set working directory in the container
WORKDIR /code

# Install dependencies
COPY ./requirements.txt .
#   COPY takes two arguments. First is the file(s) which is going to copy into
#   the docker image. The second is where the file(s) is is going to copy to.
RUN pip install -r requirements.txt

# Copy project
COPY . .
```

docker-compose.yml
```yaml
# specifies the version of Docker Compose
version: "3.9"

# dockers included in the docker container
services:
  web:
# look in the current directory for Dockerfile
    build: .
    ports:
      - "8000:8000"
    command: python manage.py runserver 0.0.0.0:8000
# synchronize the container with the local filesystem
    volumes:
      - .:/code
```

## Bullet Points
- Docker is a way to implement Linux Containers. Virtual Environment can only
  isolate Python packages.
- Start a Django project with Docker:
    1. Create a Virtual Environment: `python -m venv .venv`.
    2. Activate the Virtual Environment.
    3. Install Django module: `pip install django`.
    4. Start a project: `django-admin startproject`.
    3. Create `requirements.txt` file: `pip freeze > requirements.txt`.
    5. Deactivate the Virtual Environment.
    6. Create `Dockerfile` and `.dockerignore` files.
    8. Build docker image: `docker build .`.
    9. Create `docker-compose.yml` file
    3. Start the Docker container: `docker-compose up`.
- A docker image is a read-only templete that describes how to create a Docker
  container.
- `Dockerfile` defines the steps to create and run the custom image.
- `.dockerignore` specifies files and directories which should not be included
  in the docker image.
- Docker commands should be executed with root previlege.

# 3 PostgreSQL
## Code Snippets

## Bullet Points
- To execute commands within dockers, prefix the traditional commands with
  `docker-compose exec [service]`.
- `-d` option of `docker-compose` enables detached mode, which hides the
  commandd output. If you want to check the output, use `docker-compose log`
  command.
