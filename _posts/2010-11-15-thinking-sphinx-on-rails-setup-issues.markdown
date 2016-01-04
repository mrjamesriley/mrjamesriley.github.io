---
layout: post
title: "Thinking Sphinx on Rails setup issues"
date: 2010-11-15 06:02
comments: true
categories:
---
Sphinx is a full-text search server, able to index data from sources such as a database and make up for what traditional databases like MySQL lack. With use of the Thinking Sphinx gem, setup and usage with Ruby on Rails is real easy – yet there is a hurdle or two I stumbled upon:

1) Searchd daemon cannot be found

This error occurred when attempting to deploy on production, and research on the internet yielded little in the way of solutions. The executable was present and included in the path, with the issue being down to running the thinking_sphinx:index rake task without specifying the rails environment.

Now this is not documented anywhere so there could be something I’ve missed during the configuration. The call:

{% highlight ruby %}
  rake thinking_sphinx :index RAILS_ENV=production
{% endhighlight %}


will do as expected, creating the configuration file based on the ‘define_index’ block in your Rails models and indexing as defined. A similar problem has caught many out before, such as leaving out the RAILS_ENV when performing a db:migrate. A more helpful error message from Thinking Sphinx would certainly be helpful.

2) Partial word searching

The Thinking Sphinx documentation mentions ‘Automatic Wildcards’ within its searching page yet this fails to work for partial words. Say I have an article titled ‘never enough’, the document would be retrieved if the search was for ‘never’, but not for simply ‘nev’. In my case, I’m using Thinking Sphinx during an auto complete search and so would require such functionality.

The solution is present within the ‘Advanced Sphinx Configuration’ page and all that’s required is to include the following line within your sphinx.yml in the config directory of your Rails application:

{% highlight ruby %}
min_prefix_len: 3
{% endhighlight %}

where 3 is the minimum number of letters till a search takes place. Note that this line can also be used within individual ‘define_index’ blocks if you do not want it to apply globally to all indexes within the same environment.
