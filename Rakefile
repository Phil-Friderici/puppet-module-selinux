require 'puppetlabs_spec_helper/rake_tasks'
require 'puppet-lint/tasks/puppet-lint'
PuppetLint.configuration.send('disable_80chars')
PuppetLint.configuration.send('disable_140chars')
PuppetLint.configuration.relative = true
PuppetLint.configuration.ignore_paths = ['spec/**/*.pp', 'pkg/**/*.pp', 'vendor/**/*.pp']

desc 'Validate manifests, templates, and ruby files'
task :validate do
  Dir['spec/**/*.rb', 'lib/**/*.rb'].each do |ruby_file|
    sh "ruby -c #{ruby_file}" unless ruby_file =~ /spec\/fixtures/
  end
  Dir['files/**/*.sh'].each do |shell_script|
    sh "bash -n #{shell_script}"
  end
end

# Puppet Strings (Documentation generation from inline comments)
# See: https://github.com/puppetlabs/puppet-strings#rake-tasks
require 'puppet-strings/tasks'

desc 'Alias for strings:generate'
task :doc => ['strings:generate']

desc 'Alias for strings:gh_pages:update'
task :doc_update => ['strings:gh_pages:update']

desc 'Run validate, lint and spec tests.'
task :test do
  [:syntax, :validate, :lint, :spec, :doc].each do |test|
    Rake::Task[test].invoke
  end
end

desc 'Release a new version.'
task :release do
  [:build, :doc_update].each do |t|
    Rake::Task[t].invoke
  end
end
