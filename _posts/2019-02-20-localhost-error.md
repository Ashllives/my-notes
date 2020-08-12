---
layout: post
title:  "Fixing Localhost Address Already in Use Error"
date:   2019-02-20 12:02:25 +0800
categories: software-development ruby
---
Sometimes, localhosts freeze in the middle of an extremely heavy process that you are trying out. That leaves two choices: close the terminal or open another terminal. Either way, when you try to run your server again, you’ll run into this error:

{% highlight ruby %}
Address already in use - bind(2) (Errno::EADDRINUSE)
{% endhighlight %}

You can choose to run your server in another port to avoid running into this error by typing:

{% highlight ruby %}
rails s -p 3001 [or whatever port you wish to run your server on]
{% endhighlight %}

But you can also **FIX** this error by actually killing the process that is running on that port. To do that, first find out what is running on it:

{% highlight ruby %}
lsof -wni tcp:3000
{% endhighlight %}

This shows what is running on port 3000. You can, of course, change the port to whatever port you want to check. But what we really need from this command is the PID that we will be using as reference when we kill the process.

For example, this is the result of the above command:

{% highlight ruby %}
COMMAND  PID  USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
 ruby    19508 cee    8u  IPv4  16901     0t0  TCP 127.0.0.1:3000 (LISTEN)
{% endhighlight %}

Alternatively, you can go to your project’s root directory and find the pid file in `/tmp/pids/server.pid`

Using the PID, run this command to kill the currently running process:

{% highlight ruby %}
kill -9 19508
{% endhighlight %}

And the error should be gone the next time you run your local server.