---
layout: post
title:  "For Local Network based Development"
date:   2023-12-07 21:20:05 -0500
categories: jekyll update development
---
Often times, we use virtualized environment, and let the hosting side's browser to test newly loaded app.

For testing Jekyll, execute `jekyll serve` command with `--host 0.0.0.0` option.

{% highlight bash %}
bundle exec jekyll serve --host 0.0.0.0 --livereload
{% endhighlight %}

Then, Jekyll website can be served also to locally networked machines,

accessible via URL like `{ComputerName}.local:4000`

Reference: [https://zarino.co.uk/post/jekyll-local-network/](https://zarino.co.uk/post/jekyll-local-network/)
