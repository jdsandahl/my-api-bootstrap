# API Bootstrap utilizing MySQL and Express.js

This project is a boilerplate for creating a Node.js/Express API environment that uses MySQL as a database. Also included is the testing packages Mocha and Supertest, it's recommended to install [Postman](https://www.postman.com/) for further end-to-end testing on live endpoints.  

## Table of Contents

1. [Setting up a new project](#setting-up-a-new-project)
    - [Clone the repo](#clone-the-repo)
    - [Create local environment files](#create-local-environment-files)
    - [Set up the database](#set-up-the-database)
2. [Running the project](#running-the-project)
3. [Testing environment](#testing-environment)    
4. [Installing Docker](#installing-docker)
    - [Ubuntu](#ubuntu)
    - [Windows and Mac](#windows-and-mac)
    - [Docker tips](#docker-tips)
5. [Recommended project file and folder structure](#recommended-project-file-and-folder-structure)
6. [Deploying to the cloud using Heroku](#deploying-to-the-cloud-using-heroku)
    - [Installing the Heroku CLI](#installing-the-heroku-cli)
    - [Logging in](#logging-in)
    - [Creating the app](#creating-the-app)
    - [Adding a database](#adding-a-database)
    - [Creating a Procfile and modifying the boilerplate](#creating-a-procfile-and-modifying-the-boilerplate)
7. [Extra resources](#extra-resources)
    - [Dependency documentation](#dependency-documentation)
    - [Dev dependency documentation](#dev-dependency-documentation)
    - [Miscellaneous](#miscellaneous)

## Setting up a new project

### Clone the repo

Create a project folder on your desktop and clone down the repo into your folder using:

```
git clone git@github.com:jdsandahl/my-api-bootstrap.git your-project-folder-name

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

 ### Set up the database

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

You can then exit MySQL by typing `exit` and pressing **enter** and then again to exit the Docker container. 

If you've used the setup above, your container should continue to run in the background while still freeing up your terminal. 

**You're now ready to begin your project!**

*[return to table of contents](#table-of-contents)*

## Running the project

After the initial setup you should be able to start a local server instance of the project using the following terminal command:

```
npm start
```

To end the localhost: from running you can use **ctrl+c** in the terminal to stop the server

*[return to table of contents](#table-of-contents)*

## Testing environment

This bootstrap uses Mocha and SuperTest to perform unit tests, it is also recommended to use [Postman](https://www.postman.com/) for further end to end tests on the live database. 

Running the following command will start the tests:

```
npm test
```

When first setting up the project, you should see the below error. 
```
Error: No test files found: "__tests__/**/*.test.js"
npm ERR! Test failed.  See above for more details.
```

This shows that the test environment is setup correctly, but is not running because you've not created any test files.

**Time to start creating some unit tests!**

When begining to add tests to the project you may need to add the following extra flag to the test script in the **package.json** file to make sure your test databases are set up when running the test:

```
--file ./__tests__/test-setup.js
```

The full test script in package.json should now look like the following:

```
   "test": "NODE_ENV=test mocha __tests__/**/*.test.js --exit --recursive --timeout 6000 --file ./__tests__/test-setup.js",
```

*[return to table of contents](#table-of-contents)*

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
- you can can also use the CONTAINER ID instead of the container name, this ID can be found using the process below.

To see a list of your running containers, all containers (running and stopped):
```
docker ps

docker ps -a
```

To view a list of your downloaded docker images you can run:
```
docker images 
```

*[return to table of contents](#table-of-contents)*

## Recommended project file and folder structure

Upon initial set-up, you will not see all of the files and folders outlined below.

Some of these files will be created after the bootstrap has been cloned and downloaded using the steps outlined [above](#setting-up-a-new-project), others will need to be created based on your project's actual needs. 

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

    |___ __tests__/
        |__ test-setup.js
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

*[return to table of contents](#table-of-contents)*

## Deploying to the cloud using Heroku
[Heroku](https://www.heroku.com) is a cloud platform as a service, to which apps built with this bootstrap can be deployed to so that the application can be served in the cloud. 

You will need to sign up for an account on the Heroku website, there is a free tier along with paid options. 

Once you've created an account, you can begin installing the Heroku command line tool.

### Installing the Heroku CLI
#### Mac
To install on a Mac, open a terminal and type the following:
```
brew tap heroku/brew && install heroku
```

#### Ubuntu
To install on Ubuntu/Linux, from the terminal type:
```
sudo snap install --classic heroku
```

#### Windows
You can get the installer from [here](https://devcenter.heroku.com/articles/heroku-cli#download-and-install)

### Logging in
Type the following line, which will prompt you to enter your password for the Heroku account that has been setup:
```
heroku login -i
```

### Creating the app
From the project root in the terminal type:
```
heroku create APP-NAME
```
Heroku can also randomly generate a unique name if you don't provide an APP-NAME.

To double check that a Heroku app has been created you view the list of git remotes associated to the project:
```
git remote -v
```

### Adding a database
Using the ClearDB add-on option that Heroku provides, a database can be added by typing:
```
heroku addons:create cleardb:ignite
```

Database credentials are kept as an environment variable and can be viewed with:
```
heroku config | grep CLEARDB_DATABASE_URL
```

### Creating a Procfile and modifying the boilerplate
In the project root create a new file named `Procfile`, this file does not have an extention, so something like `Procfile.txt` will be **invalid**.

Once the Procfile has been created, type the following and save the file:
```
web: node index.js
``` 

This file tells Heroku to start the app by running the command `node index.js`. 

**If you don't include a Procfile, Heroku will then check and attempt to run the available "start" script in package.json, which means modifying the scripts, using a Procfile requires less modification**

In **/src/app.js** change APP_PORT to the below:
```
const APP_PORT = process.env.PORT || 3000;
```

In the file /models/index.js change the `const sequelize` code to look like the following:
```
const { DB_NAME, DB_USER, DB_PASSWORD, DB_HOST, DB_PORT, CLEARDB_DATABASE_URL } = process.env;

const setupDatabase = () => {
  const sequelize = CLEARDB_DATABASE_URL ?
  new Sequelize(CLEARDB_DATABASE_URL) :
  new Sequelize(DB_NAME, DB_USER, DB_PASSWORD, {
    host: DB_HOST,
    port: DB_PORT,
    dialect: 'mysql',
    logging: false,
  });
```  
*Be sure to declare CLEARDB_DATABASE_URL withing the object destructuring of process.env*

### Push and deploy
Once everything has been setup, the app can be deployed.

**Push any changes to git hub first**
```
git push origin
```
then,

```
git push heroku master
```

Check that everything is running:
```
heroku logs -t
```

Assuming there are no errors, the app is **live!**


*[return to table of contents](#table-of-contents)*

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

- [Docker documentation](https://docs.docker.com/)
- [Postman documentation](https://learning.postman.com/docs/postman/launching-postman/introduction/)
- [MySQL Workbench](https://dev.mysql.com/downloads/workbench/)
- [Heroku CLI & Dev Center](https://devcenter.heroku.com/articles/heroku-cli#download-and-install)

*[return to table of contents](#table-of-contents)*