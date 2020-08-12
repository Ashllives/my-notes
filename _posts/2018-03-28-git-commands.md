---
layout: post
title:  "How to: Version Control your Code with Git"
date:   2018-03-28 12:02:25 +0800
categories: software-development git
---
In writing codes, there would always be a time when you’d break up something in it or even break the whole thing. Sometimes, you might just want to look at the changes made on the code. For these cases, it is always handy to have copies of your code in version control.

**What is Version Control?**
> Version control (also known as revision control or source control) is a category of processes and tools designed to keep track of multiple different versions of software, content, documents, websites and other information in development. Any system that provides change tracking and control over programming source code and documentation can be considered version control software. -whatis.techtarget.com

This allows you to revert to specific versions of your code. Much like an undo button. 

**Other Benefits of Version Control**

Aside from rolling back to the previous version of your code in case of mishaps, there are other benefits of putting your code in version control softwares.

1. Create workflows – helps enforce a singular development process for all developers working on the same project.
2. Code together – synchronizes versions and prevents conflicts when making changes.
3. Keep a history – keeps track of who, why and when changes are made.
4. Automate tasks – automates testing, code analysis and deployment.

**What is Git?**

Git is the most commonly used version control system today. It is a distributed version control system, meaning your local copy of code is a complete version control repository. These fully-functional local repositories make it is easy to work offline or remotely. You commit your work locally, and then sync your copy of the repository with the copy on the server. 

**Useful Git Commands**

It really takes some time getting used to working with Git. Here are some of the most common and useful git commands you can use.

_Creating a New Branch in Local_

There are actually two ways to do this: the traditional way,
{% highlight ruby %}
git fetch --prune
git checkout --track -b [branch_name] origin/[branch_name]
git branch
{% endhighlight %}

or the newer, shorter one:
{% highlight ruby %}
git fetch --prune
git checkout [branch_name]
{% endhighlight %}

I use these commands too:
{% highlight ruby %}
git pull
git checkout -b [branch_name]
{% endhighlight %}

_Updating Local Branch_
{% highlight ruby %}
git fetch --prune
git checkout [branch_name]
git merge origin/[branch_name]
{% endhighlight %}

or 

{% highlight ruby %}
git pull origin [branch_name]
{% endhighlight %}

_Add File to Staging_
{% highlight ruby %}
git add --all
{% endhighlight %}

or to add one file:
{% highlight ruby %}
git add [path/to/file_name]
{% endhighlight %}

or to add specific line of code only in a file:
{% highlight ruby %}
git add [path/to/file_name] --patch
{% endhighlight %}

_Check Code Changes/Differences_

To view code changes on all updated files:
{% highlight ruby %}
git diff .
{% endhighlight %}

To view changes on a single file:
{% highlight ruby %}
git diff path/to/file_name
{% endhighlight %}

To view changes on staged files:
{% highlight ruby %}
git diff --staged
{% endhighlight %}

_Commit File_
{% highlight ruby %}
git commit -m "[commit_message]"
{% endhighlight %}

_Amend Previous Commit Message_
{% highlight ruby %}
git commit --amend
{% endhighlight %}

_Push File to Remote_
{% highlight ruby %}
git push origin [branch_name]
{% endhighlight %}

_Push Branch to Heroku_
{% highlight ruby %}
git push heroku [branch_name]:master
{% endhighlight %}

_Push Master to Heroku_
{% highlight ruby %}
git push heroku master
{% endhighlight %}

_Check Branches_
{% highlight ruby %}
git branch
{% endhighlight %}

The current branch will be with asterisk

There are a lot of git commands for different actions. But these are the commonly used ones.

_Notes taken from: [Whatis](https://whatis.techtarget.com/definition/version-control), [Microsoft Azure](https://docs.microsoft.com/en-us/azure/devops/learn/git/what-is-version-control), [Azure Docs](https://docs.microsoft.com/en-us/azure/devops/learn/git/what-is-git)_