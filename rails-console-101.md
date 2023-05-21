# Rails Console 101

## What is it?
The Rails console is a way to load your app so that you can work with it and its data interactively.

## Why do you use it?
Things I do on a daily basis with my Rails console:
- checking out data: what attributes do particular database records have?
- testing little bits of code in an interactive environment where I can validate my ideas before putting them into the bigger, slower context of the application code.
- benchmarking snippets of code to understand how performant one approach is compared to another (Ruby includes [a module](https://ruby-doc.org/stdlib-2.3.0_preview1/libdoc/benchmark/rdoc/Benchmark.html) for this purpose.)
- testing data-modifying operations that might screw up my local database (more on that in a minute)
- (on remote machines) checking out the state of data for problems, fixing little data inconsistency issues (not! in prod, I really advise not doing this!... generally on playpens)
- very ocassionally to query the reporting database, and you might also like to be able to do this

## How do I get to it?
You'll need to have an Indigo development environment set up on your machine (you can also use the console on a playpen), and then it's as simple as running `rails c` in that directory. You can also pass an environment if you need to do that for some reason, although usually the default of development will be right for you (although some users may typically connect to the reporting database instead).

## Interacting with the database through the Rails console

### ActiveRecord

### What is it?
ActiveRecord is what we call an ORM - an Object-Relational Mapping for the data in our Postgres database. If you've ever used SQL to ask questions about our data, Active Record acts as a wrapper around that for your comfort and ease of use. Think of it like this: all wine bottle openers are effectively just a corkscrew, but some of them have a really nice comfortable handle to use that helps you get leverage on the cork without having to use too much force. ActiveRecord makes it a little more natural for Ruby programmers to quickly get data from the database, although it still helps a lot to understand how relational databases work in general for what it's doing to feel as useful as it can be.

### A brief interlude on relational databases and object-oriented programming
The thing about ActiveRecord that is really nice is that it lets us use some of the paradigms of object oriented programming and relational databases together. Relational databases, like the one we use for Indigo (Postgres) let us compose tables of information to find rows that are relevant to us for a given data question. 

Relational databases let us compose data tables together to answer these questions by using normalization, which is a fancy way of saying we try not to repeat ourselves. We accomplish this by using primary and foreign keys to match rows that have a relationship. Check out a somewhat dated example of how this is done in [Indigo](https://github.com/actblue/indigo/wiki/pdfs/data_model_high_level.pdf). These relationships often map to an object-oriented paradigm, which is a way of thinking about programming that prioritizes objects and how they relate to eachother. The idea is that it's easier if we think about our tables/classes like we do about things in the real world, as things that can belong to each other, be like one another (inheritance) have many or just one of another, etc.

## Object attributes
You can find things out about the different models we have just by typing the name of the table/model with a capital first letter, this will show you that model's database attributes. (Bear in mind that sometimes gems that add functionality are hard to see this way):
```
User
```

## Queries
Say you want to find a particular record or set of records... you can! This is done with `find_by` or `find` for a singular record, or `where` for a collection of records.

Let's start with `find_by`. Let's say you looked at the `User` model and you know it has an email address attribute. Super. Now you can use `find_by` to get just that one:

```
User.find_by(email: 'melissa@actbluetech.com')
```
Keep in mind that `find_by` returns the very first record that matches your criteria, usually sorted by when it was created in ascending order. So if you know there could be more than one that you want, you might want to start with a `where` query... or just ask ActiveRecord to order the records before grabbing one for you (more on that in a minute).

`find` is used to find a singular record where you already know its primary key identifier, so for example:

```
User.find(1)
```

Be careful with find vs find_by when programming -- the former will raise an error if the record is not found, whereas the latter will just return empty.

Use `where` when you want to look at a set of records and then do something with them -- look at all attributes of a particular subset, run some code on them and see what happens, etc. Here's an example:

```
 User.where(authenticator_confirmed: true)
```

So... this is a lot of records. Maybe I want to order them so the most recently created are first? I can modify the `where` with an `order`:

```
User.order(created_at: :desc).where(authenticator_confirmed: true)
```

This query is kinda slow because there's no index on this column and the table is big, so we can also use a `limit` to return fewer records. Sometimes we don't need EVERY record of a particular type, we just need, say, ten so that we can find a pattern.

```
User.order(created_at: :desc).where(authenticator_confirmed: true).limit(10)
```

And maybe we don't need ALL the attributes of each record to understand what we're trying to learn, so we can just `pluck` what we care about and then manipulate it more from there to answer questions:

```
User.order(created_at: :desc).where(authenticator_confirmed: true).limit(10).pluck(:second_factor_attempts_count)
```

## Modifying records
Once you find a record, you might need or want to modify it. You can do that with `update`, and you can chain that right on to the query you used to find the record! For exaaaaaample, you might want to unexpire your local account, or unlock it, or change its password.

```User.find(1).update(password: '$upersecurepassword1234', password_confirmation: '$upersecurepassword1234')
```

You can usually tell if what you did worked, because it will either say COMMIT or ROLLBACK in the output (the former is work-y, the latter is womp-y).

## Other console commands that don't have to do with ActiveRecord

To quit:

```
exit
```

Lots of things with pry can be done from the rails console, which is backed by pry and triggered during runtime when you put a `binding.pry` into your code. See what I talked about with that in a previous [installment](https://gist.github.com/mlg-/c740e41381ccd66e80b6f8929fd1dcd4).


## Playing around without breaking the world
My most useful and favorite way to open a rails console is to use the `--sandbox` flag. This wraps allllll of your activity for the entire session into a database transaction and rolls it back afterward. This is a great way to experiment while trying to write data-modifying rake tasks, or just if you want to play arond with practice queries without messing anything up.

