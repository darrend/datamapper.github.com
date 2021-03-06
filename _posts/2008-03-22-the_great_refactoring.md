---
layout:       articles
categories:   articles
title:        The Great Refactoring
created_at:   2008-03-22T15:55:50-05:00
summary:      Change is afoot
author:       afrench and ssmoot
---

{{ page.title }}
================

"Tip" DataMapper (hosted on [github](http://github.com/datamapper/dm-core)) is
going through a dramatic re-factor. Here's a quick summary of the anticipated
NEW public API.

Not Just for Databases Anymore
------------------------------

DataMapper's class terminology will change and 'de-couple' itself from
database-specific terminology. Gone are "database", "table", "column" and
"join". Say hello to "Repository", "Resource", "Property", and "Link".

"Why would you want to do that?", you ask. Ultimately it's because DataMapper
will soon support different types of persistence layers, not just databases.
It'll talk to all sorts of things like web services (REST and such), XML files,
YAML files, non-relational databases, even custom file-types or services of your
own design. Just implement an Adapter that conforms to a certain API and
DataMapper could support any type of data store. No need for a completely
separate library or anything.

As an added benefit, DataMapper become more "RESTful". [Ryan Tomayko](http://tomayko.com/writings/rest-to-my-wife)
has a very good explanation
of REST that all should read entitled [How I Explained REST to My Wife](http://tomayko.com/writings/rest-to-my-wife).

Model Definitions Are a Little Different
----------------------------------------

Since we're changing up the terminology, model definitions are going to change
up a little bit. Here's what a Planet model would look like using the new API:

{% highlight ruby linenos %}
class Planet
  include DataMapper::Resource

  resource_names[:legacy] = 'dying_planets'

  property :name, String
  property :age,  Integer
  property :core, String,  :private => true
end
{% endhighlight %}

A couple of things are going on here. First, DataMapper::Base and
DataMapper::Persistence are gone and replaced with [DataMapper::Resource][DataMapper_Resource].
Next `set_table_name` has been replaced with
`resource_names` hash where you specify which arena play occurs in. After that
we have a couple of Property definitions that look a little different.

First off, Properties will no longer take `:symbols` for their types and instead
take real constants like String, Integer, DateTime. Also on the docket are the
ability to define your own custom types.

Think about that for a minute. If developers are able to define their own custom
types with their own materialization and serialization methods, DataMapper will
be able to support all kinds of wild data-types like GIS information, network
information, marshaled objects, JSON...pretty much anything a developer might
need, or want.

A Command-Line Interface
------------------------

Taking a lesson from web frameworks, DataMapper will sport an interactive
command-line so that you can browse your resources without the need to load up
the entire environment of your application. Here's an example session:

{% highlight bash linenos %}
$ dm mysql://root@localhost/great_musicians  # connecting to a repository
{% endhighlight %}

An IRB session boots up...

{% highlight ruby %}
>> the_king = Person.first(:name => 'elvis')
>> the_king.alive?  # => maybe
{% endhighlight %}

This is very similar to `script/console` in Rails or `merb -i` in Merb, only it
won't load up the entire environment of your application, just your DataMapper
resources and their associations, methods, and such. If you prefer "fat models",
this will constitute the core of your application.

How This All Comes Together
---------------------------

This is the coolest new feature of DataMapper: we're skipping all the way from
0.3.0 to 0.9! Get excited, contact the press, fire up the blogosphere! Its a
huge jump and we're honestly concerned that people may not be able to handle it.

Alright, so it's not _that_ big of a deal, but we're confident that all of this
will get DataMapper so close to going 1.0 that we'll be able to taste it. To get
there, DataMapper's more advanced features like single table inheritance,
paranoia, and chained associations will be re-implemented to use all this new
stuff, and then we're sure 0.9 will need a touch up or two.

So close....so very very close...

Stay tuned in to the [mailing list](http://groups.google.com/group/datamapper),
check up on the [wiki](http://datamapper.org/), chat it up in
[#datamapper](irc://irc.freenode.net/%23datamapper) and watch
[github commit messages](http://github.com/datamapper/dm-core/commits/master) for updates.

[DataMapper_Resource]:http://rubydoc.info/github/datamapper/dm-core/master/DataMapper/Resource
