# BootstrapTesting

## GRUNT

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


