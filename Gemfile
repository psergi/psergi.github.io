source "https://rubygems.org"
ruby '2.5.0'

require 'rbconfig'
if RbConfig::CONFIG['target_os'] =~ /darwin(1[0-3])/i
  gem 'rb-fsevent', '<= 0.9.4'
end

gem "jekyll", "3.8.5"
gem 'jekyll-compose', group: [:jekyll_plugins]
