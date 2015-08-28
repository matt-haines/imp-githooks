# imp-githooks

The imp-githooks project is an incredibly simple webserver designed to be used with [GitHub's Webhooks](https://developer.github.com/webhooks/) to deploy code to your Electric Imp devices whenever you push changes to the master branch of a tracked repository.

## Limitations

At present, the imp-githooks project *only* works for public repositories, and only tracks the master branch of the project.

This project is *not* complete, and is intended to act as a starting point for more complex Electric Imp + GitHub CI tools.

## Installation

This installation guide will demonstrate how to setup and configure this project on a free [Heroku](heroku.com) instance. Before getting started you will need to ensure you have done the following:

- Installed [Node.js and npm](https://docs.npmjs.com/getting-started/installing-node)
- Created a [Heroku account](https://signup.heroku.com/login)
- Installed the [Heroku Toolbelt](https://toolbelt.heroku.com/)
- Installed the [imp-cli](https://github.com/matt-haines/imp-cli#installation)
- Copied your [Build API key](https://electricimp.com/docs/buildapi/keys/)
  - **Note**: We recommend you create a separate Build API key for use with imp-githooks.

### Clone the imp-githooks repository

```bash
$ git clone git@github.com:matt-haines/imp-githooks.git
$ cd imp-webhooks
```

### Create and configure the Heroku app

```bash
$ heroku create
$ git push heroku master
$ heroku ps:scale web=1
$ heroku config:set BUILD_API_KEY=BuildApiKey
$ heroku config:set GIT_PUSH_SECRET=YourSecret
```

**NOTE:** We'll use the GIT_PUSH_SECRET when setting up our GitHub webhook. It should be something unique and difficult to guess.

When you run the `heroku create` command, it should return with the URL of your app (in the example below it is https://damp-inlet-8875.herokuapp.com/):

```bash
$ heroku create
Creating damp-inlet-8875... done, stack is cedar-14
https://damp-inlet-8875.herokuapp.com/ | https://git.heroku.com/damp-inlet-8875.git
Git remote heroku added
```

### Setup a Webhook in GitHub

- Browse your GitHub repository and click on "Settings" -> "Webhooks and services"
- Click "Add Webhook"
- Set the "Payload URL" to your apps URL with "/webhook" appended to the end of it
  - i.e. https://damp-inlet-8875.herokuapp.com/webhook
- Set the "Secret" to the GIT_PUSH_SECRET you set in your Heroku app.
- Click "Add Webhook"

## Creating imp-githook Compatible Repositories

The imp-githook server is intended to be used with the config files generated by the imp-cli. To create a new project run the the `imp init` command with the `-g` option (which prevents your Build API Key from being injected into the .impconfig file).

```bash
$ imp init -g
```

Whenever a commit is made to the master branch of a repository with the proper webhooks and .impconfig setup, the imp-githook server will push the new code to each device associated with the model, and restart the devices.

For a sample project, view [imp-test](https://github.com/matt-haines/imp-test).

# License

The imp-githooks project is licensed under the [MIT License](./LICENSE).
