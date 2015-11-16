<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [klarify-ds-webcrm](#klarify-ds-webcrm)
    - [Setting up the project](#setting-up-the-project)
    - [Running the project](#running-the-project)
    - [Usage](#usage)
      - [Available routes](#available-routes)
    - [Testing](#testing)
    - [Contributing](#contributing)
      - [Commiting and pull requests](#commiting-and-pull-requests)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


# klarify-ds-webcrm

[WebCRM integration page](https://b2bsys.net/tcm/crm_PROD_5/integration.asp)

### Setting up the project

The project runs on [Node.js](https://nodejs.org/en/), and is written in JavaScript. The database type is Microsoft SQL.

To install Node.js, simply install it by clicking the big green button on [their website](https://nodejs.org/en/).

Clone the repository using Git (if you don't have Git on your computer, [here's the download link](https://git-scm.com/download))

```
git clone git@github.com:Kugghuset/klarify-ds-webcrm.git
```

If you don't have [Gulp](http://gulpjs.com/), [Express](http://expressjs.com/), [Mocha](http://mochajs.org/) or [DocToc](https://github.com/thlorenz/doctoc), install them via npm, using the `-g` flag (to install them globally).

```
npm install -g gulp express mocha doctoc
```

Install the packages.

```                                                                                   
npm install
```

To ensure changes to `userConfig.js` doesn't sneak into the repository, run: 

```
git update-index --assume-unchanged userConfig.js
```

Lastly update `userConfig.js` file to match your setup.
Note: changes to this file won't be commited as it is in the `.gitignore` file. To ensure you don't have to constantly get new keys, keep a copy of this file outside the repository, so won't lose any local keys when pulling.
Note: sometimes the file might be reset to the state it is in the repository, for instance resetting to a previous commit or when checkout out another branch based on a remote branch, the local `userConfig.js` file will be reset. This will cause issues when running the `gulp` command, as it won't be able to set the server up.

Here's how I've got mine set up, you'll need to add any API keys for this particular data source yourself:

```javascript
'use strict'

module.exports = {
  dbUser: 'sa', // database user
  dbPass: 'pass', // database password
  dbServer: 'EASTGROVESOFTWA\\LOCALSQL', // database server
  dbName: 'master' // name of database
};
```

### Running the project

You'll need a Microsoft SQL Server running somewhere. I've got mine setup via [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).

When the server is up and running, assuming you're done setting up, simply run:

```
gulp
```

### Usage

The project itself is only a service, and will on startup only spin up a service open for http requests. I use [Postman](https://www.getpostman.com/) for mocking requests to the service, and requests are made to `<url>/<endpoint>/<action>`. For instance, if I'm running the server on `http://localhost:3000` and wanted to fetch newly modified, I'd make a ´GET´ request to `http://localhost:3000/customer/fetchNewlyModified`. This can of course be done straight in any web browser, but Postman formats the output and makes it more readable.

The resources are in singular, whereby you simply replace `<resource>` with for instance `customer` and `<baseurl>` with in my case `http://localhost`. So, if I want all customers, including historical data, I'd make a GET request to `http://localhost:3000/customer/` and I can expect a JSON response containing an array of all customers in the db.

#### Available routes

```
GET: <baseurl>:3000/<resource>/
  -> Gets all <resource>s from the db, includes historical data.

GET: <baseurl>:3000/<resource>/getAllActive
  -> Gets all active <resource>s from the db

GET: <baseurl>:3000/<resource>/fetchNewlyModified
  -> Triggers the fetchNewlyModified flow method for the <resource>

GET: <baseurl>:3000/<resource>/cleanAndFetch
  -> Triggers the cleanAndFetch flow method for the <resource>
```

### Testing

Tests are written in [Mocha](http://mochajs.org/), and uses [unit.js](http://unitjs.com/) (which allows for testing in a couple of different framworks).

The testing suite can be run either via Mocha itself, npm or gulp. I advice to use `npm test` as it's colorized and runs from everywhere.

**npm:** from anywhere in the project, run:
```
npm test
```

### Contributing

#### Commiting and pull requests

Committing straight onto `master` is a no-no. Commits done onto feature specific branches, which then are audited and if it passes can be merge into `master` by an admin. Commits are made on a regular basis when *_the current step is finished_* and Pull Requests are made *when the current feature is finished*.

Commmit messages should be concise and written in futurum, which means a commit which has added a GET request to the Customer endpoint on the Fortnox API would be something like: `Add GET request to Customer endpoint`.
