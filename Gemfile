source "https://rubygems.org"

gem "github-pages", group: :jekyll_plugins
# `wdm` speeds up file watching on Windows, but its native extension currently
# fails to build on Ruby 3.4+, so skip it there.
gem 'wdm', '>= 0.1.0' if Gem.win_platform? && Gem::Version.new(RUBY_VERSION) < Gem::Version.new('3.4')
# Ruby 3.4 no longer includes some stdlib gems by default; Jekyll 3.9 expects them.
gem "csv"
gem "logger"
gem "webrick", "~> 1.8"
