+++
categories = ["Technology", "Blogging"]
date = "2015-07-12T16:30:53Z"
description = "Using Hugo for blogging and moving from Jekyll."
keywords = ["hugo, blogging, Go, software"]
title = "Using Hugo"
+++

## Background
This is my first post written on my new, [Hugo-powered](http://gohugo.io/) site. I had previously been using [Jekyll](http://jekyllrb.com/) for my blogging, but I decided before I got *too* into things that I should consider switching to something else. The main reason for me was some concern around performance. I chose Jekyll not really realizing that there were competitors in the static-HTML-generation game. I knew that Jekyll was used for [GitHub Pages](https://pages.github.com/), and I just assumed that was what got used everywhere.

Then I happened to see an article on using [VSCode](https://code.visualstudio.com/) to generate a Hugo site. Having never heard of Hugo, I dug into it a little and discovered that it's basically a Jekyll competitor written in Go rather than Ruby. This offers some advantages, the biggest of which is that it's a **lot** faster. While I love Ruby -- I think it's one of the most enjoyable programming languages to write in -- it's hard to deny that there can be some performance issues with large projects... just launch [Metasploit](http://www.metasploit.com/) and see how the wait time is.

I did a bit of hunting to see what *else* was out there on top of Jekyll and Hugo. It turns out there are [quite a few options](http://www.sitepoint.com/6-static-blog-generators-arent-jekyll/). Looking into it, though, Hugo seems to be one most focused on performance. A few people with more dedication than me have done some [fairly exhaustive testing](http://fredrikloch.me/post/2014-08-12-Jekyll-and-its-alternatives-from-a-site-generation-point-of-view/) to confirm that while Jekyll may eventually start to lag with regard to build times for a site, Hugo crushes it... though newer versions are slightly slower than the old ones. As someone who can potentially blog quite frequently I didn't want to invest too much of my time and effort into a Jekyll site only to struggle with migrating the content to something else later on if I started to see performance issues.

## Implementation
Using Hugo was fairly straightforward. I already had a server running Nginx that I wanted to use, so I just had to [download the binaries for Hugo](https://github.com/spf13/hugo/releases). I placed them in my `PATH` for the sake of sanity. I'll just have to keep an eye on their releases on GitHub to see when there are updates. Building a new Hugo site is stupidly easy. I just created a directory where I wanted to store everything and then used the following:

    hugo new site .

This gave me the skeleton struction that I needed. Next up, I wanted to get a theme rolling. After looking through the available themes, I settled on [Hyde-x](https://github.com/zyro/hyde-x). The README for it was awesome enough to give a very thorough rundown of what options are available for my `config.toml` file, so I leveraged that template to fill out my own information. I thought the MD5 hash of my Gravatar email address to import my avatars from there was a neat touch. Adding a new post was also a simple matter by running the following:

    hugo new post/test.md

This creates a brand new post Markdown file for me, complete with the skeleton structure of the front matter that I needed. After fiddling with it, I was able to build my site with a test post:

    hugo

That publishes all of the content to the **public** folder. I actually don't need it there since I'm not going to use Hugo as the web server; that's what Nginx is for. So I just copied the contents of **public** to where my Nginx instance is reading from. I saw that there's a `destination` parameter in the [configuration options](http://gohugo.io/overview/configuration/), so I don't necessarily need to worry about that step going forward. I've been having difficulty getting it to work properly, though, as everything continues to get pushed to my **public** folder. I'll need to do some additional research to see if I'm not understanding it properly. If nothing else, though, this gives me the option of previewing new content by serving it up via Hugo before pushing it to Nginx.

Having successfully published a post, I wanted to migrate just a couple of my old Jekyll posts over. That basically only involved changing the front matter format since I wanted to use TOML rather than YAML. After reconciling that, though, everything rebuilt without any issues. Long live Hugo!
