source "https://rubygems.org"

gem "github-pages", group: :jekyll_plugins

gem "tzinfo-data"
gem "wdm", "~> 0.1.0" if Gem.win_platform?

# Stdlib gems that were removed from default gems in Ruby 3.4+ / 4.0
# Jekyll 3.9 (pinned by github-pages) still expects them to be built-in.
gem "csv"
gem "base64"
gem "bigdecimal"
gem "logger"
gem "ostruct"
gem "fiddle"
gem "mutex_m"
gem "webrick"

# If you have any plugins, put them here!
group :jekyll_plugins do
  gem "jekyll-paginate"
  gem "jekyll-sitemap"
  gem "jekyll-gist"
  gem "jekyll-feed"
  gem "jemoji"
  gem "jekyll-include-cache"
end
