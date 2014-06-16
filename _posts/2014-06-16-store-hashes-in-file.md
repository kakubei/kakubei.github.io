---
layout: post
title: Storing hashes in a file
---

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

### Use YAML instead of Marshal to store those hashes
But wait, there's more! Once you get the hang of this hash-storing trick, switch to using `YAML::Store` instead of `PStore`. The reason is simple: `yaml` files are much more readable than `PStore` files which are not readable at all by humans.

Yaml::Store uses [yaml](http://ruby-doc.org/stdlib-2.0.0/libdoc/yaml/rdoc/YAML.html) to write the data whereas PStore uses [Marshal](http://www.ruby-doc.org/core-2.1.1/Marshal.html).

Learn more about [YAML::Store here](http://ruby-doc.org/stdlib-2.0.0/libdoc/yaml/rdoc/YAML/Store.html), but basically you can keep everything pretty much the same just:

{% highlight ruby %}
    require 'yaml/store'
    store = YAML::Store.new(path_to_file) # instead of PStore
{% endhighlight %}
