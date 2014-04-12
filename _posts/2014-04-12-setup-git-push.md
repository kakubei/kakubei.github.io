---
layout: post
title: "Setting up git with SSH to update your site on push"
---

Due to the immense popularity of this blog, I've received thousands of requests on how to properly setup git with a regular hosting provider to publish your site with just a `git push` command. *

\* **Disclaimer:** This is an unashamed lie, *thousands* of requests is a bit of an exaggeration, nobody reads this shit, so it was actually more like 0 requests. But hey, Dipak Chopra and the rest of the *The Secret* crew assure me that if you send your positive wishes to the Universe they will all come true (or some crap like that), so this is me putting my positive vibes out there.


Let's assume you were so enthralled by my [brilliant piece on blogging software]({{ site.url }}/blogging-software-link) that you can't wait to try [Jekyll](jekyllrb.com) to create your very own blog. But you don't want any plain vanilla, crappy blog, you want the whole enchilada, all-you-can-eat, hog-wild blog. Well, once again, I'm here to help you.

First things first. Let's install Jekyll

## Installing the Jekyll gem
Open up a terminal session (Applications/Utilities/Terminal), then type this:

{% highlight ruby %}
gem install jekyll
{% endhighlight %}

Now we have the gem that does all the magic.

## Lanyon

Now let's get the [Poole Lanyon](https://github.com/poole/lanyon) variant which has some nice styles to get you started. You can either clone the repository by `cding` in the Terminal over to where you want your blog to be and typing this:

    git clone https://github.com/poole/lanyon

If you don't know what the above means or the Terminal scares the heebie-jeebies out of you, just click the [Download ZIP](https://github.com/poole/lanyon/archive/master.zip) link right here, then:

* Grab the unzipped folder
* put it where you want your blog to live
* rename it to something sensible and original like `blog`
* and edit it with your preferred editor. I suggest [SublimeText](http://www.sublimetext.com) or [Rubymine](http://www.jetbrains.com/ruby/) if you do any Ruby programming.

## Enable git
If you've never used git, it's a [version control system](http://en.wikipedia.org/wiki/Revision_control) which means it basically helps you keep various versions of your files so that world won't end when you make horrible mistakes and completely screw up your program. Not that any of us would *ever* do that.

Back in the terminal, `cd` over to where your blog lives and types this:

    git init

That enables git.

## Add files to git
Now we need to add the files that we want to track. You can pick and choose which files to track and that's always recommended, but since this isn't a git tutorial, it's a *Just give me my damn blog already* tutorial, we'll just add everything to git.

Still in the terminal, type this:

    git add -A

this adds everything in the current folder. This means all your files are now being tracked.

## Commit changed or new files to git
Every time you are happy with your changes to a file you need to commit them to git, telling git that you want it to remember the state of this file. Git doesn't do this automatically. *you must commit your changes to git every time*. But we haven't changed anything! I hear you say. True, but we did just add some new files and you also need to commit new files. So, `changed or new file = commit`, got it?

Terminal time, type this:

{% highlight bash %}
    git commit -m 'initial commit'
{% endhighlight %}

The `-m` stands for `message`. Each time you make a commit, make sure to add a descriptive message of what you changed. This is really useful for when you need to go back and see where you messed up.

You can find out more about the git commands [here](http://git-scm.com/book/en/Git-Basics-Getting-a-Git-Repository).

Enough with git, let's move on.

## Publishing to github pages
If you want to publish your blog to github pages for free, follow their excellent tutorial [here](https://pages.github.com).

If you already have a website and would like to publish your blog there for whatever reason, continue reading.

## SSH
[ssh](http://en.wikipedia.org/wiki/Secure_Shell), or secure shell is, as its name implies a secure shell. It's a secure way for computers to communicate, over a network (usually the Internet) without having to use passwords and without people being able to sniff over what they're saying to each other. Actually, that's technically incorrect, but I don't want to explain what a shell is, etc. so just go with it for now.

We'll use `ssh` to publish our blog to our remote server on the internet. In order to do this, we need to create secure keys so that the computers can trust each other. `ssh` can be daunting but just follow along and it doesn't have to be.

\* Another Disclaimer: the first s in `ssh` stands for secure, so security is very important when you're dealing with `ssh` otherwise it sort of defeats the purpose. Below, I'm recommending you don't use a passphrase just because it's easier to setup and use, but please do [read more about ssh](http://jimmyg.org/blog/2008/beginners-guide-to-ssh-keys-with-ssh2.html) and be conscious of security issues. And no, you can't sue me if your computer gets hacked.


## Create SSH keys
We're going to create those keys on your computer. Let's go back to our friend the Terminal.

    ssh-keygen

It will prompt you for the location to save the keys, just hit `enter` to choose the default of `~/.ssh` which is fine.
Next it will ask you to enter a passphrase, thsi is optional, it's basically so no one else can look at your keys or do anything with them without this passphrase, to make things easier we won't add a passphrase here, once again, hit `enter` when prompted for the passphrase.

This will create your private and public keys `~/.ssh/id_rsa` and `~/.ssh/id_rsa.pub` respectively.

The magic happens when we add our public key, `~/.ssh/id_rsa.pub` to the remote server. Your private key, don't copy it anywhere, leave it alone where it is, don't touch it, don't even look at it. OK you can look at it, all I'm saying is don't copy it over to anywhere.

## Multiple SSH keys for multiple users
What if I have multple users for different web sites? You might have one user for your computer and another user for your personal site, yet another user for your work site, one more for your Heroku project, this is not unusual, I have a bunch of websites and I use a different user for each because I'm not a **total** idiot. Having one user and password for everything might be a lot easier but it's just asking for trouble.

If you do have multiple users, just generate the keys with:

{% highlight bash %}
    ssh-keygen -C "user-name"
{% endhighlight %}

## Copy the public key to your remote server
We need to copy our public key over to the remote server where you have your site. In my case it's Site5 and they make you request shell access before you can login with ssh so you might have to do that on your server, find out how to connect remotely to it. You can usually do it with the user and password you used to setup the `FTP` for your website.

We need to copy the public key to a file called `authorized_keys` in the server.

The easiest way to copy your public key to the remote server is probably to `echo` (or print it) from the command line over to the remote server. It also has the added benefit of making you feel like a hacker badass. Terminal please and type:


    cat ~/.ssh/id_rsa.pub | ssh user-name@mysite.com ‘cat – >> ~/.ssh/authorized_keys’

Of course, change `user-name` for your user name and `mysite.com` with your site URL.

**If the above doesn't work**, do it manually. Once again in Terminal type this:

    cat ~/.ssh/id_rsa.pub

This will print out your public key, which starts with `ssh-rsa` and ends with your user name. Copy the whole thing, then ssh over to your server with:

    ssh user-name@mysite.com

once there see if the `~/.ssh/authorized_keys` exists, if it doesn't create it with:

    mkdir ~/.ssh; touch ~/.ssh/authorized_keys; chmod 700 ~/.ssh

Now open `~/.ssh/authorized_keys` with your favourite terminal editor like Emacs or Pico or Nano, paste your public key and save and close the file.

Congratulations, if you made it through all this you should be able to login to your site without providing a password, awesome. Get yourself a beer, you earned it.

## Publishing the site
We're almost done, now we need to setup git to publish our site remotely. I know I said that we were done with git and were moving on. I lied, we need one last thing with git, but this is it I promise, after this you're *almost* done and will have a wonderful way to publish your awesome Jekyll blog very soon.

OK, let's go. We need to tell git where our remote server is this requires 2 things:

1. Create a git folder where we can put the files on the remote server
2. Set up git to push your changes to it

### Step 1: create remote repository
Still in your remote website, choose a place to put your files. This is not the folder where your site will be, git requires its own folder to put its own files, not your published web files, and type this:

    mkdir blog.git
    # this creates the folder where the git files will live
    cd blog.git
    git init --bare

The last command will create an empty git repository so that when we push git won't complain.

### Step 2: tell git where to push changes
Back on your computer we need to tell git where to push the changes, the easiest ting to do is to go to the Terminal and `cd` over to where your blog is and type this:

    git remote add origin user-name@mysite.com:remote_blog_directory

`remote_blog_directory` is the name of the folder you created in Step 1 above. And of course, change `user-name` and `mysite.com` for your user and site name.

**If the above doesn't work or you want to do it manually**, you need to edit this file `.git/config` in your blog folder. This file is hidden so use whatever you have on your Mac to show you empty files, an easy way to do this is to go to the Finder and hit `Command-Shift-G` which will prompt you for a path, type this in the path: `path_to_my_blog/.git`.

If you installed SublimeText you can also type this in the Terminal: `subl path_to_your_blog/.git/config` and it will open that file for editing.

Git now knows where to publish your blog and since we already setup `SSH` before, it won't prompt you for a password.

## Tell git to copy our files to the website folder after we push
OK there is one last thing to do before we can push our site with git and see our hard-earned rewards, we need to tell git that every time you publish a change to your site, it needs to copy all the files from its internal repository and over to the folder where your website actually exists. This is easily accomplished thusly.

On the remote server, go to the folder we created in Step 1, then `cd` over to the `.git/hooks` folder. You will find a bunch of files there, the one we want is called `post-receive.sample`. Copy a version of it so we can edit it:


    cp post-receive.sample post-receive

Now edit it (again with whatever editor you have on the remote server) and put this into the file:

{% highlight bash %}
    GIT_WORK_TREE=/home/user-name/public_html/ git checkout -f
{% endhighlight %}

In this case, replace not only `user-name` with your user on the remote server but also `public_html` with whatever folder you're using to serve your blog. For example, if the blog is in `public_html/blog` then put that in there.

This hook tells git to copy everything over to that folder every time you push a change.

## Pushing our changes and publishing
Alright, this is it, the final command to publish your site! Yay.

Create a new post in your local blog, something earth-shattering like `hello world` and let's push that baby.

Go to your old fiend the terminal, still in the folder for your blog (on your computer, not the remote server) and type this:

    git push origin master

If all the planets are aligned, you should see a message saying everythig was successful.

Now visit your site on a browser and bask in the fuits of your hard labour. I know I'm mixing my metaphors but this occasion merits it, we've done **a lot** of hard work and now it's time to enjoy it. Get yourself 2 beers this time, minimum.

The good news is that from now on, the only thing you need to do is:

{% highlight bash %}
    git commit -m 'added new post (or whatever)'
    git push origin master
{% endhighlight %}


And your site will be updated. Remember the `git commit` command is to actually commit the changes to git. If we don't commit them and try to push something git will tell us there is nothing to push.


## GUI alternatives
You love git but hate the Terminal? Use [Tower](http://www.git-tower.com), a wonderful app for all your git needs.

If you use Rubymine, it has built-in control for git so you can add, commit and push without ever touching the command line terminal.

I hope this actually helps someone. If it does, drop me a line in the comments section below.

## Coming Soon
In a next installment I shall reveal the secrets to all the changes I made to this blog so you can be as cool as me ;)
