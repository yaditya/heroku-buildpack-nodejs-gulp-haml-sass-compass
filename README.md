# Heroku Buildpack for Node.js + Gulp + HAML + SASS + Compass

Differences to the original buildpack:

- `npm` will also install dev dependencies in order to run `gulp`
- it will also install HAML, SASS and Compass
- it will detect a `gulpfile.js` and execute the `heroku` task in it

## Create a heroku task

```
gulp.task('heroku', 'build')
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

## License

http://opensource.org/licenses/MIT
