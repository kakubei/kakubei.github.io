---
layout: post
title: "Blogging Software"
tags:
---

So you want to have a blog (who doesn't, right?) and you're wondering which software you should use. Should you use an existing web site or roll your own? Perhaps a CMS (Content Management System) so you can go hog-crazy and add all sorts of stuff to your blog, videos, photos, plugins, cats, etc.

Well, I'm here to help you. I've had my blog for a while in [blogger](http://blogger.com) and it's fine, but recently I wanted to create another blog to post the chapters of a novel I'm working on to put it out there and maybe get some feedback, you can check it out at [novel.kakuweb.com](http://novel.kakuweb.com). So I started hunting around for the best way to do so.

I looked at all the major blogging software out there and tried most of them, some I won't even mention here, what's the point of going on and on about something I didn't like? I'll just talk about the top 3.

## TL;DR
The `Too Long, Didn't Read` version is:

* if you know nothing about coding and don't care to learn, go with [blogger](blogger.com) easy and quick to set up.
* if you love Markdown and don't mind getting your hands a bit dirty, use [Poole](https://github.com/poole/poole) or more specifically Poole with the [Lanyon](https://github.com/poole/lanyon) theme (read the rest of the post to know what this means).

## The long version
Why not blogger then? Easy: blogger is a bit limited in your options, and the worst sin of all: it doesn't support [Markdown](http://whatismarkdown.com), plus the code hightlighting is crap, it just supports the `<code>` or `<pre>` HTML tags, so I needed an alternative.

### Wordpress
Of course, the first thing I tried was [Wordpress](http://wordpress.org), the king of blog software out there, there must be more Wordpress blogs in the world than all of the other options combined.

Wordpress has a wealth of options and plugins and themes and what not, it's fine, go ahead and use it, but it has some things I hate:

#### Complexity
* You need a Mysql database and user to be able to use Wordpress, this in itself can be very daunting to some people and means your web hosting company needs to support it. Of course, Mysql support is fairly common these days for hosting companies.
* It runs on PHP, if you want to make a change not covered by your theme, you need to know PHP and find a hosting company that supports it. Like Mysql, hosting companies with support for PHP are a-plenty.
* It stores everything in a database, moving you Wordpress blog can be pretty hard depending on your level of expertise.
* Restyling themes and templates is not trivial and can be quite hard, I just finished restyling a whole website and the blog was a bitch.

#### PHP
I've worked with PHP for years, on and off for about 12 years, it's an OK language, very quick in execution but after switching to [Ruby](https://www.ruby-lang.org/en/) I now hate it. Can't stand it, don't want to ever see it again, let alone work with it. Wordpress is written entirely in PHP.

#### Themes
Wordpress has millions of themes, yet finding the right one is hard, and editing one that you mostly like is a pain in the arse.

### Other PHP-based blogs
There are a ton out there, each purporting to be the next best thing since sliced bread. I didn't try them for all the same reasons outlined with Wordpress.

### Ghost
[Ghost](https://ghost.org) is the new kid on the block, the latest blogging software and is quite _en vogue_ right now. It's based on [Node.js](http://nodejs.org) and is open source.

I tried it, I liked it, simple, pretty, easy to use, no user setup since it relies on SQLite, but it has some issues:

* SQLite is not supported on [Heroku](https://www.heroku.com/about) where I wanted to host the blog.
* You can pay as little as $5 a month and have your blog on ghost.org, that's great, but I'm already paying monthly for my website hosting, didn't want to pay twice.

Ok, so host it on your regular website, what's the problem? I can't, the problem is Ghost is written in [Node.js](http://nodejs.org) and my current web host provider, Site5.com, does not support Node.js for shared accounts, meaning you need a private server all to yourself, forget it.

Also, there aren't a ton of customization options, so if that's your thing, look elsewhere. If not, and you don't mind paying $5 a month for your blog or can find a provider that supports Node.js and want to install it yourself, it's a good option.

### Jekyll
[Jekyll](https://www.heroku.com/about) creates static pages, no database, no users, no PHP, no Chemical Brothers, just your blog pages, and it's based around Markdown, you can write all your pages in Markdown and nothing else, great. It's actually a Ruby gem but you don't have to know Ruby or do any programming to get it going, you just install it with:

{% highlight ruby %}
	gem install jekyll
	jekyll new my-awesome-site
	cd my-awesome-site
	jekyll serve --watch
	# => Now browse to http://localhost:4000
{% endhighlight %}

And you have an instant blog. All you need to do is edit the pages inside `_posts` and Jekyll will refresh everything automatically (provided you used the `--watch` flag when you started the server). The best part, to publish your site, the easiest thing to do is copy everything inside the `_site` folder over to your server and that's it, no configuration of any kind, no database, no weird languages, etc. your pages are live. Name your posts with the date and name and Jekyll will pick it up automatically from the name, for example:

    2014-04-05-the-best-blogging-software-ever

It comes with a lot more bells and whistles and themes and things and you can publish to different places, including [github](https://github.com) for free , but it's great that you can create a blog so easily and quickly and with so little pain. In fact, I had my blog being updated just by pushing to my remote repository with git, how awesome is that?

However, Jekyll is pretty bare-bones, you'll want to grab the [Lanyon](https://github.com/poole/lanyon) variant of Jekyll (technically Lanyon is a variant of Poole which is Jekyll with some added files), it comes complete with a nice minimalistic theme that puts text first and foremost.

Make sure you installed Jekyll with `gem install jekyll`, download Lanyon to your machine, open a terminal, `cd` over to that folder and type:

{% highlight ruby %}
	jekyll serve --watch
{% endhighlight %}

Boom, brand new, awesome blog!

I love this thing, it's really wonderful the lack of complexity and hassle, you just type your posts in Markdown and push your remote repo and you're done! Nothing can beat that. If you don't do git, no worries, just FTP the content of the `_site` directory over to your site, you'll still end up with a beautiful blog with minimum hassle.

It even has support for drafts, just create a regular `_drafts` folder in the root of your blog and create your drafts there, just don't put the date in the name since you don't know when you're going to publish them, and you can keep working on them until they're ready.

It comes with a templating engine (yeah, I know, *yet another one???*) called `liquid` which is pretty easy to use and allows you to customize your site in a jiffy.

I was so excited by this that I initially had it publishing to my github pages simply because it was so easy to do, I was in heaven. However, for a novel, probably putting the whole manuscript up on github is not such a great idea, all it takes is one bastard to download the whole thing with one click and then sell it as their own, leaving me destitute. So it was with a broken heart that I decided not to publish the novel to github (oh the pain) but rather to my own server where it's a bit harder to download all the chapters. I mean, sure, anyone can copy/paste but there is no link to download the whole thing with one click as there is on github.

So the conclusion is that Lanyon is your best bet, get it, use it, love it.

But wait, what about [Jekyllbootstrap](http://jekyllbootstrap.com) or [Ruhoh](http://ruhoh.com) which are both based on Jekyll?

They're both valid options but I even though Jekyllbootstrap adds a number of options to get you up and running, it's more complex than Lanyon and I found it poorly documented, plus the guy who wrote it is no longer maintaining it, he moved over to Ruhoh which is even more complex and also doesn't have great documentation. I'm not saying don't use them, or that they're too hard to figure out but I'm saying that with the amount of time I had to dedicate to this project, Lanyon hit just the right balance, not as bare-bones as Jekyll yet not as complex as to the others, the sweet spot.

If you use Rails and want to incorporate Jekyll, [here's a good article](http://www.homemarks.com/blog/2014-04-19-jekyll-style-blogging-on-rails) on how to do that.

---

Thanks to [Joshua Lande](http://joshualande.com/jekyll-github-pages-poole/) for pointing me to this variation of the [Jekyll](http://jekyllrb.com) blog software.
