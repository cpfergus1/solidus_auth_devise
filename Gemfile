# frozen_string_literal: true

source 'https://rubygems.org'
git_source(:github) { |repo| "https://github.com/#{repo}.git" }

branch = ENV.fetch('SOLIDUS_BRANCH', 'master')

solidus_git, solidus_frontend_git = if (branch == 'master') || (branch >= 'v3.2')
                                      %w[solidusio/solidus solidusio/solidus_frontend]
                                    else
                                      %w[solidusio/solidus] * 2
                                    end

gem 'solidus_api', github: solidus_git, branch: branch
gem 'solidus_backend', github: solidus_git, branch: branch
gem 'solidus_core', github: solidus_git, branch: branch
gem 'solidus_frontend', github: solidus_frontend_git, branch: branch
gem 'solidus_sample', github: solidus_git, branch: branch

gem 'rails', ENV.fetch('RAILS_VERSION', nil)

case ENV['DB']
when 'mysql'
  gem 'mysql2'
when 'postgresql'
  gem 'pg'
else
  gem 'sqlite3'
end

if Gem::Version.new(RUBY_VERSION) < Gem::Version.new('3')
  # While we still support Ruby < 3 we need to workaround a limitation in
  # the 'async' gem that relies on the latest ruby, since RubyGems doesn't
  # resolve gems based on the required ruby version.
  gem 'async', '< 3', require: false

  # 'net/smtp' is required by 'mail', see:
  # - https://github.com/ruby/net-protocol/issues/10
  # - https://stackoverflow.com/a/72474475
  gem 'net-smtp', require: false
end

gemspec

# Use a local Gemfile to include development dependencies that might not be
# relevant for the project or for other contributors, e.g.: `gem 'pry-debug'`.
eval_gemfile 'Gemfile-local' if File.exist? 'Gemfile-local'
