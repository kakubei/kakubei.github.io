---
layout: post
title: "HTML Frameworks and Preprocessors to Love"
---

If you've done any web programming lately you'll no doubt be aware of the increasing trend in HTML frameworks and preprocessors. Frameworks seem to be all the rage these days, from the behemoths that are Bootstrap and Foundation to dainty little grids like Zen Grids or 960 grid. Granted some are complete frameworks and others are simply CSS grids, but I'll call them frameworks here for simplicity. There is also [Compass](http://compass-style.org/), a Ruby gem that will really help you deal with CSS.

Then there are the preprocessors, things like [Less](http://lesscss.org/) and [Sass](http://sass-lang.com/). These 2 are extension languages for CSS to help you deal with the mess and mediocrity of CSS (CSS sucks, let's get this out of the way right now. It's ok for styling a very simple document, but for formatting it, it's just shite).

Lastly there are the libraries like [JQuery](http://jquery.com/) (a library for Javascript), with their own API to make your life easier.

All these things are good, they *do* make your life easier, but you have to know how to use them, and which ones to use in which case. They also take some investment in time and energy to understand them and this can sometimes be frustrating, especially when you're in the middle of a production cycle.

So I'm here to dispel myths and shed some light into this whole thing. After <strike>fighting</strike> playing with a number of these technologies, I think I've gotten to a point where I feel comfortable holding forth about them. 

Seriously though, I do hope this helps someone because it's very easy to get lost in all of this.

## Frameworks
The most popular frameworks out there are [Twitter Bootstrap](http://getbootstrap.com/) and [Zurb's Foundation](http://foundation.zurb.com/), arguably followed by [Gumby](http://gumbyframework.com/), [Sekeleton](http://www.getskeleton.com/) and a couple of others. Both Bootstrap and Zurb are pretty big and opinionated, they will change **a lot** of your HTML and even come with Javascript (JQuery) components. This is not a bad thing, it's easy to get a site up and running with them and they take some of the pain of formatting web sites away. I've used Bootstrap for two major projects and was pretty happy with it since neither of them required pixel-perfect placement and I was OK using the theme's default after some tweaking. By all means, use them, they are pretty handy.

Things start falling apart with them when you need to edit specific styles or are not happy with a particular behaviour or you're very demanding about the look and flow of your site. Then they can be a nightmare because you have to debug the myriad CSS styles to see which one is affecting the element in question. They are also pretty big and do add some overhead to your pages.

Another thing I don't really like about them is the necessity to modify all your HTML to be able to use them. It's a standard of adding CSS classes to your elements, nothing weird there, but once you have a site created for example, it's a pain to go in and start adding classes all over the place:

{% highlight css %}
	<section class='col-md-8 col-sm-2 pull_right'>
{% endhighlight %}

or something like that.

I believe a better alternative is to do most of your edits in the actual CSS itself. This is where frameworks like Compass really shine, yes it's a [Ruby](https://www.ruby-lang.org/en/) [gem](http://rubygems.org/), but before you click away in search of Internet cats, you need to know that you can use it with an HTML-only project, you don't need to use or know anything about Ruby, you just need Ruby to install the files and away you go. 

Another Ruby gem that is a great help is [Zen Grids](http://zengrids.com/), don't be fooled by the ugly website, it's quite nice and very easy to use. In fact, for the simplest workflow where you don't have to learn any Compass, configurations, etc. just install Zen Grids and use that for your next web project. It will go a long way towards keeping you and your CSS sane.

For the last non-Rails site I created, I used Sass, Zen Grids and lots and lots of elbow grease and am very happy with the results. You can check out the site here: [bruji.com](http://bruji.com). It was also a complete redesign but that's a different story for another post.

I also believe that if you are going to do web programming you need to know how to edit and fight with CSS directly, without the aid of a framework so that you can solve issues that will undoubtedly arise. Having said that, though, I don't think fighting CSS should be 80% of the web creation process and that's what it seems to be in my experience. We definitely need a better alternative to CSS, that's why these frameworks are so popular, if CSS were good at its job we wouldn't need them. I know it's a sort of chicken and egg thing: the web and browsers were not created for the use they are getting right now, but things have evolved rapidly and tools and languages should evolve along with them.
