require 'erb'
require 'fileutils'
require 'rake/testtask'

desc "Run all tests"
task :test do
  $LOAD_PATH.unshift(File.expand_path('test'))
  require 'redgreen' if Gem.available?('redgreen')
  require 'test/unit'
  Dir['test/*/**/test_*.rb'].each {|test| require test }
end

desc "Recompile the Racc parser (pass -v and -g for verbose debugging)"
task :build, :extra_args do |t, args|
  sh "racc #{args[:extra_args]} -o lib/coffee_script/parser.rb lib/coffee_script/grammar.y"
end

desc "Build the documentation page"
task :doc do
  rendered = ERB.new(File.read('documentation/index.html.erb')).result(binding)
  File.open('index.html', 'w+') {|f| f.write(rendered) }
end

namespace :gem do

  desc 'Build and install the coffee-script gem'
  task :install do
    sh "gem build coffee-script.gemspec"
    sh "sudo gem install #{Dir['*.gem'].join(' ')} --local --no-ri --no-rdoc"
  end

  desc 'Uninstall the coffee-script gem'
  task :uninstall do
    sh "sudo gem uninstall -x coffee-script"
  end

end

task :default => :test