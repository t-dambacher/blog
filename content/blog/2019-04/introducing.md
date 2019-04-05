---
title: "My first baby step"
date: 2019-04-03T18:07:35+02:00
draft: false
tags: ["Azure", "Introduction", "Static website", "Blog", "Blob storage", "Storage", "CDN", "Hugo" ]
series: []
categories: ["Other"]
toc: false
---

Hi! Welcome to my blog! This is my first post _ever_, so please be understanding! As a first simple exercice, I thought I could start by introducing myself briefly, and talk a bit about how my blog is powered.

### Me, myself & I
Ok so first things first, let me introduce myself. My name is Thibaut, I'm a french guy living near Strasbourg, in France. Aside from wine, cheese and baguettes (obviously), I love computers and science.\
I alsco love music, listening to it, playing it, and recording it. In fact, I play guitar and piano in a little band called [_Last Sunset On Uranus_](https://soundcloud.com/lastsunsetonuranus) (yes, our name is a bad joke :smile:). Come check us out!

On the IT side, I recently started a new gig as a senior .NET engineer in a [consulting firm](https://twitter.com/sfeir). Time to say the boring stuff: as this is my own private blog and not a corporate one, my opinions are mine only and do not reflect in any way my employer's. Ok, done.

Aside from the pure development stuff, part of my job is to preach and teach the best practices, and help our individuals grow, and our teams to scale. I read blogs daily, and for once want to be part of the blogosphere too, and log my thoughts somewhere. On a more personal note, I want to try to give something back to the community. I am considering participating in some open source projects, but that's not nearly clear enough for now. Anyway, this is the reasons I am starting this blog, as a way to store in the open what I have learned. And if it could even help someone someday, it would be a great reward.

Ok, enough with this, let's talk tech!

### The blog's stack
I've come accross a great post by Matt Fisher, very appropriately called  _[Blogging for pennies on Azure](https://blog.bacongobbler.com/post/2018-03-09-blogging-for-pennies-on-azure/index.html)_. As the title says it, Matt explains how to host a blog on Azure, and I pretty much followed his tutorial to get this site up.

In brief, this is how it all work :

* The blog's content is stored as markdown files. There isn't a database where I put my thoughts through a WYSIWYG interface, nor HTML pages with static content included and edited by hand. I'm writing it through VS Code with the mardown preview enabled, but any editor works. [Here is even the content](https://github.com/t-dambacher/blog/tree/master/content/blog/2019-04/welcome.md) of this very own post!
<img src="/img/blog/thats-not-enough-we-need-to-go-deeper.jpg" alt="Blog-ception" title="Blog-ception" class="mx-auto d-block" style="width: 50%;">
* I use Github to store the content. This allows me to have all benefits of a distributed VCS: I can write from anywhere (like the bathtub), save what I've written, and continue the next day on another computer. I'll forever have access to all revisions and could see what changed. I'm even able to ask someone to proof-read me before publishing in a _code reviews_ fashion, with a pull request. For a techie, this is a great way to work!
* Once written, the content goes through [Hugo](https://gohugo.io/), a _static website generator_. This means that my blog is fully static and does not need a backend with a runtime to interact with. Hugo is the CMS-like part, where I define the visual style of my blog, and create reusable layouts and partial views. As it's _very_ fast to generate the output, I can see the result of my work instantly.\
Hugo comes with a lot of features and templates, so you can start blogging quickly and don't have to start from scratch; those template are also available on Github, and I reference mine as a submodule, so I'm able to get the new changes automatically on checkout.
* I added _Continuous Delivery_ support through an Azure Pipeline, which triggers a full build at each commit on the _master_ branch of the blog's git repository. Azure checks out the full content, runs it through Hugo, and pushes the generated html and the other resources to an Azure Storage Account through the `AzCopy` command line tool.
* The data are stored in Blob Storage, which is a cloud mechanism to store any data in binary blobs. The files are typically manipulated through a REST API, but Microsoft added a feature called [_static website_](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-static-website-host), which allows the download of a file with a standard GET request instead of posting some JSON to a REST endpoint. It also allows the use of a default file while querying the account's endpoint, so when someone make a request to `myfunnyblog.onazure.blabla.com`, instead of returning a 404 as this isn't an URL pointing to a file, it returns the default `index.html` file. Put in other words, the _static website_ feature simply allows a browser to access the files.
* An Azure CDN is used as a safe net just in case one day, thousand of people will read me. More seriously, it's where the SSL certificate is defined, automatically. A rule also redirects some requests to HTTPS.
* Finally, the domain I use to hide all the partially ugly URLs is registered to [OVH](https://ovh.com), and is also configured to do some _http to https_ redirection at the domain level.

The best thing is that all of this has been deployed in less than a day, without much issues. I only encountered some problems with the _http to https_ redirection and only because I wasn't patient enough, as the DNS weren't propagating as fast as I would have wanted. Once I waited a bit, it all worked great.

It also didn't cost much, as the one and only thing I need to pay for is the domain. The Azure subscription works in a _pay as you go_ manner, which means that I pay based on the consumption of what is deployed in Azure. The Storage is charged based on the size of my files, and costs 0,002€ per GB per month; as I only have a few KB of text and MB of images, it's basically free. The CDN is charged based on the volume of data transferred to the consummers of the CDN endpoints, so the more visits my blog have, the more it costs me, at something like 0,2€ per GB per month. As long as I'm not famous, it's basically free.

So here you have, I'm _blogging for pennies_. What really excites me is the whole _cloud_ and _open-source_ logic: there isn't anything on my side, no physical machine, no backup, no static IP, no database. There's no SaaS CMS (think _Wordpress_) either. Just free softwares, inteconnected and working only when needed, and the underlying stuff is deported to cloud providers, and is hopefully optimised to do things in an efficient manner. I can only imagine what can be done with such a modern architecture on a more complex app. It's a great time to be a developer! 