# Working

1. Add Spring gems to Gemfile

  > vi Gemfile

  group :test, :development do
    gem 'spring', '1.3.5'
    gem 'spring-commands-rspec', '1.0.4'
  end

2. To run tests from your engine root folder, you need to tell Spring to use
   your Combustion app dummy at 'spec/internal'

   > vi config/spring.rb

   Spring.application_root = 'spec/internal'

3. Spring needs a 'config/application.rb' on your dummy to run

  > vi spec/internal/config/application.rb

  ENV['BUNDLE_GEMFILE'] ||= File.expand_path('../../Gemfile', __FILE__)

  require 'bundler/setup'
  require 'rails'


4. Tell Spring to generate the bin stubs.

  > bundle exec spring binstub --all 
 
  (they will be generated at ./spec/internal/bin)

5. Use symlinks to sync your dummy gems with the engine

  > cd spec/internal
  > ln -s ../../gutentag.gemspec .
  > ln -s ../../Gemfile .
  > ln -s ../../Gemfile.lock .

6. Test the Spring server starts inside your dummy

  > cd spec/internal
  > spring stop
  Spring is not running

  > ./bin/rspec

  > spring status 
  Spring is running:

  92194 spring server | internal | started 1 min ago o
  92201 spring app    | internal | started 1 min ago | test mode

FAIL: 

Although the Spring server starts, running the ./bin/rspec command fails with:

  ... .gem/ruby/2.2.1/gems/spring-1.3.5/lib/spring/application.rb:92:
  in `require': no implicit conversion of Pathname into String (TypeError)`

Since the dummy has no './spec' folder no test should run. But running rspec
without errors was a test before complicating things more linking the dummy bin
stubs to the root of the engine


# Later ...

x. Since you want to run bin stubs from your engine root (not the combustion
   dummy), you must link them into your root, or just the whole ./bin folder

  > ln -s spec/internal/bin .

