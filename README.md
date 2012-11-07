Django Template
======================
- clean project structure
    - separate ``src`` directory for Django code
    - ``_site`` directory for web server document root
        - copied static files and user uploaded media files
        - works well with nginx's ``try_files`` directive
        - ``maintenance`` directory for toggling maintenance mode's
- tested server configurations for nginx, uWSGI, and Supervisor
- tiered settings for easier cross-environment support
    - ``global_settings.py`` for environment-independent settings
    - ``local_settings.py`` for environment-specific settings (not versioned)
    - ``settings.py`` for bringing them together and post-setup
- ``local_settings.py.sample`` template
- a clean static directory for large Web app development
- wicked hot Makefile for watching static files pre-processors:
    - ``make watch``
    - CoffeeScript (requires Node and CoffeeScript)
    - SCSS (requires Ruby and Sass)
    - compiles scss => css
    - compiles coffee => javascript/src
- integration with [r.js](https://github.com/jrburke/r.js/)
    - ``make optimize``
    - includes ``app.build.js`` file for single-file JavaScript optimization
    - compiles javascript/src => javascript/min
- context processor for including JavaScript and CSS static urls
    - ``{{ CSS_URL }}``
    - ``{{ JAVASCRIPT_URL }}``
- script in ``./bin/secret_key.py`` to output a ``SECRET_KEY`` for your
    ``local_settings.py``
- simple, but useful fabfile.py for common commands
- ...HTML5 boilerplate hotness

Dependencies
------------
- Python 2.6 or 2.7

For stylesheet development, the [Sass](http://sass-lang.com) CSS preprocessor is used, specifically using the SCSS syntax.

For script development, [CoffeeScript](http://coffeescript.org) is used to ensure consistently generated JavaScript.

All scripts are written as modules using the [AMD API](http://requirejs.org/docs/whyamd.html). For distribution, all scripts are _uglified_ and some are concatenated using the [RequireJS optimizer](http://requirejs.org/docs/optimization.html).

If doing SCSS development, the following are required to compile the SCSS files:

- [Ruby](http://www.ruby-lang.org)
- [RubyGems](http://rubygems.org)
- Sass - `gem install sass`

If doing CoffeeScript development, the following dependencies are required to compile CoffeeScript files and run the optimizer:

- [Node](http://nodejs.org)
- [npm](https://npmjs.org)
- CoffeeScript - `npm install -g coffee-script`
- UglifyJS - `npm install -g uglify-js`

**Note, all compiled CSS and JavaScript files must be added to the repo's .gitignore file to prevent having to deal with merging these files over time.**

Fabfile Commands
----------------
- ``mm_on`` - turns on maintenance mode
- ``mm_off`` - turns off maintenance mode

Makefile Commands
-----------------
- ``build`` - builds and initializes all submodules, compiles SCSS and
    CoffeeScript and optimizes JavaScript
- ``watch`` - watches the CoffeeScript and SCSS files in the background
for changes and automatically recompiles the files
- ``unwatch`` - stops watching the CoffeeScript and SCSS files
- ``sass`` - one-time explicit recompilation of SCSS files
- ``coffee`` - one-time explicit recompilation of CoffeeScript files

Local Settings
--------------
``local_settings.py`` is intentionally not versioned (via .gitignore). It should
contain any environment-specific settings and/or sensitive settings such as
passwords, the ``SECRET_KEY`` and other information that should not be in version
control. Defining ``local_settings.py`` is not mandatory but will warn if it does
not exist.

CoffeeScript/JavaScript Development
-----------------------------------
CoffeeScript is lovely. The flow is simple:

- write some CoffeeScript which automatically gets compiled in JavaScript
(assuming you did ``make watch``)
- when ready to test non-``DEBUG`` mode, run ``make optimize``

The ``app.build.js`` file will need to be updated to define which modules
should be compiled to single files. It is recommended to take a tiered
approach to reduce overall file size across pages and increase cache potential
for libraries that won't change for a while, for example jQuery.

SCSS Development
----------------
[Sass](http://sass-lang.com/) is awesome. SCSS is a superset of CSS so you can
use as much or as little SCSS syntax as you want. It is recommended to write
all of your CSS rules as SCSS, since at the very least the Sass minifier can
be taken advantage of.
