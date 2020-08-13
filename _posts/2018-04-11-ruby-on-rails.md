---
layout: post
title:  "Learning RoR on the Go"
date:   2018-04-11 12:02:25 +0800
categories: software-development ruby
---
Since I’m obviously new to Rails, I have yet to learn these different migration commands. And though it’s fairly easy to trust Google on this, keeping it all together in one place makes it a whole lot easier for me. So here are the common migration problems I google all the time:

**Generate a Scaffold**

For a quick MVC setup, scaffolding is the way to go. Plus the command is easy enough to understand:
{% highlight ruby %}
rails g scaffold TableName field_name1:field_type field_name2:field_type
{% endhighlight %}

You can name as many fields as you want. Just keep in mind that the table name must be in CamelCase and in singular form.

**Generate a Model**

{% highlight ruby %}
rails g model ModelNameInSingular col_name:field_type
{% endhighlight %}

**Add New Column to an Existing Model**
{% highlight ruby %}
rails g migration AddColumnNameToModel col_name:field_type
rake db:migrate
{% endhighlight %}

**Change Column Name**

To change a column name or column names at one time, generate a migration file in the console:
{% highlight ruby %}
rails g migration ChangeColumnName
{% endhighlight %}

In the migration file, write rename_column inside the change method like this:

{% highlight ruby %}
class ChangeColumnName < ActiveRecord::Migration[5.1]
  def change
    rename_column :table_name_in_plural, :old_col_name, :new_col_name
  end
end
{% endhighlight %}

**Change Column Type**

In the same migration file (provided that you still haven’t migrated it), you can also update the column type. All you have to do is add another line to it:

{% highlight ruby %}
class ChangeColumnName < ActiveRecord::Migration[5.1]
  def change
    ...
    change_column :table_name_in_plural, :col_name, :desired_field_type
  end
end
{% endhighlight %}

Or if you have to do it in another migration file, then simply generate another one through your console.

{% highlight ruby %}
rails g migration ChangeColumnType
{% endhighlight %}

And in the new migration file created, just type the same change_column code snippet from earlier.

**Add Reference Column to a Model**

{% highlight ruby %}
rails g migration AddColumnNameToModel column_name:references
{% endhighlight %}

This will create a migration file with a foreign key `column_name` to the model `Model`. Generally, the migration file should look something like this.

{% highlight ruby %}
class AddColumnNameToModel < ActiveRecord::Migration[5.1]
  def change
    add_reference :model_name_in_plural, :column_name, foreign_key: true
  end
end
{% endhighlight %}

**Adding Default Value to a Boolean Attribute**

Just in case you forgot to specify the default value of your boolean attribute like I did, here’s how to do it. Generate a migration file, as always:

{% highlight ruby %}
rails g migration add_default_to_attribute
{% endhighlight %}

Then add the following lines to the change method:

{% highlight ruby %}
change_column :table_name_in_plural, :attribute, :boolean, default: true_or_false
{% endhighlight %}

**Delete a Column from an Existing Model**

To remove a column with a migration file:
{% highlight ruby %}
rails g migration RemoveColumnNameFromModel column_name:field_type
{% endhighlight %}

The migration file should look something like this:
{% highlight ruby %}
class RemoveColumnNameFromModel < ActiveRecord::Migration[5.1]
  def change
    remove_column :model_name_in_plural, :column_name, :field_type
  end
end
{% endhighlight %}

_Note: Removing a column with an existing index, removes the index as well._

**Remove an Index from a Table**

To remove an index from a given table:
{% highlight ruby %}
rails g migration RemoveIndexFromModel
{% endhighlight %}

The migration file looks like this:

{% highlight ruby %}
class RemoveIndexFromModel < ActiveRecord::Migration[5.1]
  def change
    remove_index :model_name_in_plural, column: :column_name
  end
end
{% endhighlight %}

**Create a Join Table without a Model**

For `has_and_belong_to_many` relationships, one approach is to create a join table without a model (HABTM). This will create a table that will hold the IDs of every entry from each model on the left and on the right. To create a join table:

{% highlight ruby %}
rails g migration CreateJoinTableModel1Model2 model1 model2
{% endhighlight %}

**Generate a Controller**

This is how you generate a controller:

{% highlight ruby %}
rails g controller ControllerName action1 action2
{% endhighlight %}

Note that the controller name must be in CamelCase and that it can take several actions as arguments in lower-case. This should already create routes, views, JavaScript and CSS files along with it too.

_Notes taken from: [medium.com](https://medium.com/forest-admin/rails-migrations-tricks-guide-code-cheatsheet-included-dca935354f22){:target="_blank"}, [tosbourn.com](https://tosbourn.com/rails-migrate-change-column-type/){:target="_blank"}, [railstutorial.org](https://www.railstutorial.org/){:target="_blank"}_