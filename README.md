CoffeeScript Work Environment
=============================
My basic boilerplate for frontend Coffeescript work.
Includes testing with Jasmine, a basic Cakefile, Google Closure Compiler.

## Directories structure

#### src
Add .coffee files here.

#### test/spec
Add your Jasmine specs here.

#### lib
A cake task will output a copy of the files here.

#### dist
A cake task will create here a single, assembled, minified version of the files.

#### bin
Includes the Google Closure compiler to uglify and compress. Used by cake tasks.

## Cake Commands
_build_      - build coffee to js, from src dir to lib dir

_build:min_  - build, join, minify

_build:dist_ - build, join, minify and place in dist dir - override to fit your needs