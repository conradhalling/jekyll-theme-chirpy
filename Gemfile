# frozen_string_literal: true

source "https://rubygems.org"

gemspec

gem 'logger', '~> 1.6', '>= 1.6.5'
gem 'csv', '~> 3.3', '>= 3.3.2'
gem 'base64', '~> 0.2.0'

gem "html-proofer", "~> 5.0", group: :test

platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

gem "wdm", "~> 0.2.0", :platforms => [:mingw, :x64_mingw, :mswin]
