# Change Agent Demo

See [the change agent documentation](https://github.com/benbalter/change_agent) for more details.

Here's an example script to track changes in the White House RSS feed over time. This repository is the resulting data store.

```ruby
require "change_agent"
require 'open-uri'
require "rss"

# init a change agent data store in the ./data folder
change_agent = ChangeAgent.init "data"

# fetch the White House blog RSS feed and parse
url = "http://www.whitehouse.gov/feed/blog/white-house"
data = open(url).read
feed = RSS::Parser.parse data

# loop through each post
feed.items.each do |post|

  # do some slight string manipulation on the URL to generate a friendly key
  key = post.link.sub("http://www.whitehouse.gov/blog/", "") + ".html"

  # Set the post content as the value
  value = post.description

  # Save via change agent
  change_agent.set key, value
end
```
