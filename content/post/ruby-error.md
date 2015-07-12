+++
categories = ["Technology"]
title = "Error Installing Jekyll Gem: Can't Find Header Files"
description = "Error installing Jekyll Ruby Gem."
date = "2015-06-24T11:35:00Z"
keywords = ["jekyll, ruby, gem, install"]

+++

As you may be able to tell at a quick glance, I *just* configured this blog that I have running on a VPS I spun up bright and early this morning. One of the things I wanted to do was get nginx rolling on it and then install Jekyll to do a bit of blogging. While the nginx setup went incredibly smoothly, I started to run into a couple of hiccups when installing the Jekyll gem. Note that I'm doing this from a Debian 8 VM.

After installing Ruby, since Debian doesn't have it by default, I ran the following:

    sudo gem install jekyll

It caught me off guard that this threw an error:

> john@mads:~$ sudo gem install jekyll
> Building native extensions.  This could take a while...
> ERROR:  Error installing jekyll:
>         ERROR: Failed to build gem native extension.
> 
>     /usr/bin/ruby2.1 extconf.rb
> mkmf.rb can't find header files for ruby at /usr/lib/ruby/include/ruby.h
> 
> extconf failed, exit code 1

Yeah, that's not what I wanted. I did some searches for this error and most of them were from folks using OS X where the problem was that Ruby couldn't find **make**. Since I actually didn't have **make** installed, either, I went ahead and put the full software compilation package on my VPS:

    sudo apt-get install build-essential

After that I did have **make**:

    which make

Sadly, trying to install Jekyll still gave me the same error. I continued digging through various help forums and StackOverflow posts before finally stumbling across one that said you need the Ruby developer tools to install Jekyll. I threw that package on my VPS next:

    sudo apt-get install ruby-dev

Sure enough, after that installing Jekyll completed without any issues. So if you're trying to get Jekyll up and running on Debian (or presumably one of its derivatives like Ubuntu or Mint) but you're seeing the error above, make sure you've got the Ruby developer tools installed first.
