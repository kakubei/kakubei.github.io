---
layout: post
title: Using a Tap Gesture That Doesn't Interfere with a TableView in iOS
---

So if you've ever used a `Tap Gesture` on a whole `ViewController` only to find that it interferes with a `TableView` and now you can't select any rows, you know how it can be a pain.

The good news is that there is a really simple way to fix it. You don't need to create any weird invisble views, etc.

Just add this bit of code in `viewDidLoad`:

{% highlight swift %}
tap.cancelsTouchesInView = false
{% endhighlight %}

Where `tap` is a `UIGestureRecognizer` object. You can create one in code like this:

{% highlight swift %}
let tap = UITapGestureRecognizer(target: self, action: "closeKeyboard")
view.addGestureRecognizer(tap)
{% endhighlight %}

## Bonus

And a simple way to close the keyboard, as in the above `selector` is this:

{% highlight swift %}
func closeKeyboard() {
    view.endEditing(true)
}
{% endhighlight %}
