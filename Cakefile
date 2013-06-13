# Imports
# =================================================
fs 			= require 'fs'
{print} = require 'sys'
{spawn} = require 'child_process'
{exec} 	= require 'child_process'

# Basename for app. Source and output folders.
# =================================================
filename = 'app'
lib      = 'lib'
dist     = 'dist'
src      = 'src'
test     = 'test'
spec     = "#{test}/spec"

# Functions
# =================================================

###
Simple build of all .coffee files from source to output folders.
Use an args object with these settings as attributes:
@param	callback	Function 	callback function invoked concludingly
@param	watch			boolean		watch changes in src dir
@param	joinFiles	boolean		join/concatenate files in the order they were passed
@param	srcMap		boolean		use source maps
###
build = (args) ->

	console?.log '================ building ================='

	msg   = ''      # Log message
	flags = []      # Store flags command for log
	opts = ['-c']
	opts.push('-w') and flags.push('-w') and msg += "- watching directory #{lib}...\n" if args?.watch?
	opts.push('-m') and flags.push('-m') and msg += "- building source maps\n" if args?.srcMap?
	opts.push('-j', "#{lib}/#{filename}.js") and flags.push('-j') and msg += "- joining files" if args?.joinFiles?
	opts.push '-o', lib
	opts.push src

	f = flags.join()
	console?.log      "- building with flag(s) #{f}" unless flags.length is 0

	coffee = spawn    'coffee', opts
	coffee.stderr.on  'data', (data) -> process.stderr.write data.toString()
	coffee.stdout.on  'data', (data) -> process.stdout.write data.toString()

	coffee.on         'exit', (code) -> args?.callback?() if code is 0

	# Move test files to the main test/spec/ folder
	fs.exists "#{lib}/#{spec}", (exists) ->
		fs.exists "#{__dirname}/#{spec}", (mustDelete) ->
			if mustDelete 
				exec "rm -r #{__dirname}/#{spec} && mv #{__dirname}/#{lib}/#{spec} #{__dirname}/#{spec}"
			else
				exec "mv #{__dirname}/#{lib}/#{spec} #{__dirname}/#{spec}"

	# Output final message
	console?.log msg

###
Minify #{filename} if it exists in the lib dir. Outputs it as #{filename}.min.js.
Use an args object with these settings as attributes:
@param	callback	Function 	callback function invoked concludingly
@param	output		string		overrides output path
###
minify = (args) ->

	console?.log '================ minifying ================='
	console?.log '- please wait...'

	# Set default output file if none provided
	if args?.output? 
		output = "#{args.output}/#{filename}.min.js"
	else
		output = "#{lib}/#{filename}.min.js" 
	
	# Launch closure compiler
	exec "java -jar 'bin/compiler.jar' --js #{lib}/#{filename}.js --js_output_file #{output}", (err, stdout, stderr) ->
		throw err if err
		args?.callback?()
		console?.log '- done!'

# Tasks 
# =================================================
task 'build',      'build coffee to js, from src dir to lib dir', -> build()

task 'build:min',  'build, join, minify', -> build(joinFiles:true, srcMap:true, callback: minify)

task 'build:dist', 'build, join, minify and place in dist dir - override to fit your needs', ->
	console.log "Messages may appear in the wrong order... don't mind this..."
	build(joinFiles:true, srcMap:true, callback: do() -> minify(output: dist))