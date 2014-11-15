---
layout: post
title: Swift is a Bit of a Mess
---

Apple’s new language is undoubtedly much more palatable and approachable than the dreaded Objective-C, no question about that whatsoever. Even if you’re a staunch, die-hard, crusty old Objective-C developer you have to admit that Swift is more elegant, less verbose (hell, even a 6-year-old girl is less verbose), easier to remember and, more important: easier to learn than Objective-C. It is Apple’s latest attempt at bringing a *modern* language to its *modern* Operating System.

But it’s no panacea. Granted, this is version 1 but we’re talking about a language that will be used to create the next and great applications for the most popular smart phones and tablets in the world, people! We’re not talking about a language developed by some nerd in his basement because he didn’t like Ruby’s syntax for Regex (I’m looking at you Haskell). This is a language developed (technically adopted) by the most successful computer company in the world! As such, you would expect nothing less than perfection, or at least **full functionality** if perfection is too much to ask. But it seems that even that: functionality is too much to ask of Apple these days. Apparently, gone are the days when processes went through rigorous in-house testing before release, now Apple is content to let their customers do their beta testing for them. Alright, this is true of all the major software companies as well, Microsoft and Adobe do the same thing as does the Behemoth that is Autodesk. But this is Apple, the company that encouraged us to *Think Different*, and for the longest time they actually practiced what they preached.

Alas, those days are over. Not only in hardware when you see the abomination that is the iPhone 6 Plus but also in software when they see fit to release version 1 of a programming language that is definitely not ready for prime time.

## The Evidence
Let’s talk facts instead of talking out of our arses.

### Between the last beta and the 1.0 release (a matter of mere weeks) Swift changed substantially. It changed enough to break a hell of **a lot of code**.

Ok, so it was a beta and I shouldn’t bitch because beta is beta and everyone knows it’s not written in stone, but one of the changes was a huge one, it had to do with *unwrapped variables*, a silly new way to prevent `nil` bugs. Basically, instead of checking for `nil` all the time, you have tell Xcode if your variable will ever return no value.

Sounds easy enough and it also sounds like a laudable goal, anything to make our lives easier and produce less bugs should be celebrated. The problem is both with the implementation of this feature and Apple’s shitty explanation of it. If you come from other languages, this is a completely alien concept that no one has been able to explain properly. Not only that, but you’re supposed to append an exclamation or question mark to your variables depending on whether they’re optional or not. So your code will look like this:

{% highlight objective-c %}
  @IBOutlet weak var taskField: UITextField!
  func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
{% endhighlight %}

Pretty horrible. And it’s not a matter of aesthetics, it’s a matter of usability, if you’re coding along and forget to use this dumb nomenclature all hell will break loose. Sometimes the compiler won’t even warn you and you’ll start getting all sorts of weird errors on runtime. Not good.

The worst part about this is that Apple made this idiotic move simply because they didn’t have time to audit all their crusty Objective-C methods to make sure they wouldn’t explode when nil was returned. You can look it up! In other words, they did this because they released the language too early (as a 1.0 release mind you, not the beta) and couldn’t guarantee it would work properly so they hurriedly added this shitty check. That just sucks. The only good sliver of hope in all of this is that maybe they’ll do away with this crap once they do get their arses in gear and audit all their crusty code.

The fact that Apple made these changes just weeks before the release of 1.0 does not bode well at all, what else did they forget to include in the first release?

I’ll tell you what else:

### Missing methods

Yup, Swift is rife with missing methods. And I’m not talking about obscure methods like `tellMeWhatIsMyFavouriteSouthParkEpisode` (yes, methods in Swift are still verbose, *sigh*), I’m talking about basic methods like date conversions. Date conversions! Date conversion functions are basic stuff, you use them all the time. But Apple didn’t have time, or couldn’t be bothered, to include them into Swift. So what do you do? You use NextStep methods.

What? I thought I was supposed to be learning Swift, the whole point of having waited all this time was because I hate Objective-C and don’t want to learn its convoluted syntax.  Well, though titty, you’ll have to learn some Objective-C after all (*damn*) more on that below. So you have to do ugly stuff like this:

{% highlight objective-c %}
  var gregorianCalendar = NSCalendar(identifier: NSGregorianCalendar)
  var date = gregorianCalendar.dateFromComponents(components)
{% endhighlight %}

### You’ll need to learn *some* Objective-C (*damn*)
Mainly because the Cocoa frameworks are written in Objective-C, when something isn’t available as a Swift object, you’ll need to reach for its Objective-C equivalent. But how will you know when a particular method is not available? And how will you know what it’s equivalent is and how to use it?

Uh… after much hair-pulling, I guess you’ll have to look at the docs somewhere and try to figure it out. Doesn’t sound too complicated but it can get quite heinous very quickly. For example, Swift already has a `string` type but it doesn’t have all the methods you might require in a string, namely, it doesn’t have the `stringByReplacingCharactersInRange` but `NSString` does so you declare your string as an `NSString` type instead of `String`:

{% highlight objective-c %}
  let oldText: NSString = textField.text
  let newText: NSString = oldText.stringByReplacingCharactersInRange(range, withString: string)
{% endhighlight %}

or the ubiquitous `NSObject` which, again, inherits a lot of nice stuff that the regular Swift object (for some unknown reason) doesn’t have. Like being `equatable` which simply means you can use the `find` method on it.

Oh, the joys of Apple products which now force us to learn not one but two programming languages. Seriously, who OKed this? I know that during Steve’s Reign this would have never been allowed, ever. For that matter nor would the iPhone 6 plus, but Steve is dead, long live Steve, so those days are over.

## Missed Opportunity
One of the uglier things in Objective-C are the method names. It’s good that they’re descriptive so you have no doubt what they do but they are overly so, it’s as if the engineer who came up with them was taking a course in positive thinking coupled with Medieval English and patterned them after the lexicon of a shy waiter in a late 1800s Victorian novel.

> Excuse me sir, did you by any chance, Finish Lunching on this delicious salad plate that the Chef Lovingly Prepared for you?

What the hell? I don’t want Jeeves, the mildly autistic waiter, as my methods manager, I want Jimbo the monosyllabic, assertive redneck:

> added Item

> I’m sorry Jimbo, did You Finish Adding the Item?

> Wadda I just say?

> Oh, yes I see, right, you were quite unequivocal about it, ha, ha, thanks. I’m just not used to such short sentences.

> Grunt

(Southern twang optional)

For example:

{% highlight objective-c %}
  didFinishAddingItem
{% endhighlight %}

Why not simply:
{% highlight objective-c %}
  addedItem
{% endhighlight %}

or
{% highlight objective-c %}
  didReceiveMemoryWarning
{% endhighlight %}

How about:
{% highlight objective-c %}
  memoryWarning
{% endhighlight %}

I mean, what’s with all the *dids*? It sounds more like a question than an assertion,

> did you add the item?

> yes I didFinishAddingTheItem

Sheesh!

also
{% highlight objective-c %}
  dequeueReusableCellWithIdentifier
{% endhighlight %}

what’s wrong with:
{% highlight objective-c %}
  reuseCell
{% endhighlight %}

It seems to me a real shame that Apple didn’t take this opportunity to rename these methods and shorten them to something more sensible. Yes, yes, I know that would throw the Objective-C crazies for a loop but these rabid dogs will never learn Swift, why should they when they can be completely competent in their horrible language? Seriously, there is no incentive for a seasoned Objective-C bastard to move to Swift, especially if all of the frameworks are still written in Objective-C, what advantage would it net them? None that I can think of.

So I maintain they should have shortened them, don’t remove the old ones, just make aliases. More changes that might lead to more bugginess? Not necessarily, and I think all new developers would thank them in the long run.

## Xcode is not ready for Swift
Try refactoring something in Swift, say simply changing a function’s name. Ha, ha, it won’t work! Xcode will even be nice enough to tell you that you can refactor in C and Objective-C but not Swift!

Refactoring is just a fancy name for rewriting code, but it is an extremely useful tool to make sure you don’t leave any traces behind, doing it manually (which were are forced to do for now) is so much more prone to bugs and errors.

## Xcode is not ready for anything (it’s buggy)
Xcode has always been buggy, it’s part of its charm it seems since Apple has never been able to ship a non-buggy version. Everything has bugs, I’m not talking about weird, obscure bugs, I’m talking about basic bugs like the compiler complaining about non-existent methods when it loses track of things! This is one of the compiler’s main functions yet it doesn’t work properly. The solution to most Xcode bugs (as any Xcode veteran will tell you)? Restart the damn thing.

It’s sad, irritating and frustrating but also a bit of a blessing that after hours of cursing and pulling your hair and trying to find where the filthy bug is coming from, they all disappear when you restart Xcode, like roaches reacting to light, an appropriate metaphor, I think.

### Templates contain all sorts of errors
A template in Xcode is, for example, when you create a new file, then choose a subclass of an existing Framework, say `UItableViewController`. You’ll get some boilerplate code with the functions you need to get the Controller working properly. Well, guess what, it’s filled with errors! Mainly errors with those pesky optional variables, yes **those**. So even Apple doesn’t know how to use them properly. Either that or they were too lazy/dumb to clean up their own templates before shipping out Xcode 6 and haven’t bothered to do anything about it, like patch it, in the time since it came out (first stable release: September 17, 2014). So you can’t even trust the code that Xcode writes for you? Now we’re in some serious shit because you’ll spend your life wondering what’s wrong with your code when it’s Xcode making the mistakes! Holy Fuck, Xcode will write bad code for you on purpose! They should list that as a feature in the iTunes page, no other IDE in the world does that (that I’ve ever used).

## Conclusion
I’ll still use Swift and love it compared to the offensiveness of Objective-C.

Despite all my bitching (hey, bitching is fun) I’m still grateful that Apple came up with Swift. I hate Objective-C with a passion. It’s ugly, inelegant, long-winded, with retarded method names and incomprehensible syntax. Honestly, it’s the worst language I’ve ever seen. Of course I come from the PHP, Ruby, Python, Javascript camp, not from the C and C++ ghettoes where I’m sure Objective-C looks pristine and modern.

Objective-C was a huge stumbling block for me (and I’m sure millions) learning Xcode and iOS and OS X development. Now we have an alternative. It’s not the greatest alternative, it could be a lot better, but it’s much better than Obj-C and it can only get better with time and that is a good thing.
