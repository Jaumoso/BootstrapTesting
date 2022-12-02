# BootstrapTesting

### Dos programitas para manejo de scripts:

## GRUNT y GULP

```
 npm install grunt-cli --global
 npm install grunt --save-dev
 ```

 ------------
 
  crear Gruntfile.js
 ```
'use strict';

module.exports = function (grunt) {
  // Define the configuration for all the tasks
  grunt.initConfig({

  });
};
```
 ------------
 
 ### GRUNT PLUGINS
 
 ```
 npm install --save-dev grunt-sass@2.1.0
 ```
 
 PRINTS THE TIME STATISTICS
 ```
 npm install --save-dev time-grunt@1.4.0
 ```
 TO LOAD THE PLUGINS WHENEVER THEY ARE NEEDED
 ```
 npm install --save-dev jit-grunt@0.10.0
 ```
 
 ```
 'use strict';

module.exports = function (grunt) {
    // Time how long tasks take. Can help when optimizing build times
    require('time-grunt')(grunt);

    // Automatically load required Grunt tasks
    require('jit-grunt')(grunt);

    // Define the configuration for all the tasks
    grunt.initConfig({
        sass: {
            dist: {
                files: {
                    'css/styles.css': 'css/styles.scss'
                }
            }
        }
    });

    grunt.registerTask('css', ['sass']);

};
```
Ahora se puede utilizar el comando:
```
grunt css
```

---------------------------------------
### Watch and Serve Tasks
```
npm install grunt-contrib-watch@1.0.0 --save-dev
npm install grunt-browser-sync@2.2.0 --save-dev
```

En grunt.js:
```
,
        watch: {
            files: 'css/*.scss',
            tasks: ['sass']
        },
        browserSync: {
            dev: {
                bsFiles: {
                    src : [
                        'css/*.css',
                        '*.html',
                        'js/*.js'
                    ]
                },
                options: {
                    watchTask: true,
                    server: {
                        baseDir: "./"
                    }
                }
            }
        }
```

```
    grunt.registerTask('default', ['browserSync', 'watch']);
    ```

Comando para iniciar el servidor y mantener actualizado el CSS:
```
grunt
``` 
--------------------------------------------
## Más Contenido de Gruntfile.js

```
,

        useminPrepare: {
            foo: {
                dest: 'dist',
                src: ['contactus.html','aboutus.html','index.html']
            },
            options: {
                flow: {
                    steps: {
                        css: ['cssmin'],
                        js:['uglify']
                    },
                    post: {
                        css: [{
                            name: 'cssmin',
                            createConfig: function (context, block) {
                            var generated = context.options.generated;
                                generated.options = {
                                    keepSpecialComments: 0, rebase: false
                                };
                            }       
                        }]
                    }
                }
            }
        },

        // Concat
        concat: {
            options: {
                separator: ';'
            },
  
            // dist configuration is provided by useminPrepare
            dist: {}
        },

        // Uglify
        uglify: {
            // dist configuration is provided by useminPrepare
            dist: {}
        },

        cssmin: {
            dist: {}
        },

        // Filerev
        filerev: {
            options: {
                encoding: 'utf8',
                algorithm: 'md5',
                length: 20
            },
  
            release: {
            // filerev:release hashes(md5) all assets (images, js and css )
            // in dist directory
                files: [{
                    src: [
                        'dist/js/*.js',
                        'dist/css/*.css',
                    ]
                }]
            }
        },
  
        // Usemin
        // Replaces all assets with their revved version in html and css files.
        // options.assetDirs contains the directories for finding the assets
        // according to their relative paths
        usemin: {
            html: ['dist/contactus.html','dist/aboutus.html','dist/index.html'],
            options: {
                assetsDirs: ['dist', 'dist/css','dist/js']
            }
        },

        htmlmin: {                                         // Task
            dist: {                                        // Target
                options: {                                 // Target options
                    collapseWhitespace: true
                },
                files: {                                   // Dictionary of files
                    'dist/index.html': 'dist/index.html',  // 'destination': 'source'
                    'dist/contactus.html': 'dist/contactus.html',
                    'dist/aboutus.html': 'dist/aboutus.html',
                }
            }
        }
        ```
        
        ```
require('jit-grunt')(grunt, {
  useminPrepare: 'grunt-usemin'
});
```

```
    grunt.registerTask('build', [
        'clean',
        'copy',
        'imagemin',
        'useminPrepare',
        'concat',
        'cssmin',
        'uglify',
        'filerev',
        'usemin',
        'htmlmin'
    ]);
    ```
    
    ```
    grunt build
    ```

# GULP
```
     npm install -g gulp-cli@2.0.1
     npm install gulp@3.9.1 --save-dev
     npm install gulp-sass@3.1.0  browser-sync@2.23.6 --save-dev
```

gulpfile.js
```
'use strict';

var gulp = require('gulp'),
    sass = require('gulp-sass'),
    browserSync = require('browser-sync');
```

```
gulp.task('sass', function () {
  return gulp.src('./css/*.scss')
    .pipe(sass().on('error', sass.logError))
    .pipe(gulp.dest('./css'));
});

gulp.task('sass:watch', function () {
  gulp.watch('./css/*.scss', ['sass']);
});

gulp.task('browser-sync', function () {
   var files = [
      './*.html',
      './css/*.css',
      './img/*.{png,jpg,gif}',
      './js/*.js'
   ];

   browserSync.init(files, {
      server: {
         baseDir: "./"
      }
   });

});

// Default task
gulp.task('default', ['browser-sync'], function() {
    gulp.start('sass:watch');
});
```


Comando para lanzar servidor:
```
     gulp
```

--------------------------------------
### Copiar archivos y limpiar

```
npm install del@3.0.0 --save-dev
```

```
var ...
    del = require('del'),
    ...
    ```

```
// Clean
gulp.task('clean', function() {
    return del(['dist']);
});

gulp.task('copyfonts', function() {
   gulp.src('./node_modules/font-awesome/fonts/**/*.{ttf,woff,eof,svg}*')
   .pipe(gulp.dest('./dist/fonts'));
});
```

---------------
### Compressing images 

```
npm install gulp-imagemin@4.1.0 --save-dev
```

```
var ...
    imagemin = require('gulp-imagemin'),
    ...
```
   
   ```
   // Images
gulp.task('imagemin', function() {
  return gulp.src('img/*.{png,jpg,gif}')
    .pipe(imagemin({ optimizationLevel: 3, progressive: true, interlaced: true }))
    .pipe(gulp.dest('dist/img'));
});
```

-----------------------------------
### Prepare dist folders

```
npm install gulp-uglify@3.0.0 gulp-usemin@0.3.29 gulp-rev@8.1.1 gulp-clean-css@3.9.3 gulp-flatmap@1.0.2 gulp-htmlmin@4.0.0 --save-dev
```

```
var ...
    uglify = require('gulp-uglify'),
    usemin = require('gulp-usemin'),
    rev = require('gulp-rev'),
    cleanCss = require('gulp-clean-css'),
    flatmap = require('gulp-flatmap'),
    htmlmin = require('gulp-htmlmin');
    ```

```
gulp.task('usemin', function() {
  return gulp.src('./*.html')
  .pipe(flatmap(function(stream, file){
      return stream
        .pipe(usemin({
            css: [ rev() ],
            html: [ function() { return htmlmin({ collapseWhitespace: true })} ],
            js: [ uglify(), rev() ],
            inlinejs: [ uglify() ],
            inlinecss: [ cleanCss(), 'concat' ]
        }))
    }))
    .pipe(gulp.dest('dist/'));
});

gulp.task('build',['clean'], function() {
    gulp.start('copyfonts','imagemin','usemin');
});
```

Comando para lanzar:
```
     gulp build
     ```

    
