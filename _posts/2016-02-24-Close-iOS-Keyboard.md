---
layout: post
title: Best Way to Close the iOS Keyboard
---

I'm sure you've seen lots of different ways of closing the keyboard in iOS. One of the most popular ones seems to be to resign the first responder. And that one's fine. But the one I like the most and I think is the simplest is to just force an end editing like so:

{% highlight swift %}
view.endEditing(true)
{% endhighlight %}

Put it into a function and call it from multiple places in your `ViewController`:

{% highlight swift %}
func closeKeyboard() {
    view.endEditing(true)
}
{% endhighlight %}

Maybe call it after a tap anywhere in your main view:

{% highlight swift %}
let tap = UITapGestureRecognizer(target: self, action: "closeKeyboard")
view.addGestureRecognizer(tap)
{% endhighlight %}
