We use the following example as a reference. There will be some differences with other environments.

#### Nodejitsu

##### Pre-requisites
1. Install the [jitsu](https://npmjs.org/package/jitsu) module
2. Install Geddy. If you're new, you can start with the [tutorial](http://geddyjs.org/tutorial)
3. Create a Nodejitsu account(Not required: we'll go over creating one from the CLI)
4. Have an app ready to be deployed

##### Notes
* Nodejitsu reads the deployed .gitignore file, so if you have the config/secrets.json file in there(you should), then you'll encounter errors about needing to do `geddy secret` if you need the secrets for sessions, etc. To circumvent this, create a .npmignore file and include all the contents from the .gitignore _except_ the config/secrets.json line. Nodejitsu ignores the .gitignore file only if a .npmignore file is included as well.

If you haven't already you'll need to sign up and log in to Nodejitsu which you can do from the jitsu executable.
```
jitsu signup
jitsu login
```

Now once you've created an account on Nodejitsu we need to prepare the application you have for deployment. First we'll edit(or create) a `package.json` file in the app's root directory
```
{
  "name": "node-example",
  "version": "0.0.1",
  "dependencies": {
    "geddy": "0.6.x"
  },
  "subdomain": "geddy-example",
  "scripts": {
    "start": "app.js"
  },
  "engines": {
    "node": "0.8.x"
  }
}
```
Here we have a subdomain key/value this tells Nodejitsu what subdomain to host the application on(e,g,. geddy-example.jit.su). We also have a start script pointing to `app.js` in the root directory, we'll go over what to put here in a second. Of course you should edit this to include anything else you want, like other dependences or an author.

Now we need to create a `app.js` file so that Nodejitsu can use it to boot the Geddy server, here's what it should look like
```
var geddy = require('geddy');

geddy.start({
  environment: 'production'
});
```
In the object we're giving to `geddy.start` you can use any other arguments you'd for the configuration files, these will override the ones loaded for the environment. For more information about this file you can go [here](https://github.com/mde/geddy/wiki/Using-Geddy-without-the-CLI)

Now that our application is set up for deployment, we need to deploy it which is just a single command
```
jitsu deploy
```
Now you can go to http://geddy-example.jit.su and see your application!

#### Heroku

##### Pre-requisites
1. Install [heroku toolbelt](https://devcenter.heroku.com/articles/quickstart#step-2-install-the-heroku-toolbelt)
2. Install Geddy. If you're new, you can start with the [tutorial](http://geddyjs.org/tutorial)
3. Be familiar with GIT, the basic geddy commands, and heroku's deployment models
4. Have an app ready to be deployed.

Add a `package.json` file to your app's root directory

```javascript
{
  "name": "node-example",
  "version": "0.0.1",
  "dependencies": {
    "geddy": "0.6.x"
  },
  "engines": {
    "node": "0.8.x",
    "npm": "1.1.x"
  }
}
```

Edit the `config/production.js` file to use the port given by heroku
```javascript
var config = {
  port: process.env.PORT
  // Other properties removed for brevity
};

```

Add a `Procfile` text file to your app's root directory

```
//web: node node_modules/geddy/bin/cli.js
var geddy = require('geddy');

geddy.startCluster({
  environment: 'production'
});
```

Now it's time to create a heroku app.

```
$ heroku create --stack cedar
```

Add everything to git and push to heroku

```
$ git push heroku master
```

For more information about deploying and supporting Node Apps on Heroku see the [Getting Started with Node.js on Heroku](https://devcenter.heroku.com/articles/nodejs) article.
