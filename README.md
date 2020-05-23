# API Bootstrap utilizing MySQL and Express.js

## Table of Contents

1. [Setting up a project](#setting-up-a-project)
    - [Clone the repo](#clone-the-repo)
    - [Create the local environment files](#create-the-local-environment-files)
    - [Set up your database](#set-up-your-database)
2. [Installing Docker](#installing-docker)
    - [Ubuntu](#ubuntu)
    - [Windows and Mac](#windows-and-mac)
    - [Docker tips](#docker-tips)
3. [Recommended project file and folder structure](#recommended-project-file-and-folder-structure)
4. [Extra resources](#extra-resources)
     - [Dependency docs](#dependecy-docs)

## Setting up a project

### Clone the repo

Create a project folder and clone down the repo into your folder using:

```
git@github.com:jdsandahl/my-api-bootstrap.git
```

Install the minimum dependencies:

```
npm install
```

### Create local environment files

In the root folder of the project (where this readme is saved) create two files named .env and .env.test 

These files will need the follwing code inside:

```
    DB_PASSWORD=<YOUR_PASSWORD> 
    DB_NAME=<YOUR_APP_NAME> 
    DB_USER=root
    DB_HOST=localhost
    DB_PORT=3306
 ```

 - Your DB_PASSWORD should be the same for both files. 

 - Your DB_NAME in .env should be your real database and in .env.test this should be the name of your test database.

 - You do not need place these between "<" ">", this is just for highlighting.

 - There is a file named .env.example that can be used for reference when setting up, you may delete this file once it is no longer needed.

 ### Set up your database

This project requires a running MySQL database. 

This can be set up with Docker with a MySQL image by running: (Make sure to change the name and password!)

 ```
 docker run -d -p 3306:3306 --name my_api_mysql -e MYSQL_ROOT_PASSWORD=<YOUR_PASSWORD> mysql
 ```
- if you don't have Docker or the image see further down ['Installing Docker'](#installing-docker) for instructions.

Once this is running, you will need to create a database this should be the name used for DB_NAME in your .env file:

```
 docker exec -it YOUR_APP_NAME bash
```

From inside the container:

```
mysql -uroot -p
```
You will be prompted to enter your password. After your password has been correctly entered, you can then create the database with:

```
CREATE DATABASE YOUR_APP_NAME
```

You can then exit MySQL using ctrl+C and then again to exit the Docker container. 

If you've used the setup above your container should continue to run in the background while still freeing up your terminal. 

You're now ready to begin your project!

## Installing Docker

### Ubuntu

Update your software database: `sudo apt update`

Remove and old versions of docker tha might be on your system: `sudo apt remove docker docker-engine docker.io`

Install docker: `sudo apt install docker.io`

Check docker version: `docker --version`

### Windows and Mac

Docker requires a Linux kernel in order to run. This can be emulated on Windows and Mac. The easiest way to do this is to install [Docker Desktop](https://docs.docker.com/get-docker/). You will need to have Docker Desktop running in order to use docker commands in your terminal.
 
### Docker tips:

If you want to stop your Docker container session type:

```
docker stop my_api_mysql
```

To restart the session, you can type:

```
docker start my_api_mysql
```
- you can can also use the CONTAINER ID instead of the database name, this can be found using the process below.

To see a list of your running containers, all containers (running and stopped):
```
docker ps

docker ps -a
```

To view a list of your downloaded docker images you can run:
```
docker images 
```

## Recommended project file and folder structure
```
|___ ROOT/
    |--- .env
    |--- .env.test
    |--- .eslintrc
    |--- .gitignore
    |--- index.js
    |--- package-lock.json
    |--- package.json
    |--- README.md
    |--- test-setup.js

    |___ tests/
    |___ node_modules/
    |___ scripts/
        |--- create-database.js
        |___ drop-database.js

    |___ src/
        |--- app.js
        |___ controllers/
        |___ lib/
        |___ middleware/
        |___ models/
            |___ index.js 
        |___ routes/
 ```       

## Extra resources

### Depedency docs

- [Sequelize documentation and API reference](https://sequelize.org/master/class/lib/model.js~Model.html)

- [dotenv](https://www.npmjs.com/package/dotenv)

- [nodemon](https://www.npmjs.com/package/nodemon)

- [mocha](https://mochajs.org/)

- [chai](https://www.npmjs.com/package/chai)

- [SuperTest](https://www.npmjs.com/package/supertest)

- [eslint-config-mcr-codes](https://www.npmjs.com/package/eslint-config-mcr-codes?activeTab=readme)

