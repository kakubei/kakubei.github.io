---
layout: post
title: Swift is Actually Cool!
---

Who knew?

I've written [disparagingly about Swift in the past](http://kakubei.github.io/2014/11/15/swift-is-a-mess/), but as I dig deeper and deeper into the language, and with the new 2.0 version just out, I've come to like it more and more.

## Closures

Something that is really cool are closures. Closures are like mini functions that get passed as a parameter to another function. Yeah, sounds weird, I know but they're great. They're called `lambdas` in Ruby and you use them all the time without even realizing what the hell they are. They're called `blocks `in other languages like Objective-C.

Does this look familiar?

{% highlight ruby %}
%w[Tony Janis Sasha Joe].map { |e| puts "#{e} is my name" }
{% endhighlight %}

This will print out:

    Tony is my name
    Janis is my name
    Sasha is my name
    Joe is my name

The `.map()` method is a great example because it actually takes a closure as its sole parameter. I bet you didn't know that. I didn't. I've been using `.map` for years in Ruby without knowing that what was inside of it was a lambda (closure). Studying the Swift guide has given me a much better appreciation for closures and how cool they are.

Take this bit of simple code for example:

{% highlight swift %}
let numbers = [2, 12, 7, 5]
var multipliedNumbers: [Int] = []
for number in numbers {
    multipliedNumbers.append(number * 2)
}

multipliedNumbers
{% endhighlight %}

Which will return `[4, 24, 14, 10]`

This can be written in a single line of code with a closure like so:

{% highlight swift %}
let multipliedNumbers = numbers.map({ $0 * 2 })
{% endhighlight %}

`[4, 24, 14, 10]`

Isn't that awesome?

But what's going on there? It's not as crazy as it looks.

* `.map()` takes an array and acts upon each item in the array, then it returns a new array with whatever changes we've made inside the `.map()` parentheses
* The `$0` refers to the first argument that is passed, which in the case of `.map` will be each item in the array, so for each item it's doing something
* That something is simply multiplying the item by 2

The verbose way of writing that closure is like this:

{% highlight swift %}
let multipliedNumbers = numbers.map({ (number: Int)-> Int in number * 2 })
{% endhighlight %}

So you see, a closure is like a function:

* it has parameters, in this case a parameter named `number` of type `Int`
* it has a return value, another `Int` (`-> Int` is how Swift declares return types)
* and then, instead of curly braces for the body of the function, it has the `in` keyword
* then the actual body, what we want this closure to do

Since Swift is pretty good at inferring stuff, we can let it infer the parameter type, and the return type.

If we were to only infer the return type, it would look like this:

{% highlight swift %}
let multipliedNumbers = numbers.map({ (number: Int) in number * 2 })
{% endhighlight %}

But we let it infer both parameter type and return type and we use the `$0` trick to grab the first parameter without having to declare it or give it a name. The result, as we saw above is this:

{% highlight swift %}
let multipliedNumbers = numbers.map({ $0 * 2 })
{% endhighlight %}

Perhaps a slightlly more readable version might be this:
{% highlight swift %}
let multipliedNumbers = numbers.map { number in number * 2 }
{% endhighlight %}

In this case, it is much more apparent what the first (and only) parameter is and what is happening to it. It still does the same thing as the line above, but I think this is more readable, and readable code will always triumph over shorter, more cryptic code.

Oh, and notice that we can also omit the parentheses after the `.map` method and just include the closure after it inside curly braces. This is known as `trailing closures` and is the way Ruby uses them. Looks much nicer.

## UIView Animations
The animations in `UIView` are also a good example of closure usage.

{% highlight swift %}
let myView = UIView()
UIView.animateWithDuration(0.3, animations: { () -> Void in
    myView.frame = CGRectMake(0, 0, 100, 100)
    }) { (completed) -> Void in }
{% endhighlight %}

Here, `animations:` takes a closure that returns nothing (`-> Void`) and we have another closure at the end which is the completion block.

Note that all the magic is happening inside the `animateWithDuration()` method, we are passing closures inside of it.

This can be further optimized by having Swift infer the return type of the `animations:` closure and by removing the completion block since it's not being used, resulting in this:

{% highlight swift %}
let myView = UIView()
UIView.animateWithDuration(0.3, animations: {
    myView.frame = CGRectMake(0, 0, 100, 100)
    }
)
{% endhighlight %}

Much easier to read.


So don't be afraid of closures, they're great, they just take a little getting used to.

Oh, and incidentally, regular functions in Swift are themselves closures! What??? True, look it up.

Learn more at [Bitfountain courses](http://www.bitfountain.io).
