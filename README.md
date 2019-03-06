## Installation

1. You need `Node.js` (at least 10.x version) installed on your machine, if you don't have it, you should install it - download [link](https://nodejs.org/en/download/)
2. [Clone the project from github](https://github.com/express-argon/) or [download the archive](https://github.com/node-argon-archive/)
3. `cd` to your downloaded Argon app
4. Install necessary dependencies:
    - **Via node `npm` package manager** - Run `npm install` on the project root
    - **Via `yarn` package manager** - Run `yarn install` on the project root

## Configuration for PostgreSQL database and Redis data structure store

##### Via Docker

1. Install **Docker** on your machine
2. Run `docker-compose up -d` in a terminal on the project root. This will start 3 containers:
    - database(PostgreSQL) container;
    - redis container - required for session management;
    - haproxy container - required only for a staging/production setup;

##### Via another chosen solution.

1. Install your **PostgreSQL** database
2. Install your **Redis** server
3. Change connection configuration, from your root `cd` to `env-files` folder and change the following configurations with your own:

###### **For PostgreSQL connection:**

```javascript
DATABASE_URL=http://127.0.0.1:5432
DATABASE_NAME=creativeTim
DATABASE_USER=creativeTim
DATABASE_PASSWORD=creativeTim
```

######  **For Redis connection:**

```javascript
REDIS_HOST=localhost
REDIS_PORT=6379
```

## Migrations and seeds

1. For database tables structure, in the project root run: `npm knex migrate:latest` or `yarn knex migrate:latest` if you are using `yarn` as the default package manager
2. To create a default user, run: `npm knex seed:run` or `yarn knex seed:run` if you are using `yarn` as the default package manager

## Run the application

1. For starting the application, the following script (defined in `package.json` under `scripts`) must be called:
    - via **npm**: `npm run start` or `npm run dev` for starting the development environment, which has livereload enabled;
    - via **yarn**: `yarn start` or `yarn dev` for starting the development environment, which has livereload enabled;


## Usage

Register a user or login using **admin@argon.com**:**secret** and start testing the preset (make sure to run the migrations and seeds for these credentials to be available).

Besides the dashboard and the auth pages this preset also has an edit profile page.
**NOTE**: _Keep in mind that all available features can be viewed once you login using the credentials provided above or by registering your own user._

## Features

In order to see the available features `cd` into `features` folder, and you will then find a folder for each of the available features, mostly each folder containing:

- A `routes.js` file that usually contains the `GET` and `POST` requests, for example, the profile router looks like this:

```javascript
const { wrap } = require('async-middleware');

const requestBodyValidation = require('./commands/verify-request-body');
const updateUserInfo = require('./commands/update-user-info');
const { loadPage } = require('./commands/profile');

module.exports = (router, middlewares = []) => {
  router.get('/profile', middlewares.map(middleware => wrap(middleware)), wrap(loadPage));
  router.post('/update-profile-info', wrap(requestBodyValidation), wrap(updateUserInfo));

  return router;
};
```

- A `repository.js` file that contains feature database queries
- A `commands` folder where you can find all feature functionality functions, for example the login template rendering which looks like this:

```javascript
function loadPage(req, res) {
  debug('login:servePage', req, res);
  res.render('pages/login');
}
```
- A `constants.js` file, to store all your static variables, for eg:

```
const USERNAME_PASSWORD_COMBINATION_ERROR = 'These credentials do not match our records.';
const INTERNAL_SERVER_ERROR = 'Something went wrong! Please try again.';
```

All feature routes are mounted in `routes/index.js` from the project root.

## For the Front-end side:

##### Templates

- You can find all the templates in `views` folder where you will find:
1. The `layout.ejs` file, the main template layout.
2. A `pages` folder with all the page templates
3. A `partials` folder with the common components (header, footer, sidebar)

## Folder Structure

```
├── CHANGELOG.md
├── ISSUES_TEMPLATE.md
├── LICENSE.md
├── README.md
├── app.js
├── bin
│   └── www
├── config
│   └── index.js
├── db
│   ├── index.js
│   ├── knexfile.js
│   ├── migrations
│   │   └── 20190213122702_create-users-table.js
│   └── seeds
│       └── create-default-user.js
├── docker-compose.yml
├── docs
│   └── documentation.html
├── ecosystem.config.js
├── env-files
│   ├── development.env
│   └── production.env
├── features
│   ├── login
│   │   ├── commands
│   │   │   ├── load-page.js
│   │   │   ├── login.js
│   │   │   ├── redirect-to-dashboard.js
│   │   │   └── verify-request-body.js
│   │   ├── constants.js
│   │   ├── init-auth-middleware.js
│   │   ├── repository.js
│   │   └── routes.js
│   ├── logout
│   │   ├── commands
│   │   │   └── logout.js
│   │   └── routes.js
│   ├── profile
│   │   ├── commands
│   │   │   ├── load-page.js
│   │   │   ├── update-user-info.js
│   │   │   └── verify-request-body.js
│   │   ├── constants.js
│   │   ├── repository.js
│   │   └── routes.js
│   ├── register
│   │   ├── commands
│   │   │   ├── create-user.js
│   │   │   ├── load-page.js
│   │   │   └── verify-request-body.js
│   │   ├── constants.js
│   │   ├── repository.js
│   │   └── routes.js
│   └── reset-password
│       ├── commands
│       │   └── load-page.js
│       └── routes.js
├── gulpfile.js
├── haproxy.cfg
├── logger.js
├── package.json
├── public
│   ├── css
│   │   ├── argon.css
│   │   └── argon.min.css
│   ├── fonts
│   │   └── nucleo
│   ├── img
│   │   ├── brand
│   │   ├── icons
│   │   └── theme
│   ├── js
│   │   ├── argon.js
│   │   └── argon.min.js
│   ├── scss
│   │   ├── argon.scss
│   │   ├── bootstrap
│   │   ├── core
│   │   └── custom
│   └── vendor
├── routes
│   └── index.js
├── screens
│   ├── Dashboard.png
│   ├── Login.png
│   ├── Profile.png
│   └── Users.png
├── views
│   ├── layout.ejs
│   ├── pages
│   │   ├── 404.ejs
│   │   ├── dashboard.ejs
│   │   ├── icons.ejs
│   │   ├── login.ejs
│   │   ├── maps.ejs
│   │   ├── profile.ejs
│   │   ├── register.ejs
│   │   ├── reset-password.ejs
│   │   └── tables.ejs
│   └── partials
│       ├── auth
│       │   ├── footer.ejs
│       │   ├── header.ejs
│       │   └── navbar.ejs
│       ├── dropdown.ejs
│       ├── footer.ejs
│       ├── header.ejs
│       ├── navbar.ejs
│       └── sidebar.ejs
└
```
## Change log

Please see the [changelog](changelog.md) for more information on what has changed recently.

## Credits

- [Creative Tim](https://creative-tim.com/)
- [Under Development Office](https://udevoffice.com/)

## License

[MIT License](https://github.com/laravel-frontend-presets/argon/blob/master/license.md).

## Screenshots

![Argon Login](/screens/Login.png)

![Argon Dashboard](/screens/Dashboard.png)

![Argon Users](/screens/Users.png)

![Argon Profile](/screens/Profile.png)
