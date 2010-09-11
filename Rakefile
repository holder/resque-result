require 'rake/testtask'
require 'rake/rdoctask'

def command?(command)
  system("type #{command} > /dev/null")
end

#
# Tests
#

task :default => :test

if command? :turn
  desc "Run tests"
  task :test do
    suffix = "-n #{ENV['TEST']}" if ENV['TEST']
    sh "turn test/*_test.rb #{suffix}"
  end
else
  Rake::TestTask.new do |t|
    t.libs << 'lib'
    t.pattern = 'test/**/*_test.rb'
    t.verbose = false
  end
end

#
# Gems
#

begin
  require 'mg'
  MG.new("resque-result.gemspec")
rescue LoadError
  warn "mg not available."
  warn "Install it with: gem i mg"
end

desc "Tag and publish a new version to Rubygems"
task :publish => [ :test, 'gem:publish' ] do
  require 'lib/resque/plugins/result/version'

  sh ["git tag v#{Resque::Plugins::Result::Version}",
      "git push origin v#{Resque::Plugins::Result::Version}",
      "git push origin master"].join(' && ')
end
