---
layout: post
title: "Serving Retina Images to Devices that Need them"
tags:
 -
---

Retina Macs are a pain in the arse for web developers, now we have to deal with 2 sets of images for everything or run the risk of suffering the unending wrath of Retina Mac users who will invariably let you know that image X looks fuzzy on their screen. When did fuzzy images become a capital crime? And it's not like I'm talking horrible, pixelated images, no, just a bit fuzzy which all of a sudden is completely unacceptable.

This is an issue that affects mainly the Macs because on the iPad and iPhone, even with Retina displays, the screen size and pixel density are such that a non-retina image is a lot less noticeable.

It would be interesting to figure out what percentage of our web viewers actually have a retina display Mac before we go and spend hours trying to cater to a small minority.

So what's a web developer to do?

## Option 1
Just serve huge images to everybody. This is the easiest thing to do, but you still need some work for things to look good on the bloody retina displays, you need to set your image width and height to half the actual size or it will still look (oh no!) *fuzzy*.

Here's an example, say your image is 1024 x 960 pixels, this is how you need to present it.

{% highlight html %}
<img src="retina-image@2x.png" width="512" height="480">
// with CSS
<img src="retina-image@2x.png" style="width: 512px; height: 480px;">
{% endhighlight %}

Of course you want to avoid HTML attributes and inline CSS so you can create a class for it.

{% highlight html %}
<img src="retina-image@2x.png" class="retina-pain-in-the-arse">
// CSS
.retina-pain-in-the-arse {
    width: 50%;
    // You'll need some extra, tricky CSS to get the image to display at the size you want.
}
{% endhighlight %}

The downside to this method is, of course, you're serving huge images, even to the majority who don't have a fancy new retina display.

And the `width: 50%` in the CSS won't help you get the image at the size you want, that's a bit tricker and you might have to do it on an individual basis which is bad news all around.


## Option 2
Use Safari's fancy new-fangled `srcset` tag to serve retina images. It sounds great. Supposedly webkit has this new tag where you specify the name of the retina image and the browser will only load the needed one. It's supposed to be like this:

{% highlight html %}
<img src="normal-image.png" srcset="big-ass-retina-image.png 2x">
{% endhighlight %}

Note the `2x` at the end of the name, no, that's not a typo, that's what the spec says to use. Safari is supposed to look at this and determine which image it will serve depending on the device. Sounds awesome. The best part: it will **not** load both images, just the one it needs to render, everybody wins.

**The bad news:** this doesn't seem to work, that is, there is no support on the main browsers for it at the moment, which is a bit strange considering that this change came out for webkit in August 2013. Safari 7 in Mavericks was supposed to have it but it doesn't, nor does Safari for iOS 7.1 either. I read somewhere that the latest version of Chrome (or Chrmomium, the poster wasn't too sure) supports this but in my tests with Chrome 33.0.1750.152 downloaded yesterday it doesn't work.

So, useless for the time being, keep it around for the future if you already changed your web pages.

Read the blog post [straight from the horses mouth](https://www.webkit.org/blog/2910/improved-support-for-high-resolution-displays-with-the-srcset-image-attribute/).


## Option 3
Use Javascript to serve the retina images only for retina devices. This seems to be the best option so far (see option 2 as to why that is). We use a little javascript to overwrite the image source tag if we detect a retina device.

This is a javascript I stole from my brother who in turn stole it from some dude on the internet. I had to change it to actually make it work because, as usual, the code he sent me was full of errors.

Presented here in Coffeescript because I hate Javascript (is there an uglier modern language that javascript out there?):

{% highlight html %}
    $ ->
      if window.devicePixelRatio > 1
        images = document.getElementsByTagName("img")
        arrayLength = images.length
        i = 0

        while i < arrayLength
          imgTag = images[i]
          attr = imgTag.getAttribute("srcset")
          if attr
            firstToSpace = attr.split(" ")[0]
            console.log("attr is: " + attr)
            imgTag.src = firstToSpace
            console.log("And firstToSpace is: " + firstToSpace)
          i++
    return
{% endhighlight %}

Alright, for you Javascript perverts:

    $(function() {
      var arrayLength, attr, firstToSpace, i, images, imgTag, _results;
      if (window.devicePixelRatio > 1) {
        images = document.getElementsByTagName("img");
        arrayLength = images.length;
        i = 0;
        _results = [];
        while (i < arrayLength) {
          imgTag = images[i];
          attr = imgTag.getAttribute("srcset");
          if (attr) {
            firstToSpace = attr.split(" ")[0];
            console.log("attr is: " + attr);
            imgTag.src = firstToSpace;
            console.log("And firstToSpace is: " + firstToSpace);
          }
          _results.push(i++);
        }
        return _results;
      }
    });

    return;

I've left the console logs in there so you can see if the damn thing is working or not.

What it does is very simple:

Since we had already used the new-fangled, useless `srcset` tag, we leverage that to overwrite the image source, it works like this:



1. Checks to see if the device is a retina device based on Pixel Ratio being greater than 1
2. then picks up all the images in the page
2. checks to see if they have the `srcset` attribute
3. if they do have the attribute, it takes the image name from that attribute,
4. removes the space and `2x` at the end of the name, hence grabbing the name of the high-res image
5. and overwrites the `src` attribute with this new, high-res image.

Simple. The nice thing about this script is that it's short, simple and you can leave the `srcset` in there for the future when the major browsers start supporting it, at which time you simply remove the call for the javascript and things should work (fingers crossed).

What if you don't want to use the damn `srcset`? Or if you don't, rightly, trust my Javascript skills?

Then your best bet is probably to use javascript written by someone who actually knows javascript, like this one right here: [retina.js](http://retinajs.com/) which seems to be the most popular out there.

Downsides to both this script and the retina.js one, a couple:

1. The browser will load **both** images, making your pages take longer to load, **even if you're not serving the retina images**, this sucks!
2. Pages will be slower on **all** devices, this is especially bad on mobile devices which have less capable CPUs and already takes them longer to render images and pages.

However, this is the best option I've found so far to serve retina images. If you know of something better, by all means, get in touch and let us know.

Cheers.
