# Overview

Welcome to Skill404—a learning platform where you’ll build multiple backend projects to strengthen your skills. While each project has different business requirements, they all share a few technical requirements:

1. Language Flexibility: You’re free to pick any programming language (Node.js, Python, Java, Go, etc.).

2. Database Usage:
You must use some database to store data (SQLite, Postgres, MySQL, MongoDB, etc.).

3. Containerization: Your application must run in a Docker container.

4. Single Command Spin-Up: We want to be able to launch your entire solution—API + database—with one command (e.g., docker-compose up --build).


Below you’ll find details on each of these points, along with references and tips for junior-to-mid-level engineers who might be newer to Docker, databases, or general dev-ops practices.

## Choosing Your Technology Stack

We believe in letting you pick the technology you’re most comfortable with or want to learn. However, you must ensure the following:

* Your chosen language can be packaged in Docker (practically every major language can).
You can connect to some database. If you’re using SQLite, your application can read the .db file within the same container. If you’re using a remote DB (Postgres, MySQL, MongoDB, etc.), you’ll define the service in `docker-compose.yml`.

## Containerization with Docker

Docker allows you to package your application and its dependencies into a single “image,” which can then be run as a “container” in any environment that has Docker installed. This ensures consistency: if it runs on your machine, it’ll run on someone else’s, too.

### Dockerfile
A Dockerfile is a list of instructions to build your image. For example, a Node.js project might have:

```Dockerfile
FROM node:18-alpine
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
```

* `FROM node:18-alpine` uses a lightweight Node.js image.
* `WORKDIR /usr/src/app` sets the working directory inside the container.
* `COPY package*.json ./` then `RUN npm install` installs dependencies.
* `COPY . .` copies source code.
* `EXPOSE 3000` makes port 3000 available.
* `CMD ["npm", "start"]` runs your app.

> **Remember**: If you use Python or Java, your Dockerfile instructions will differ (install Python or Java, copy your code, run pip install or Gradle/Maven, etc.).

## Docker Compose for Multi-Service Apps

### Why Docker Compose?

If your app only needs a single container (e.g., Python + SQLite in one image), you can run it with plain Docker. But if you want, say, Node.js + Postgres, you have two containers that must run together. Docker Compose simplifies this:

* You define a docker-compose.yml file with services for your app and any related containers (database, cache, etc.).
* One command (docker-compose up --build) spins up everything you need.

### Example docker-compose.yml
Below is an example for a Node.js + Postgres setup:

```yaml
version: '3.8'
services:
  app:
    build: .
    container_name: library_app
    ports:
      - "3000:3000"
    depends_on:
      - db
    environment:
      - DB_HOST=db
      - DB_PORT=5432
      - DB_NAME=library_db
      - DB_USER=postgres
      - DB_PASSWORD=postgres
  db:
    image: postgres:14
    container_name: library_db
    environment:
      - POSTGRES_DB=library_db
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
    ports:
      - "5432:5432"
    volumes:
      - db-data:/var/lib/postgresql/data

volumes:
  db-data:
```

This file declares two services:

* `app`: Built from the Dockerfile in the current directory (build: .), exposing port 3000, with environment variables that let your app connect to the db.
* `db`: A Postgres container with environment variables to set up the database name and user.

> When you run docker-compose up --build, both containers start, and your app knows where to find the DB via environment variables.

---

We recommend setting up a simple "hello world" api containerized with the tech stack you chose, and then move on to the functional requirements of a project.

If you are having any difficulties setting up, please open an issue on our [github repo](https://github.com/404skill/projects) and we'll try to help :)