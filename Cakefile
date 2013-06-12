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
@param	joinFiles	boolean		join/concatenate files in the order they were passed
@param	srcMap		boolean		use source maps
###
build = (args) ->

	console?.log '================ building ================='

	msg  = ''
	flags = []
	opts = ['-c']
	opts.push('-w') and flags.push('-w') and msg += "- watching directory #{lib}...\n" if args?.watch?
	opts.push('-m') and flags.push('-m') and msg += "- building source maps\n" if args?.srcMap?
	opts.push('-j', "#{lib}/#{filename}.js") and flags.push('-j') and msg += "- joining files" if args?.joinFiles?
	opts.push '-o', lib
	opts.push src

	f = flags.join()
	console?.log "- building with flag(s) #{f}" unless flags.length is 0

	coffee = spawn 		'coffee', opts
	coffee.stderr.on 	'data', (data) -> process.stderr.write data.toString()
	coffee.stdout.on 	'data', (data) -> process.stdout.write data.toString()

	coffee.on 				'exit', (code) -> callback?() if code is 0

	console?.log 			msg

###
Minify #{filename} if it exists in the lib dir. Outputs it as #{filename}.min.js.
###


task 'build', 'build coffee to js, from src dir to lib dir', -> build(joinFiles:true, srcMap:true, watch:true)