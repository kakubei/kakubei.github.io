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

## Storing hashes in a file
If you ever need to store data in a file and then retrieve it, consider using PStore, a standard Ruby library specifically for this purpose. I found the documentation not terribly clear, but basically here's how it works:

We first need a PStore object to work with.

{% highlight ruby %}
    store = PStore.new(path_to_file)
{% endhighlight %}

This just creates the object, we haven't created a file or written to it yet. let's create some hashes:

{% highlight ruby %}
    store.transaction do
        store[:date] = Time.now
        store[:name] = 'Cowboy Ninja Viking'
        store[:real_name] = 'Chad'
        store.commit
    end
{% endhighlight %}

**This last line is very important**, without it the system will not write anything to file. the docs say you can alternatively just use end without the commit and the file will be written, but that did not work for me.

### To read the stored hashes

{% highlight ruby %}
    store.transaction(true) do
      store.roots.each do |data_root_name|
        p data_root_name
      end
    end
{% endhighlight %}

This is an easy way to store data in hashes, and who doesn't love hashes?
