# API Bootstrap utilizing MySQL and Express.js

## Table of Contents

1. [Setting up a new project](#setting-up-a-new-project)
    - [Clone the repo](#clone-the-repo)
    - [Create local environment files](#create-local-environment-files)
    - [Set up your database](#set-up-your-database)
2. [Running the project](#running-the-project)
3. [Testing environment](#testing-environment)    
4. [Installing Docker](#installing-docker)
    - [Ubuntu](#ubuntu)
    - [Windows and Mac](#windows-and-mac)
    - [Docker tips](#docker-tips)
5. [Recommended project file and folder structure](#recommended-project-file-and-folder-structure)
6. [Extra resources](#extra-resources)
    - [Dependency documentation](#dependency-documentation)
    - [Dev dependency documentation](#dev-dependency-documentation)
    - [Miscellaneous](#miscellaneous)

## Setting up a new project

### Clone the repo

Create a project folder on your desktop and clone down the repo into your folder using:

```
git@github.com:jdsandahl/my-api-bootstrap.git your-project-folder-name

cd your-project-folder-name
```

Install the minimum dependencies:

```
npm install
```

### Create local environment files

In the root folder of the project (where this readme is saved) create two files named **.env** and **.env.test** 

These files will need the follwing code inside:

```
    DB_PASSWORD=<YOUR_PASSWORD> 
    DB_NAME=<YOUR_APP_NAME> 
    DB_USER=root
    DB_HOST=localhost
    DB_PORT=3306
 ```

 - Your **DB_PASSWORD** should be the same for both files. 

 - Your **DB_NAME** in **.env** should be your real database and in **.env.test** this should be the name of your test database.

 - You do not need place these between "< >", this is just for highlighting.

 - There is a file named **.env.example** that can be used for reference when setting up, you may delete this file once it is no longer needed for reference.

 ### Set up your database

This project requires a running MySQL database. 

This can be set up with Docker with a MySQL image by running: (**Make sure the password matches your DB_PASSWORD! You can also appropriately name the container more specific to your project**)

 ```
 docker run -d -p 3306:3306 --name my_api_mysql -e MYSQL_ROOT_PASSWORD=<YOUR_PASSWORD> mysql
 ```
- if you don't have Docker or the image see further down ['Installing Docker'](#installing-docker) for instructions.

Once this is running, you can manually create a database using the steps below, this should use the same name that has been entered used for DB_NAME in your .env file:

*Note: the below steps shouldn't be required if using nodemon and the included scripts, as these should create the database automatically from your .env files. The steps below are provided should there be an issue*

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

You can then exit MySQL using **ctrl+c** and then again to exit the Docker container. 

If you've used the setup above your container should continue to run in the background while still freeing up your terminal. 

**You're now ready to begin your project!**

## Running the project

After the initial setup you should be able to start a local server instance of the project using the following terminal command:

```
npm start
```

To end the localhost: from running you can use **ctrl+c** in the terminal to stop the server

## Testing environment

This bootstrap uses Mocha and SuperTest to perform unit tests, it is also recommended to use [Postman](https://www.postman.com/) for further end to end tests on the live database. 

Running the following command will start the tests:

```
npm test
```

When fist setting up the project, you should see the below error. 
```
Error: No test files found: "__tests__/**/*.test.js"
npm ERR! Test failed.  See above for more details.
```

This shows that the test environment is setup correctly, but is not running because you've not created any test files.

**Time to create your unit tests**


## Installing Docker

### Ubuntu

Update your software database: `sudo apt update`

Remove and old versions of docker tha might be on your system: `sudo apt remove docker docker-engine docker.io`

Install docker: `sudo apt install docker.io`

Check docker version: `docker --version`

### Windows and Mac

Docker requires a Linux kernel in order to run. This can be emulated on Windows and Mac. The easiest way to do this is to install [Docker Desktop](https://docs.docker.com/get-docker/). You will need to have Docker Desktop running in order to use docker commands in your terminal.
 
### Docker tips:

If you want to **stop** your Docker container session type:

```
docker stop my_api_mysql
```

To **restart** the session, you can type:

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

Upon initial set-up, you will not see all of the files and folders outlined below.

Some of these files will be created after the bootstrap has been cloned and downloaded using the steps outlined above, others will need to be created based on your project's actual needs. 

Below is a hypothetical file structure to be used as guideline when putting together a new project. 

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

    |___ __tests__/
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

### Dependency documentation

- [Express.js](https://expressjs.com/en/4x/api.html)
- [Sequelize documentation and API reference](https://sequelize.org/master/index.html)
- [mysql2](https://www.npmjs.com/package/mysql2)
- [dotenv](https://www.npmjs.com/package/dotenv)

### Dev dependency documentation

- [nodemon](https://www.npmjs.com/package/nodemon)
- [mocha](https://mochajs.org/)
- [chai](https://www.npmjs.com/package/chai)
- [SuperTest](https://www.npmjs.com/package/supertest)
- [eslint-config-mcr-codes](https://www.npmjs.com/package/eslint-config-mcr-codes?activeTab=readme)

### Miscellaneous

- [Postman documentation](https://learning.postman.com/docs/postman/launching-postman/introduction/)
- [MySQL Workbench](https://dev.mysql.com/downloads/workbench/)