---
layout: post
title: Ajax and Rails, making it work 
---


Every time I need to add some Ajax to a Rails project I need to look up documentation and search through all my earlier files to see how I did it. Maybe this is because I’m old and was doing javascript (which I hate BTW) the old-fashioned way so getting my head around the Rails way might be more difficult, I don’t know, but it’s a pain in the arse, so I’m writing a little blog post so I can refer to it and hopefully it’ll help other people too. 

## How it works
To me, Ajax in Rails is needlessly convoluted, but here’s how it works:

1. You create a link with `link_to` and set the `remote` option to `true`
2. In your controller, for that link, create a `format.js` response
3. Create a javascript file with the same name as the method the link responds to
4. In that javascript file, put the `js` code you want Rails to execute when you click the link

### Example
This is a simple issue where I need to hide or show a column based on the value of a cookie which I’ve previously put into an instance variable:

The link

{% highlight ruby %}
# 1
= link_to "<span class='btn btn-sm btn-primary'>eye-icon</span>".html_safe, users_flip_show_dogtag_path, title: icon_title, remote: true
{% endhighlight %}

The controller method

{% highlight ruby %}
# 2
def flip_show_dogtag
   if cookies[:show_library_dogtags].to_i == 1
     set_dogtag_to = '0'
   else
     set_dogtag_to = '1'
   end
   cookies.permanent[:show_library_dogtags] = set_dogtag_to
   @dogtag_show_value = set_dogtag_to # we'll use this in the views
 
   respond_to do |format|
     format.js
   end
 end
{% endhighlight %}

The javascript file

{% highlight javascript %}
// 3
if ("<%=j @dogtag_show_value %>" == '1') {
    $('.dogTag').show();
 } else {
    $('.dogTag').hide();
 }
 // this last line simply refreshes the header with the icons which also change based on the cookie state
 $('.collection-flip-switch').html("<%=j render 'layouts/flip_collection_state' %>");
{% endhighlight %}

## Pitfalls
Beware of comparing integer values inside `js.erb` files, you might get a **missing method error** when the javascript helper tries to run `gsub` on the integer:

{% highlight ruby %}
    NoMethodError - undefined method 'gsub' for 0:Fixnum:
       actionpack (4.0.4) lib/action_view/helpers/javascript_helper.rb:27:in 'escape_javascript'
{% endhighlight %}       

This sucks because it means you can’t use integers for anything you need to then equate in a `js.erb` file but I don’t know of a workaround.

## Reloading a DOM element
If you want to reload part of the page, it gets even more convoluted because you need to create a Rails template that you can call to render only that portion. Rails calls this the ‘unobtrusive’ way, but it really fragments your views and you end up with as much as 5 files to edit just to get this to work, let’s count them: 

1. the `routes` file, you’ll need to add a route for the link
2. the `controller` file that the link method calls
3. the `view` file that shows your data
4. the `js.erb` (or `coffescript`) file that contains the actual javascript code
5. the `Rails partial` file that you want to refresh

Not what I would call unobtrusive at all but there you go. 

So in our earlier example, lets refresh part of the page.

### Refreshing part of the page
All of the above stays the same, but we need 2 things to make the refresh happen:

1. an `element` that we will refresh
2. the `Rails partial` that will contain the data that we will *reload*

In our view file we create an element (generally a div) that we will refresh, it might look like this:

{% highlight ruby %}
# 1
.collection-flip-switch
  = render 'layouts/flip_collection_state'
{% endhighlight %}

The Rails partial

{% highlight ruby %}
- if cookies[:show_library_dogtags].to_i == 1
   - dogtag_icon = "<i class='icon-eye-open'></i>"
 -else
   - dogtag_icon = "<i class='icon-eye-close'></i>"
 
= link_to "<span class='btn btn-sm btn-primary'>#{dogtag_icon}</span>".html_safe, users_flip_show_dogtag_path, title: icon_title, remote: true
{% endhighlight %}

The bit in the `js.erb` that actually refreshes the DOM element:

{% highlight javascript %}
$('.collection-flip-switch').html("<%=j render 'layouts/flip_collection_state' %>");
{% endhighlight %}

In this case the partial only contains a link and the logic to represent the right icon depending on a cookie value (I’ve stripped some other code that also gets refreshed to keep things simple).

## Conclusion
So, not very unobtrusive at all and too many moving parts to remember every time. 

Feel free to refer to this nifty guide the next time you need to use Ajax in Rails, I know I will :)
