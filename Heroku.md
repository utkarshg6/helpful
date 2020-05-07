# Heroku

## App Deployment through Terminal

### Add SSH Keys

```zsh
heroku keys:add
```

Press y and hit Enter.

### Create Heroku Project

```zsh
heroku create {unique-project-name}
```

### Add Script

Inside the file `package.json`

1. Remove key value pair for `"test"`.
2. Add a key value pair `"start": "{command-to-run-server}"`. Eg. `"start": "node src/app.js"`.
3. Dev's can now run `npm run start` to run on localhost.

### Add support for Heroku

Change the listen function for `PORT`. Also, make changes to statements which contains `localhost`.

### Push Code to Heroku

```zsh
git push heroku {branch-name}
```
