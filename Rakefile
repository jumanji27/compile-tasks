require "rake"


path = File.dirname(__FILE__)
# All files local path here (if it changes)
SCSS_PATH      = "#{path}/assets/css"
COFFEE_FILES   = FileList["#{path}/assets/js/**/*.coffee"]
JS_OUTPUT_DIR  = "#{path}/assets/_js"
JS_OUTPUT_FILE = "assets/js/js.js"

namespace :compile do
  desc "Compile all"
  task :all => [:scss]

  desc "Compile SCSS"
  task :scss do
    require "sass"
    sh "sass --watch #{SCSS_PATH}/style.scss:#{SCSS_PATH}/style.css"
  end


  def compile_files(files, output_dir)
    require "coffee-script"

    counter = "a"
    files.each do |file|
      new_file = File.join(output_dir, counter + ".js")
      File.open(new_file, "w") do |out|
        puts "Compiling #{file} to #{new_file}"
        out.puts CoffeeScript.compile File.read file
      end
      counter = counter.next
    end
  end

  desc "Compile Coffee"
  task :coffee_once do
    if File.directory? JS_OUTPUT_DIR
      compile_files(COFFEE_FILES, JS_OUTPUT_DIR)
    else
      FileUtils.mkdir JS_OUTPUT_DIR
      compile_files(COFFEE_FILES, JS_OUTPUT_DIR)
    end

    sh "cat #{JS_OUTPUT_DIR}/*.js > #{JS_OUTPUT_FILE}"
    sh "rm -rf #{JS_OUTPUT_DIR}"
    puts "Done concatenating files"
  end
end

