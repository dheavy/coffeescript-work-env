# Imports
# =================================================
fs 			= require 'fs'
{print} = require 'sys'
{spawn} = require 'child_process'
{exec} 	= require 'child_process'

# Basename for app. Source and output folders.
# =================================================
filename 	= 'app'
lib				= 'lib'
dist			= 'dist'
src 			= 'src'

# Functions
# =================================================

###
Simple build of all .coffee files from source to output folders.
Use a config object with these settings as attributes:
@param	callback	Function 	callback function invoked concludingly
@param	watch			boolean		watch changes in src dir
@param	joinFile	boolean		join/concatenate files in the order they were passed
@param	srcMap		boolean		use source maps
###
build = (args) ->

	console?.log '================ building ================='

	opts = ['-c', '-o']
	opts.push '-w' if args?.watch?
	opts.push '-m' if args?.srcMap?
	opts.push '-j' if args?.joinFile?
	opts = opts.concat [lib, src]

	coffee = spawn 		'coffee', opts
	coffee.stderr.on 	'data', (data) -> process.stderr.write data.toString()
	coffee.stdout.on 	'data', (data) -> process.stdout.write data.toString()

	coffee.on 				'exit', (code) -> callback?() if code is 0


task 'build', 'build coffee to js, from src dir to lib dir', -> build()