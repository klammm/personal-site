# My first attempt at building a Node.js application

 4/18/2017 -- Kevin Lam

Hello there, my friend.

If you're looking for a Hello World tutorial, this is the wrong place. However if you're looking to laugh at, sympathize, or empathize with the struggles of an average web developer, then you've come to the right place.

Before we go any further, here is a list of tools I used:

- [PostgreSQL database](https://www.postgresql.org/)
    - [Knex.js](http://knexjs.org/)
- [Express.js](https://expressjs.com/)
- [Swagger API Framework](http://swagger.io/)

In this read, I will not go in depth as to why I chose a specific tool over another based on my lack of experience with other tools in the community. I started with these tools based on recommendations I was given and so here was my first attempt.

### #0: Set up~~

Let's check if you have Node.js and npm installed. If not, you can download it [here](https://nodejs.org/en/). I am currently using Node 7.6.0, but it's ok to download and install the latest version.

`npm -v` & `node -v`

Now to install our dependencies:

- `npm install express`

- `npm install -g swagger knex`

- `swagger project create INSERT_NAME`

### #1: Plan and map out your data architecture.

- Draw your ERD([Entity Relatinship Diagram](https://www.lucidchart.com/pages/er-diagrams))

Please. Please. Please for the love of (insert name). Please have an idea of what you're data architecture is going to look like. Throughout building this project, I was ambiguous with my data models and a bit messy with data exchange from one function to another. As a result, I was forced to backtrack and fix my migration schema(more on that below) time and time again. From being forced to fix my migration schema, I was then led to configure my `swagger.yaml` file accordingly. Whether your response returns a [primitive or reference data type](http://stackoverflow.com/questions/13266616/primitive-value-vs-reference-value), it would be incredibly convenient to a developer if they knew __exactly__ what to build first to be [effective vs. being efficient](http://www.insightsquared.com/2013/08/effectiveness-vs-efficiency-whats-the-difference/). So plan your data architecture accordingly and accrue the least amount of technical debt.

### #2: Create the database and server

- Declare the migration schemas, create the seeds, and create the server

First off, our migrations use a `knexfile.js` that specifies various configuration settings.

Once we have our `knexfile.js`, run this command on the terminal:

- `knex migrate:make INSERT_SCHEMA_NAME`

Here is an example migration schema:

![example schema](/images/schema.png)

So there's several things going on here. We are returning a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) within the `exports.up` and `exports.down` functions. Within the function itself, we are using the `table` parameter passed in through the [arrow function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) to invoke knex schema building such as the `increments()`, `string()`, and `integer()`.

### #3: Configure the Swagger.yaml


### #4: Write the function logic using Knex


### #5: Run the server and check to see if it is working
