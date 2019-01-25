# Multiple Heroku Apps from the same GutHub repo

This is a simple example for demostrating how we can manage multiple Heroku apps from the same GitHub repo.

By default Heroku expects a separate repo for each app that gets deployed to Heroku. To work around this limitation we can follow the setup shown here.

## Create Heroku Apps

In this example we have apps `app-one` and `app-two` which are both part of the same repo. We want to deploy them to Heroku as separate apps.

Assuming the `git` repo is already initialized in the root of the project, we should open each project separately, create the Heroku app with a custom `remote` name and add a script for easier deployment to its `package.json`:

```
cd app-one
heroku create
```

This will create an app in Heroku for the `app-one` and will show its name (e.g. `mysterious-tundra-89433`) and its remote url (e.g. `https://git.heroku.com/mysterious-tundra-89433.git`). We should also manually add `heroku-one` remote that points to the git repo in Heroku by running `git remote add heroku-one <app-url>` (replace `<app-url>` with the actual repo url). 

The last step is to add a `publish` command in the `package.json` which looks like

```
"scripts": {
  "publish": "currDir=${PWD##*/} && cd ../ && git subtree push --prefix ${currDir} heroku-one master || true"
}
```

Now we can run `npm run publish` from the `app-one` directory and it will get published to Heroku.

## Repeat

For the other app - `app-two` we should do the same.

```
cd ../
cd app-two
heroku create
```

This will create another app in Heroku and will return its git repo url. We should add a `heroku-two` git remote for it by running `git remote add heroku-two <app-url>` (replace `<app-url>` with the actual repo url). 

And then again, add a publish command in the `package.json` for the `app-two` project.

```
"scripts": {
  "publish": "currDir=${PWD##*/} && cd ../ && git subtree push --prefix ${currDir} heroku-two master || true"
}
```