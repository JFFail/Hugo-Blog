+++
categories = ["Technology"]
date = "2015-08-07T20:46:19-04:00"
description = "Using Jekyll or Hugo for a static HTML blog to Azure Web Apps."
title = "Using Jekyll Or Hugo To Blog To Azure Web Apps"

+++

It's far from impressive, but recently I've been using some of my free Azure hosting via Dreamspark for this blog. The [free tier](http://azure.microsoft.com/en-us/pricing/details/app-service/) isn't exactly impressive, but it's not too bad for just messing around with Azure a bit and learning; I'm pretty sure learning is really what the free tier is intended for regardless. Since I've had plenty of time to get my hands on a lot of the IaaS and SaaS offerings in Azure, I figured I needed to step things up in the PaaS department.

Rather than just hosting my own HTML files like I do [on GitHub](http://awk.ninja), though, I wanted to see about using something like [Hugo](https://gohugo.io) or [Jekyll](http://jekyllrb.com) to parse together a static HTML blog that could be hosted out of Azure. I'm already familiar with both frameworks [from SDF](http://failtime.freeshell.org), and I'm a *huge* fan. Being able to so easily create static HTML blogs is **incredible**, and I can't give enough credit to the teams behind these projects. Plus, they seemed like a perfect fit considering that Azure Web Apps allow you to do [continuous deployment via Git](https://azure.microsoft.com/en-us/documentation/articles/web-sites-publish-source-control/). So in my case I can link my Azure Web App to a GitHub repository. Then whenever I push a change to that repo, it'll automatically load in Azure. Cool!

The hiccup I ran into is that, by default, when you link an Azure Web App to a Git repository Azure wants to serve up the *root* of that repository. In my case that was problematic because the root of it was the root of [my Hugo directory](https://github.com/JFFail/Hugo-Blog); the HTML files I wanted to be hosted were in the `/public` directory. After a little bit of searching, I learned that [Kudu](https://github.com/projectkudu/kudu) is the answer to this. Kudu is the engine behind Git deployments in Azure. You can create a file in the root of your repository called `.deployment` and use that to tell Kudu where to look for the files to host. It seems to use TOML for the markup, which is great since that's what I've been using for Hugo as well. The only things I added to my deployment file were:

    [config]
    project = public

This lets Kudu know that when it goes to host the content in my repository it should be looking in the `public` folder rather than in the root. I also temporarily flipped which repository my Azure Web App was looking at to test it out with [this Jekyll repo](https://github.com/JFFail/Blog). Things also worked perfectly with Jekyll; I just had to modify my deployment file to take into account the default folder where Jekyll wants to publish HTML:

    [config]
    project = _site

I have no doubt there are a *ton* of other things you can do with Kudu to further customize Git deployments in Azure. I was just geeked, though, to be able to do precisely what I wanted with so little effort.
