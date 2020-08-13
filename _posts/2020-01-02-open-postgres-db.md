---
layout: post
title:  "How to Open Postgres DB in Terminal"
date:   2020-01-02 12:02:25 +0800
categories: software-development ruby
---

Using `psql`, one can interact with PostgreSQL from the terminal. Here is how to connect to a database:

{% highlight ruby %}
psql -h localhost -U [username] [database_name]
{% endhighlight %}

The flags used here were:

- -h which pertains to the host to connect to
- -U which pertains to the user to connect with
- -p which pertains to the port to connect to (default is 5432)

_Notes taken from: [postgresguide.com](http://postgresguide.com/utilities/psql.html){:target="_blank"}_