---
layout: post
title: "Checking Variable Existence in Ruby"
tags:
 -
---

For such a wonderful language, Ruby does seem to have too many different ways of asserting whether a variable or method exists and things can get a little confusing. Following are the different ways I've found that work most of the time if used correctly. By used correctly I mean you have to make sure you understand what the object is you're working with, is it a variable? an instance variable? an instance of a class perhaps? Because not all objects respond to these different methods.

Check for a non-existent variable


If you want to know if the variable has been defined, then use defined?:

    defined? var_name
    # nil or true

For when you need an assertion if this var does not exist then…

    defined?(var) == nil

Note: `nil?` will only work with existing variables or objects, it will throw an error with non-existent ones.

You can use `.nil?` to check for existing hash or array keys:

    params = { id: 1, title: 'Some crap' }
    params[:iphone].nil? # -> true

`.nil?` will also work with instance variables:

    @var.nil? # -> true
    Check for method in an object

This should work for any method on any object, but for example, does an instance of a Model object (representing a table in your database) have a particular field?

    instance_object.respond_to?(:field_name)
    user.respond_to?(:locked)
    # returns true or false

This is great for helpers that might respond to different models and some might not have a particular method or field on them. This way, you avoid the dreaded undefined method error.

But this won't work if you want to find out if there is a particular method in a Class, for that you'll need `method_defined?`:

    Dvd.method_defined? :credits
    # true or false

Check for empty or nil variable

In Ruby these 2 things are not the same. So you have to check for them both, there doesn’t seem to be one method that does both at the same time unless you are using ActiveRecord.

If using ActiveRecord, then use the `blank?` method.

    unless @game.features.blank?

Note: This will not work in straightup Ruby! In Ruby (without ActiveRecord), use:

    variable.empty?

Note that the variable must exist before you can check whether it's empty or not, I know this sounds logical, but if you come from other languages (like PHP) you might be surprised by this. If the variable doesn't exist you'll get an undefined variable or method error.
