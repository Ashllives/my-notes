---
layout: post
title:  "Solving Duplicate Keys in PostgreSQL"
date:   2019-02-15 12:02:25 +0800
categories: software-development ruby
---

Adding new data on a table should not be a problem. However, if there are already existing entries on a table with the ID value explicitly provided during a migration, some issues may arise. One such issue is:

{% highlight ruby %}
PG::UniqueViolation: ERROR:  duplicate key value violates unique constraint "your_table_pkey"
DETAIL:  Key (id)=(2) already exists.
{% endhighlight %}

On such cases, this means that the serial column sequence value is not updated. This is fairly easy enough to fix though. Simply run:

{% highlight ruby %}
SELECT setval('your_sequence_name', (SELECT max(id) FROM your_table));
{% endhighlight %}

What this does is it resets the sequence object’s counter value. The first parameter accepted is the sequence name of a column and the second parameter is the value from which the sequence will be advanced. You may actually pass a third parameter (optional) which is a boolean — true, meaning that the next value will advance the sequence before returning a value; false, meaning the next value returned will be exactly the specified value, and sequence advancement commences with the following value. The third parameter defaults to `true`.

An example from PostgreSQL documentation:

{% highlight ruby %}
SELECT setval('foo', 42); ====> Next nextval will return 43 
SELECT setval('foo', 42, true); ====> Same as above 
SELECT setval('foo', 42, false); ====> Next nextval will return 42
{% endhighlight %}

To find out the sequence name:
{% highlight ruby %}
SELECT currval(pg_get_serial_sequence('your_table', 'id'));
{% endhighlight %}

> pg_get_serial_sequence returns the name of the sequence associated with a column, or NULL if no sequence is associated with the column. If the column is an identity column, the associated sequence is the sequence internally created for the identity column. The function returns a value suitably formatted for passing to sequence functions (see [Section 9.16](https://www.postgresql.org/docs/current/functions-sequence.html)). A typical use is in reading the current value of a sequence for an identity or serial column.

Or, simply type `\d` and all the tables and their sequence name will be listed.

{% highlight ruby %}
                          List of relations
 Schema |                 Name                  |   Type   |  Owner
--------+---------------------------------------+----------+----------
 public | balloons                              | table    | postgres
 public | balloons_id_seq                       | sequence | postgres
(3 rows)
{% endhighlight %}

In this example, `balloons_id_seq` is your sequence name.

After this, your database sequence should be updated and you may start inserting new rows on your table without running on duplicate key issues.

_Notes taken from: [Functions Sequence](https://www.postgresql.org/docs/8.2/functions-sequence.html){:target="_blank"}, [StackExchange](https://dba.stackexchange.com/questions/46125/why-does-postgres-generate-an-already-used-pk-value){:target="_blank"}, [Functions Info](https://www.postgresql.org/docs/current/functions-info.html#FUNCTIONS-INFO-CATALOG-TABLE){:target="_blank"}_