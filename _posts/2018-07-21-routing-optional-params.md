---
layout: post
title:  "Routing With Optional Parameters"
date:   2018-07-21 12:02:25 +0800
categories: software-development ruby
---
If you need to get something or pass something that is not a very sensitive data, you can usually pass it in parameters as such:
{% highlight ruby %}
post_path(@post, optional_param: 'foo')
{% endhighlight %}

or,

{% highlight ruby %}
<%= link_to "Foobar", show_path(@post, optional_param: 'foo') %>
{% endhighlight %}

or yet another trick, if you are not sure of the type of record or parameter that will be passed to it:

{% highlight ruby %}
polymorphic_url(@post, optional_param: 'foo')
{% endhighlight %}