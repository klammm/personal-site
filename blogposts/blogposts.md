# 5 things I wished someone told me before building a Swagger-Express and PostgreSQL application

 4/18/2017 -- Kevin Lam

Hello there, my friend.

If you're looking for a Hello World tutorial, this is the wrong place. However if you're looking to laugh at, sympathize, or empathize with the struggles of an average web developer, then you've come to the right place.

Before we go any further, here is a list of tools and their versions I used:

- [PostgreSQL database 9.6.2](https://www.postgresql.org/)
    - [Knex.js 0.12.8](http://knexjs.org/)
- [Express.js 4.12.3](https://expressjs.com/)
- [Swagger API Framework 1.5.13](http://swagger.io/)

In this read, I will not go in depth as to why I chose a specific tool over another based on my lack of experience with other tools in the community. I started with these tools based on recommendations I was given and so here was my first attempt.

_Note_: I am developing on a Mac Operating System. I do not account for Windows or Linux development.

### #0: Set up

Let's check if you have Node.js and npm installed. If not, you can download it [here](https://nodejs.org/en/). I am currently using Node 7.6.0, but it's ok to download and install the latest version.

`npm -v` & `node -v` to check whether you already have node or npm.

Now to install our dependencies:

- `npm install --save express pg`

- `npm install -g swagger knex`

- `swagger project create INSERT_NAME`

### #1: Plan, map out, and finalize your data architecture.

- Draw your ERD([Entity Relatinship Diagram](https://www.lucidchart.com/pages/er-diagrams))

Please. Please. Please for the love of all that is good in this world. Please have an idea of what you're data architecture is going to look like. Throughout building this project, I was ambiguous with my data models and a bit messy with data exchange from one function to another. As a result, I was forced to backtrack and fix my migration schema(more on that below) time and time again. From being forced to fix my migration schema, I was then led to configure my `swagger.yaml` file accordingly. Whether your response returns a [primitive or reference data type](http://stackoverflow.com/questions/13266616/primitive-value-vs-reference-value), it would be incredibly convenient to a developer if they knew __exactly__ what to build first to be [effective vs. being efficient](http://www.insightsquared.com/2013/08/effectiveness-vs-efficiency-whats-the-difference/). So plan your data architecture accordingly and accrue the least amount of technical debt.

### #2: Create the database and start the server

- Declare the [migration schemas](https://en.wikipedia.org/wiki/Schema_migration), create the seeds, and create the server

First off, our migrations use a `knexfile.js` that specifies various configuration settings.

Run the following command in the terminal:

`knex init` which creates our `knexfile.js` with some pre-set configuration

Feel free to delete everything and start fresh or simply delete the `development`, `test`, and `production` key values.

For some simple configuration, please resemble this following format in your `knexfile.js`:

![knexfile](/images/knexfile.png)

Here, we are directing all three keys to be using PostgreSQL as the client with a connection pointing towards its respective databases. You might have already noticed that the `production` key is pointing to a connection of `process.env.DATABASE_URL`. When deploying to Heroku, we will have to set some environment variables for the production environment in which case the connection will point to Heroku's environment variable of `DATABASE_URL`. Do _NOT_ put your own database URL in. Leave it as is. Only change the `DATABASE_NAME` in the development and test keys.


ALRIGHT. Now that we're hopefully on the same page, let's create our database first by running the following command on the terminal:

`createdb DATABASE_NAME_dev` to create our development environment database

`createdb DATABASE_NAME_test` to create our test environment database

`psql -l` to check if our database has been made or not. You should be able to see a list of your databases here.

I've made the mistake of creating my migrations before even creating the database itself. D'oh.

Once we have our `knexfile.js`, run this command on the terminal:

- `knex migrate:make INSERT_SCHEMA_NAME`

Here is an example migration schema:

![example schema](/images/schema.png)

So there's several things going on here. We are returning a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) within the `exports.up` and `exports.down` functions. Within the function itself, we are using the `table` parameter passed in through the [arrow function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) to invoke knex schema building such as the `table.increments()`, `table.string()`, and `table.integer()`.

Once you've configured your knex schema, it's time to make our migrations. If you do not already have PostgreSQL, an easy way to install it is with [Homebrew](https://brew.sh/) by running `brew install postgres`

If you want to understand more about what's happening under the hood or if you're having trouble, here's [PostgreSQL's](https://www.postgresql.org/docs/9.2/static/tutorial-install.html) in depth installation guide(good luck, my friend).

Now that we have our migration schemas. Let's actually make the migration. Run the following command in the terminal:

`knex migrate:latest`

Onto creating the seeds now!

Run the following command in the terminal:

`knex seed:make 00_SEED_NAME` to create a seed.

`knex seed:make 01_NEXT_SEED_NAME` to create the next seed and so on...

Here I give it a file name of `00_SEED_NAME` to run my seeds in an order I specify. Otherwise, we have no control over what order knex runs our seeds in.

Here's an example:

![knex seeds](/images/seeds.png)

If you haven't noticed yet, I have a `.then()` after `knex.insert()` on line 25 - 27. Something I wish I would have known when building out my database is this incredibly clutch piece of code. To be frank, I am still trying to understand what is exactly going on here. To keep it short and I may very well be wrong so take it with a grain of salt, I _believe_ this knex statement appropriately increments my database table primary ID's whenever deleting a row followed up with an insert statement(Please correct me in the comments if I am wrong. I'd like to know what is happening here also and for the sake of everyone else too).

Now that we've made our seeds, it's time to run them.

Run the following command in the terminal:

`knex seed:run`

### #3: Configure the Swagger.yaml


### #4: Write the function logic using Knex


### #5: Run the server and check to see if it is working
