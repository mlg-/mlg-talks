# debugging in ruby aka what is pry tech talk notes
## Intro
**what is it?**

pry is an alternative repl for ruby that is included in indigo. 

**ok but what’s a repl?**

repl stands for read-eval-print loop. basically it lets you shorten the feedback loop very significantly for interpreted languages: you type in an expression and when you’ve completed it, the repl evaluates it on the spot for you. this means you can find out FAST if your code doesn’t work. 

**cool, so how do I use it?**

if you open a rails console in indigo, you are in a pry repl. pry also lets you set a breakpoint in your ruby code so that you can pause execution and get feedback in realtime about how your code works (or, probably, doesn’t) 

**let’s see the most basic example**

put `binding.pry` somewhere and then try to make that code execute

cool, now we are stopped here so we can poke around a little in this context!

**you can use pry to pause execution of dependencies, too**

for example, I am often poking around in the rails core code or devise to understand how things are done. I use gem open to get the source code opened up and then throw my `binding.pry` in there!

**neat, but there is a LOT more**

as you can imagine, pausing execution is very powerful in and of itself. but there are also a ton of amazing things you can do once you’re in your breakpoint

## Understanding your context
**TMI or just enough** 

`ls` is for list, does something much like what it does in unix when you call it to see your file system. here  you get everything that’s in scope, and it’s a hell of a lot

so maybe you just want a little bit of that? you can pass flags to `ls`! some common ones:
`ls -i` for instance variables
`ls -l` for local vars
`ls -c` for constants

and many [more](https://github.com/pry/pry/wiki/State-navigation#learning-about-your-context-with-the-ls-command)

**convenience methods for stuff in your scope**

`_` gives you the result of the last statement you executed which is helpful when you want to use it next
`_ex_` last exception

**the call is coming from inside the house**

let’s say we can’t figure out why some code is executing from looking at the logs. maybe we think according to our flow control we should never hit a particular method. or maybe some dependency gem is messing with our result and we need to find out where: pry has a fun way to help you figure that out! 

`caller`

caller returns an array of file path strings. that means you can use good old ruby code to filter that if you know you want only lines from indigo, for example

`caller.select { |line| line.include? 'indigo' }`

you can also get the call stack specifically for the last exception with
 `wtf?` , a personal favorite:


```
[12] pry(main)> called_this_method_but_it_does_not_exist
NameError: undefined local variable or method `called_this_method_but_it_does_not_exist' for main:Object
from (pry):5:in `<main>'
[13] pry(main)> wtf?
Exception: NameError: undefined local variable or method `called_this_method_but_it_does_not_exist' for main:Object
--
0: (pry):5:in `<main>'
1: /Users/mlgore/.rbenv/versions/2.3.8/lib/ruby/gems/2.3.0/gems/pry-0.11.3/lib/pry/pry_instance.rb:355:in `eval'
2: /Users/mlgore/.rbenv/versions/2.3.8/lib/ruby/gems/2.3.0/gems/pry-0.11.3/lib/pry/pry_instance.rb:355:in `evaluate_ruby'
3: /Users/mlgore/.rbenv/versions/2.3.8/lib/ruby/gems/2.3.0/gems/pry-0.11.3/lib/pry/pry_instance.rb:323:in `handle_line'
4: /Users/mlgore/.rbenv/versions/2.3.8/lib/ruby/gems/2.3.0/gems/pry-0.11.3/lib/pry/pry_instance.rb:243:in `block (2 levels) in eval'
5: /Users/mlgore/.rbenv/versions/2.3.8/lib/ruby/gems/2.3.0/gems/pry-0.11.3/lib/pry/pry_instance.rb:242:in `catch'
6: /Users/mlgore/.rbenv/versions/2.3.8/lib/ruby/gems/2.3.0/gems/pry-0.11.3/lib/pry/pry_instance.rb:242:in `block in eval'
7: /Users/mlgore/.rbenv/versions/2.3.8/lib/ruby/gems/2.3.0/gems/pry-0.11.3/lib/pry/pry_instance.rb:241:in `catch'
8: /Users/mlgore/.rbenv/versions/2.3.8/lib/ruby/gems/2.3.0/gems/pry-0.11.3/lib/pry/pry_instance.rb:241:in `eval'
9: /Users/mlgore/.rbenv/versions/2.3.8/lib/ruby/gems/2.3.0/gems/pry-0.11.3/lib/pry/repl.rb:77:in `block in repl'
```

**help, i got lost at disneyworld**

if you start running some code interactively, you might lose track of where you were. to see the source code in your context, use `whereami` or `show-source -l` which is a little more verbose

## Gather more info without switching contexts

**even more source discovery**

you can use `show-method method_name` and `show-source method_name or Class, etc` to get allll kinds of helpful information about method calls you might notice in scope that you aren’t familiar with

**discover who else is calling what you’re looking at**

want to find out other methods that invoke one you’re looking at?
`find-method -c method_name`

**documentation discovery with ri**

know you want to use a method that is part of ruby or something and want the docs for it? we can do that here without having to leave, woot

`ri Array#reduce` or whatever your heart desires


## Manipulate your context
**and then you can change stuff, fast**

you can configure pry to open in your editor of choice if you’d like, and then editing the source for something you know you want to fix is mega-easy

`edit method_name`

**or even faster**

one of my favorite ways to write new code where I’m not 100% sure yet what I’m trying to do is to add a `binding.pry` in the context I know I will need to make some modification and then write code on the fly in the interpreter for faster feedback about whether or not it works.

this can be annoying if you make a mistake, so, for example, let’s say I’m writing a new method and I get the signature wrong

```
[8] pry(main)> def some_method
[8] pry(main)*   puts some_arg
[8] pry(main)*   amend-line 1 def some_method(some_arg)
1: def some_method(some_arg)  puts some_arg
[8] pry(main)*   end
=> :some_method
[9] pry(main)> some_method('hi')
hi
=> nil
```

ahh crap, I realize while this line that I forgot to pass in `some_arg` 

no worries, I can amend that: `amend-line LINE_NUMBER NEW_STUFF`

**or even fasterrrrr?**

YET STILL if you know what’s wrong and you can still fix it within the current execution, try an exit statement

`exit some_statement_to_make_this_work`

## Navigating & Saying byeeeeee

**when you’re ready to resume**

maybe you’re ready to keep going out of your breakpoint after all.

you can `step` if you still haven’t identified your problem and want to walk one step at a time through the code that follows

you can `exit` to leave your breakpoint and resume execution as normal

you can also `!!!` or `exit-program` for the big bail/abort mission

## Where to learn more
Somewhat unbelievably this is still only a very small sampling of the kinds of things you can do with `pry`. It is very powerful and has lots of additional features I haven’t discussed today. It also supports a plugin ecosystem that extends it further. I encourage you to check out the [docs](https://github.com/pry/pry/wiki) and the [plugin page](https://github.com/pry/pry/wiki/Available-plugins) for more info!


