GULP assetsToBase64
==================

This helper will inject images and fonts into your css files.

Warning ! This technique is really efficient with small files (<14 Kb) cause it avoids DNS requests and makes the page loading faster. But for larger files it will be a mistake to use it !

Install it
----------

```
npm install --save-dev gulp-assetstobase64
```

Use it
------

Here is my sass config. As you can see, I use the 'maxSize' option to specitfy that files larger than 14kb are not injected into the css file.

```
var assetsToBase64 = require('gulp-assetstobase64');

$ = require('gulp-load-plugins')({lazy: true});
...

// SASS
gulp.task('sass', function() {
    return gulp
        .src(config.sass.src)
        .pipe($.sass().on('error', $.sass.logError))
        .pipe(assetsToBase64({
           baseDir: config.src,
           maxSize: 14 * 1024,
           debug: true
        }))
        .pipe($.autoprefixer('last 2 version', 'safari 5', 'ie 8', 'ie 9', 'opera 12.1', 'ios 6', 'android 4'))
        .pipe($.concat(config.sass.output))
        .pipe(gulp.dest(config.sass.dest))
});
```

Options
-------
 - ``baseDir`` : the root path for assets
 - ``useRelativePath`` : overrides baseDir; root path is relative to the input file's directory
 - ``maxSize`` : define the limit size of injected assets
 - ``debug`` : show debug messages

Force asset injection
---------------------

In your css file, just add ``,base64`` to the image url : it will force the asset to be injected in base64 in css file, event if the ``maxSize`` is reached.

```
div.logo {
	background: transparent url(/img/logo.png,base64) no-repeat center center;
}
```
