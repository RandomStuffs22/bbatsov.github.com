---
layout: post
title: "The Totally Unofficial Ruby coding style guide"
categories:
- Programming
- Ruby
---

## Prologue

This document was created when I, as the Technical Lead of the company
in which I work for, was asked by the CTO to create some internal
documents describing good style and best practices for Ruby
programming. Since I didn't find any "official" coding guidelines I
started off by copying
[this existing style guide](https://github.com/chneukirchen/styleguide),
because I concurred with most of the points in it. Afterwards I
proceeded to built upon it and plan to improve it a lot more (the
examples now are totally superficial, but I'm quite busy these
days). I hope it will be useful to other people as well and I hope
that I'll get a lot of feedback and suggestions on how to improve the
guide for the benefit of the entire Ruby community.

## The Guide
# Formatting

* Use UTF-8 as the source file encoding.
* Use 2 space indent, no tabs. (Your editor/IDE should have a setting to help you with that)
* Use Unix-style line endings. (Linux/OSX users are covered by default, Windows users have to be extra careful)
    * if you're using Git you might want to do this `$ git config --global core.autocrlf true` to protect your project from Windows line endings creeping into your project
* Use spaces around operators, after commas, colons and semicolons, around { and before }.

{% highlight ruby %}
sum = 1 + 2
a, b = 1, 2
1 > 2 ? true : false; puts "Hi"
[1, 2, 3].each { |e| puts e }
{% endhighlight %}

* No spaces after (, [ and before ], ).

{% highlight ruby %}
some(arg).other
[1, 2, 3].length
{% endhighlight %}

* Indent **when** as deep as **case**. (as suggested in the Pickaxe)

{% highlight ruby %}
case
when song.name == "Misty"
  puts "Not again!" when song.duration > 120
  puts "Too long!" when Time.now.hour > 21
  puts "It's too late"
else
  song.play
end

kind = case year
       when 1850..1889 then "Blues"
       when 1890..1909 then "Ragtime"
       when 1910..1929 then "New Orleans Jazz" when 1930..1939 then "Swing"
       when 1940..1950 then "Bebop"
       else "Jazz"
       end
{% endhighlight %}

* Use an empty line before the return value of a method (unless it
  only has one line), and an empty line between defs.

{% highlight ruby %}
def some_method
  do_something
  do_something_else

  result
end

def some_method
  result
end
{% endhighlight %}
* Use RDoc and its conventions for API documentation.  Don't put an
  empty line between the comment block and the **def**.
* Use empty lines to break up a method into logical paragraphs.
* Keep lines fewer than 80 characters.
    * Emacs users should really have a look at whitespace-mode
* Avoid trailing whitespace.
    * Emacs users - whitespace-mode again comes to the rescue

# Syntax

* Use **def** with parentheses when there are arguments. Omit the
  parentheses when the method doesn't accept any arguments.
  {% highlight ruby %}
def some_method
  # body omitted
 end

def some_method_with_arguments(arg1, arg2)
  # body omitted
end
{% endhighlight %}
* Never use **for**, unless you exactly know why. Most of the time
  iterators should be used instead.
  {% highlight ruby %}
arr = [1, 2, 3]

# bad
for elem in arr do
  puts elem
end

# good
arr.each { |elem| puts elem }
{% endhighlight %}
* Never use **then** for multiline **if/unless**.
{% highlight ruby %}
# bad
if x.odd? then
  puts "odd"
end

# good
if x.odd?
  puts "odd"
end
{% endhighlight %}
* Use **when x; ...** for one-line cases.
* Use &&/|| for boolean expressions, and/or for control flow.  (Rule
  of thumb: If you have to use outer parentheses, you are using the
  wrong operators.)
* Avoid multiline ?: (the ternary operator), use **if/unless** instead.
* Favor modifier **if/unless** usage when you have a single-line body.
{% highlight ruby %}
# bad
if some_condition
  do_something
end

# good
do_something if some_condition

# another good option
some_condition && do_something
{% endhighlight %}

* Favor **unless** over **if** for negative conditions:
{% highlight ruby %}
# bad
do_something if !some_condition

# good
do_something unless some_condition

# another good option
some_condition || do_something
{% endhighlight %}
* Suppress superfluous parentheses when calling methods, but keep them
  when calling "functions", i.e. when you use the return value in the
  same line.
{% highlight ruby %}
x = Math.sin(y)
array.delete e
{% endhighlight %}

* Prefer {...} over do...end for single-line blocks.  Avoid using {...} for multi-line blocks.  Always use do...end for
  "control flow" and "method definitions" (e.g. in Rakefiles and
  certain DSLs.)  Avoid do...end when chaining.

* Avoid **return** where not required.
{% highlight ruby %}
# bad
def some_method(some_arr)
  return some_arr.size
end

# good
def some_method(some_arr)
  some_arr.size
end
{% endhighlight %}

* Avoid line continuation (\\) where not required. In practice avoid using line continuations at all.
{% highlight ruby %}
# bad
result = 1 + \
2

# good
result = 1 \
+ 2
{% endhighlight %}
* Using the return value of = is okay:
{% highlight ruby %}
if v = array.grep(/foo/) ...
{% endhighlight %}
* Use \|\|= freely.
{% highlight ruby %}
# set name to Bozhidar, only if it's nil or false
name ||= "Bozhidar"
{% endhighlight %}
* Avoid using Perl-style global variables(like $0-9, $`, ...)

# Naming

* Use snake_case for methods and variables.
* Use CamelCase for classes and modules.  (Keep acronyms like HTTP,
  RFC, XML uppercase.)
* Use SCREAMING_SNAKE_CASE for other constants.
* The length of an identifier determines its scope.  Use one-letter variables for short block/method parameters, according to this scheme:

      a,b,c: any object
      d: directory names
      e: elements of an Enumerable
      ex: rescued exceptions
      f: files and file names
      i,j: indexes
      k: the key part of a hash entry
      m: methods
      o: any object
      r: return values of short methods
      s: strings
      v: any value
      v: the value part of a hash entry
      x,y,z: numbers

   And in general, the first letter of the class name if all objects are of that type.

* When using **inject** with short blocks, name the arguments **\|a, e\|** (mnemonic: accumulator, element)
* When defining binary operators, name the argument "other".
{% highlight ruby %}
def +(other)
  # body omitted
end
{% endhighlight %}
* Prefer **map** over *collect*, **find** over *detect*, **find_all** over *select*, **size** over *length*. This is not a hard requirement, though - if
the use of the alias enhances readability - it's ok to use it.

# Comments

* Write self documenting code and ignore the rest of this section.
* Comments longer than a word are capitalized and use punctuation. Use two spaces after periods.
* Avoid superfluous comments.
* Keep existing comments up-to-date - no comment is better than an outdated comment.

# Misc

* Write **ruby -w** safe code.
* Avoid hashes-as-optional-parameters.  Does the method do too much?
* Avoid long methods (longer than 10 LOC). Ideally most methods will be shorter than 5 LOC. Empty line do not contribute to the relevant LOC.
* Avoid long parameter lists (more than 3-4 params).
* Use **def self.method** to define singleton methods. This makes the methods more resistent to refactoring changes.
{% highlight ruby %}
class TestClass
  # bad
  def TestClass.some_method
    # body omitted
  end

  # good
  def self.some_other_method
    # body omitted
  end
end
{% endhighlight %}
* Add "global" methods to Kernel (if you have to) and make them private.
* Avoid **alias** when **alias_method** will do.
* Use **OptionParser** for parsing complex command line options and
  **ruby -s** for trivial command line options.
* Write for Ruby 1.9. Don't use legacy Ruby 1.8 constructs.
    * use the new JavaScript literal hash syntax
    * use the new lambda syntax
    * methods like **inject** now accept method names as arguments - `[1, 2, 3].inject(:+)`
* Avoid needless metaprogramming.

# Design

* Code in a functional way, avoid mutation when it makes sense.
* Do not mutate arguments unless that is the purpose of the method.
* Do not mess around in core classes when writing libraries. (do not monkey patch them)
* Do not program defensively. See this [article](http://www.erlang.se/doc/programming_rules.shtml#HDR11.) for more details.
* Keep the code simple (subjective, but still...). Each method should have a single well-defined responsibility.
* Avoid more than 3 levels of block nesting.
* Don't overdesign. Overly complex solutions tend to be brittle and hard to maintain.
* Don't underdesign. A solution to a problem should be as simple as
possible... but it should not be simpler than that. Poor initial design
can lead to a lot of problems in the future.
* Be consistent. In an ideal world - be consistent with the points listed here in this guidelines.
* Use common sense.

## Overture

I've created a [GitHub project](https://github.com/bbatsov/ruby-style-guide), so that everyone can easily participate
in improving the quality of the guidelines. I hope that together we
could make a resource that will benefit the entire Ruby community. I
have a couple more guides like this one in the pipeline - one for
Rails and for testing Ruby and Rails applications.

If you're an Emacs user - the [Emacs Prelude](https://github.com/bbatsov/emacs-prelude) features
customizations that enforce some of the stuff outlined in the guide -
encoding, indentation, line length and lots of other goodies
(whitespace cleanup, ri integration, autopair for quotes, brackets,
etc, Ruby snippets, irb integration, git integration, haml, sass and
coffeescript support out of the box...). You
might want to check it out.

**P.S. The version of the Ruby style guide here is out of date. Please,
  refer to the
  [GitHub project](https://github.com/bbatsov/ruby-style-guide) for
  the latest and greatest version.**
