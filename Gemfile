# frozen_string_literal: true

source "https://rubygems.org"

gem "jekyll-theme-chirpy", "~> 7.6"

#allows links from old Blogger site to continue working
gem 'jekyll-redirect-from'

gem "github-pages", group: :jekyll_plugins

gem 'jekyll-compose', group: [:jekyll_plugins]

gem "html-proofer", "~> 5.0", group: :test

platforms :windows, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

gem "wdm", "~> 0.2.0", :platforms => [:windows]
