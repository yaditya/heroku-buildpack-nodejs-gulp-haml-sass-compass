# Heroku Buildpack for Node.js + Gulp + HAML + SASS + Compass

Differences to the original buildpack:

- `npm` will also install dev dependencies in order to run `gulp`
- it will also install HAML, SASS and Compass
- it will detect a `gulpfile.js` and execute the `heroku` task in it

## Create a heroku task

Add a heroku task to your `gulpfile.js`:

```
gulp.task('heroku', 'build');
```

This task will be called by the compile script. You should build everything and delegate the serving to a server used by the Procfile.

## Heroku

```
heroku config:add BUILDPACK_URL=https://github.com/9elements/heroku-buildpack-nodejs-gulp-haml-sass-compass.git
```

## Dokku

Create a `.env` file with this content to use this custom buildpack

```
export BUILDPACK_URL=https://github.com/9elements/heroku-buildpack-nodejs-gulp-haml-sass-compass.git
```

## Serving the Website

This `index.js` file creates a static express server with password protection. It assumes that your `heroku` task created a `build` directory.

```
var express = require('express');
var basicAuth = require('basic-auth');
var app = express();

var auth = function (req, res, next) {
  function unauthorized(res) {
    res.set('WWW-Authenticate', 'Basic realm=Authorization Required');
    return res.send(401);
  };

  var user = basicAuth(req);

  if (!user || !user.name || !user.pass) {
    return unauthorized(res);
  };

  if (user.name === 'fancy' && user.pass === 'password') {
    return next();
  } else {
    return unauthorized(res);
  };
};


app.set('port', (process.env.PORT || 5000));
app.use(auth);
app.use(express.static(__dirname + '/build'));

app.listen(app.get('port'), function() {
  console.log("Static express server is now running at localhost:" + app.get('port'))
})
```

Use this `Procfile` to run it

```
web: node index.js
```

## License

http://opensource.org/licenses/MIT


