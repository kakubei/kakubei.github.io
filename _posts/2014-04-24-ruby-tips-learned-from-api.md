---
layout: post
title: Ruby Tips I learned While Building an API From Scratch
---

## Some of the lessons I learned while writing our new API from scratch in Ruby and Padrino

## Part 1

### Find unique values inside an array of hashes based on a value
Say you have an array of hashes and want to remove duplicates based on 1 key, that's easy you do:

{% highlight ruby %}
    array.uniq { |e| e[:key_name] }
{% endhighlight %}

For example, consider the following array of hashes:

{% highlight ruby %}
array = [{:name=>"Yes, Yes, Yes", :artist=>"Some Dude", :duration=>"3:21"},
{:name=>"Chick on the Side", :artist=>"Another Dude"},
{:name=>"Luv Is"},
{:name=>"Yes, Yes, Yes", :artist=>"Some Dude"},
{:name=>"Chick on the Side", :artist=>"Another Dude", :duration=>"2:20"},
{:name=>"new one", :composer=>"compo"}]
{% endhighlight %}

These are tracks in an album, but the array could be of anything. So if we wanted to remove any duplicate tracks by name, we'd do:

{% highlight ruby %}
    array.uniq { |e| e[:name] }
{% endhighlight %}

This would return only unique entries based on the `:name` value.

But what happens when you need to determine uniqueness by more than one key? No problem, `uniq` can take a block as you can see in the above example, the trick is knowing how to write that block.

In our case, we want to keep uniq values based on name, artist and composer so we would do this:

{% highlight ruby %}
    array.uniq { |a_track| [ a_track[:name], a_track[:artist], a_track[:composer] ] }
{% endhighlight %}

It's the same as the first example but using more than one criteria, nice!

Continuing with hashes, what if you need to store hashes in a file for some reason? Maybe you're running some tests and want a quick way to store the results, or maybe you need to store some data in an ordered manner but don't want to overhead of a database. Whatever the case, there is an easy way to store hashes in a file.

## [Storing hashes in a file](/2014/06/16/store-hashes-in-file)
