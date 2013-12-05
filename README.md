A local Gemfile where you can add gems that you don't want to add to the repository

# Install

Copy Gemfile.local into project root and add the following to your .gitignore:

	/Gemfile.local
	/Gemfile.local.lock

To use Gemfile.local run:

	bundle --gemfile Gemfile.local

Gemfile.local March 30, 2012 source: http://jeybalachandran.com
Bundler is excellent for managing dependencies of my applications but not for managing my custom dependencies. There are gems that I’d like to use locally, knowing their implications, without forcing that behavior on other developers or designers on the team. One such gem is rails-dev-boost, which speeds up my development environment considerably. Gemfile.local solves this limitation.

Create Gemfile.local at Rails.root with the following content:
    eval File.read('Gemfile')
    group :development do
      gem 'rails-dev-boost', :git => 'git://github.com/thedarkone/rails-dev-boost.git', :require => 'rails_development_boost'
    end

Then execute these commands from Rails.root:
    cp Gemfile.lock Gemfile.local.lock
    BUNDLE_GEMFILE=Gemfile.local bundle
    echo "$(cat .gitignore)\nGemfile.local\nGemfile.local.lock" > .gitignore
    git commit -m "Added Gemfile.local to .gitignore." .gitignore

Whenever you need to use your custom Gemfile.local you can prepend the command with:
    BUNDLE_GEMFILE=Gemfile.local bundle exec

For example, start the rails server by executing:
    BUNDLE_GEMFILE=Gemfile.local bundle exec rails s

Enjoy the benefits of your custom Gemfile.

Note: This should store a config in the .bundle/config file and then read from that, however I couldn't get this to work for the BUNDLE_GEMFILE config no matter what I tried. There is a bug request to add this as standard functionality to Bundler, however it appears there is nobody around to work on it:
https://github.com/carlhuda/bundler/issues/183#issuecomment-595008 
