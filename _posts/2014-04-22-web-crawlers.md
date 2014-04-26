---
layout: post
title: "Web Crawlers"
---

Kimonolabs is the best way to crawl the web. Period. I've tested a lot of different apps and gems, etc and [Kimonolabs](http://www.kimonolabs.com) is by far the best and easiest way to crawl it. Forget competitors like [import.io](http://import.io), it's clunky, bug-ridden and confusing.

Not only is Kimonolabs' API creation simple and effective, but it's also a pleasure to use. A lot of time and effort has gone not only into the technical side of things but also the design. Everything is clearly delineated and easy to see. It does take a little bit to get used to the process of identifying data on a web page, but once you do you'll find it very intuitive and easy.

It's also very flexible, it will output the data in JSON, CSV or RSS formats. Kimonolabs offers a link to your data point on their servers so you don't have to open up your site to any exploitable ports. But if you *do* want to have Kimonolabs send the data to your site that is also available via web hooks. I've tested both methods and they worked fine and were very easy to set up. It also offers scheduled and on demand crawling so you're not hitting a website a thousand times a day and possibly getting into trouble for it.

One of the best features is what they call [Targeted Crawling](http://www.kimonolabs.com/learn/targeted-crawling). This is a system where you can create 2 APIs, one for general crawling and another for details crawling that will actually crawl the pages your general API has discovered, you even specify which link you'd like the details API to follow. This is fantastic because it means you can set up both APIs with Kimonolabs and then simply retrieve the details from the details API or both if you need to, but it makes web crawling *sooooo* much easier. It also shows that they've put a lot of thought into what people use web crawling for since this is a very typical scenario but to have it built into their systems is just icing on the cake.

And the good news doesn't stop there, their support is also excellent. I got in touch with them because I was having some issues trying to pick up data on a particular web page and not only did they offer great advice but a couple of days later updated their code to more easily capture similar data.

They've also been nice enough to offer guidance on accessing your data using Ruby, Python, PHP, Node, jQuery and Curl. All this data is available for each API you create, very handy.

I really encourage anyone who needs to do any type of web crawling to [check them out](http://www.kimonolabs.com/). They have a free plan that allows plenty of experimentation. You'll never look at web crawling the same way again.

If you can't afford Kimonolabs very fairly priced plans then you'll have to crawl manually which is a huge pain. But if you're using Ruby, I will be posting another entry on how to crawl with Ruby real soon, so make sure to subscribe so you don't miss it. 