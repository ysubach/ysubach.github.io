---
layout: post
title: "Google Cloud Platform for static website hosting"
tags: [google, cloud, website]
---

Nowadays speed of website is very important factor that affects conversion and
overall user experience. Since significant amount of data loaded for every
page belongs to static assets (such as images, CSS and JavaScript files)
category optimization of delivery here can contribute a lot to overall website load
speed.

Technique described in this post is generally applicable for two major use
cases:

* _Subdomain for static assets_: all your images, CSS and
JavaScript will be loaded from "static.mydomain.com".

* _Entire static website_: all website files for "www.mydomain.com" are static
and loaded from main domain. 

This is not rare situation. The number of 
[static site generators](https://www.staticgen.com/) allow to publish blogs
and other pages easily. Besides that modern dynamic systems created with
underlying API layer can be easily deployed as static websites because of
clear separation of web content from dynamic features.


## Cloud storage for website

The idea here is to upload all static files to the cloud storage provider and
allow it to serve them for you.

This service is offered for free by various cloud providers. You need to pay
associated data storage and transfer costs but for majority of systems it'll
be minimal.

* Google Cloud Platform: [Hosting a Static
Website](https://cloud.google.com/storage/docs/hosting-static-website)
* Amazon Web Services: [Hosting a Static Website on Amazon Web
Services](http://docs.aws.amazon.com/gettingstarted/latest/swh/website-hosting-intro.html)
* Github offers [Github Pages](https://pages.github.com/) which is good if you
want to use their Git repository

The rest of this post focuses on Google Cloud Platform solution.


## How to start

Before you begin please:

1. Create [Google Cloud](https://cloud.google.com/) account with billing enabled
1. Create a project
1. Make sure you own or manage domain's DNS settings

Now you can follow [instructions from
Google](https://cloud.google.com/storage/docs/hosting-static-website), 
as result you'll be able to:

* Associate a Cloud Storage bucket with a domain name.
* Upload your site's files.

When all above steps are completed you can test the website, or test our own
website at [dekses.com](http://dekses.com)

In general, everything works smooth except a couple of small issues we are
going to highlight below.


## ACL permissions

By default public access for your Google Storage bucket is disabled. So you
need change ACL settings before anyone can access you website. One way of 
doing that is to update setting for each file recursively:

{% highlight no %}
gsutil acl -r ch -u AllUsers:R gs://www.mydomain.com/
{% endhighlight %}

This method is slow if you have many files. The faster way is to change
default ACL settings, which is fine because we mapped entire bucket to the
domain.

{% highlight no %}
gsutil defacl ch -u AllUsers:R gs://www.mydomain.com
{% endhighlight %}


## URL rewrites

Let say you have "category.html" file and you want it to be available on your
website from "http://www.mydomain.com/category" URL. For traditional hosting
with Apache, Nginx or any other web server you can easily define processing
rules or "rewrites" to achieve that.

This is not supported by Google Storage but we can get the same thing if the
file is renamed to "category" before uploading to the bucket. The only issue
now is that this file will be offered for download instead of rendering as a
page. It happens because "Content-Type" header is defined automatically by the
file extension, so we just need explicitly define it for "category" file:

{% highlight no %}
gsutil setmeta -h "Content-Type:text/html" gs://www.mydomain.com/category
{% endhighlight %}


## Quick updates deployment

Not as issue, just a good way to upload updates for your website using "rsync"
command:

{% highlight no %}
gsutil -m rsync -d -R /my/local/folder gs://www.mydomain.com
{% endhighlight %}


## Summary

Modern cloud storage solutions are definitely great tools for static assets
and entire websites storage and delivery. This is valuable design
pattern and another step into cloud-enabled future.
