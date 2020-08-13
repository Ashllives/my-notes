---
layout: post
title:  "On Ruby Lambdas"
date:   2020-03-10 12:02:25 +0800
categories: software-development ruby
---

**KEY POINTS**

Lambdas behave like functions in that it enforces arity (number of arguments)
{% highlight ruby %}
a = lambda {|argument| puts argument}
a.call # ArgumentError: wrong number of arguments (0 for 1)
a.call("Hello World!") # this works!
{% endhighlight %}


Lambda functions also supports default arguments
{% highlight ruby %}
a = lambda { |argument = "Hello World!"| puts argument }
a.call # this works now
{% endhighlight %}

Return statement returns and exits from the lambda
{% highlight ruby %}
def foo
  puts "before lambda"
  a = lambda { |bar| return }
  puts "after lambda"
end

foo # returns and exits from lambda only; puts are printed
{% endhighlight %}


Lambdas are also closures (means they can be stored in variables and has the ability to carry **reference** to the values of local variables and methods from the context where it was defined. Thus, it returns the latest value of the variables). For example:

{% highlight ruby %}
def call_lambda(my_lambda)
  count = 500
  my_lambda.call
end

count = 1
my_lambda = lambda { puts count }

lam = call_lambda(my_lambda) # this prints latest value of count
{% endhighlight %}


How does this happen? Lambdas store these scope values to itself through the binding method
{% highlight ruby %}
# binding method
def return_binding
  foo = 100
  binding
end

puts return_binding.class # Binding class
puts return_binding.eval('foo') # 100
puts foo # nil
{% endhighlight %}

_NOTE: bindings are not used directly. For next time, binding may be used to debug at a certain scope_

Application to lambda:
{% highlight ruby %}
def make_lambda
  hash = {attr: 'value'}
  lambda { hash[:attr] }
end

make_lambda.call # remembers 'value' because of binding method even if call is already out of scope 
{% endhighlight %}

Because of this ability, the body of the lambda is evaluated in the context of the current instance of the object
{% highlight ruby %}
def run(function, argument)
  function.call(argument)
end
 
double = lambda { |x| x * 2 }
run double, 16
#=> 32

# if we override the value of the argument from inside the function run
def run(function, argument)
  argument = 2
  function.call(argument)
end
 
double = lambda { |x| x * 2 }
run double, 16
#=> result is 4 because it evaluates 2 instead of 16 
{% endhighlight %}

**Cool Lambda Examples**
{% highlight ruby %}
class FakePerson
  def initialize(options = {})
    puts 'entered initialize'
    puts 'A----'
    @first_name = options[:first_name]
    puts 'B----'
    @last_name = options[:last_name]
    puts '---------'
    puts 'done initializing'
  end

  def call
    return first_name, last_name
  end
end

require 'faker'
fuzzer = lambda { |k| puts 'inside fuzzer'; Faker::Name.send(k) }
FakePerson.new(fuzzer) # returns random names yay!
{% endhighlight %}


_Notes taken from: [Ruby for Admins: Lambda Functions](http://rubyforadmins.com/lambda-functions){:target="_blank"}, [Ruby Blocks, Procs & Lambdas - The Ultimate Guide!](https://www.rubyguides.com/2016/02/ruby-procs-and-lambdas/){:target="_blank"}, [Using Lambdas in Ruby - Honeybadger Developer Blog](https://www.honeybadger.io/blog/using-lambdas-in-ruby/){:target="_blank"}, [ruby - Skip instance in conditional callback block (lambda or Proc) - Stack Overflow](https://stackoverflow.com/questions/28829717/skip-instance-in-conditional-callback-block-lambda-or-proc){:target="_blank"}_

