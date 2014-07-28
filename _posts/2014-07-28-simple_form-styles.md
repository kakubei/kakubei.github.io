---
layout: post
title: 'Remove div wrappers from simple_form fields'
---

The `simple_form` gem, by default will wrap all your input fields in different div classes. This is fine to get up and running out of the box, but it can be a real pain when you want to style your inputs in ways that conflict with the default CSS.

To get rid of the formatting is very simple, use `f.input_field` instead of `f.input`

{% highlight ruby %}
= f.input_field :email, label: false, placeholder: 'email'
{% endhighlight %}

Then you can style them as you wish.

I discovered this recently when I wanted to put a small icon next to the input field, by default it would not allow the icon to be flush with the field, it would actually sit above it.

If using Bootstrap, with this styling you can add a small icon next to the field:

{% highlight haml %}
%tr
  %td.input-prepend
    %span.add-on
      %i.icon-envelope
    = f.input_field :email, label: false, placeholder: 'email'
{% endhighlight %}


This produces the following HTML code:

{% highlight html %}
<tr>
    <td class="input-prepend">
        <span class="add-on">
        <i class="icon-envelope"></i>
        </span>
        <input class="string email required" id="user_email" \
        label="false" maxlength="50" name="user[email]" \
        pattern="^[^@][\w.-]+@[\w.-]+[.][a-z]{2,4}$" \
        placeholder="email" size="50" type="text">
    </td>
</tr>
{% endhighlight %}


If you simply want to change the wrappers, edit `app/initializers/simple_form_bootstrap.rb`. Or `simple_form.rb` in the same location if you're not using Bootstrap.
