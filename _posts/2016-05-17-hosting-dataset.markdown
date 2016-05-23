---
layout: post
title: 'Hosting datasets'
date:   2016-05-17 17:00:01 +0800
categories: visualization d3 data science dataset storage
---

On my previous post we've successfuly added d3 layout in a post. But the dataset was hard-coded in the html file. And it looks kind of bad.

Dataset are provided normally via api or a different link than the web app use to visualize. Why is that? 

1. Because of the MVC framework? models are needs to be separated from the view.
2. Needs to be lightweight to save bandwidth
3. For faster / more responsive web application

These are just my opinion but in any case all of these looks logical. You may want to remove the hard-coded data in the html file and move it somewhere else. Here are my suggestions:

1. Github
2. Dropbox
3. Heroku
4. Amazon

While browsing google here is what I found, _Google Fusion Tables_ and _Google Public Data_. But the site looks too public. You may want to host your dataset somewhere personal. 

Ok so lets go back. It seems we can use _github_ to host our dataset. We will follow the steps in this [site](http://www.r-bloggers.com/data-on-github-the-easy-way-to-make-your-data-available/).

Steps to create a data repository in _github_:

1. Create a repo with data
2. Create a cover site
  * Click ```admin``` button next to repository's name
  * Click ```Automatic Page Generator```
  * Choose layout
  * Add _tracking id_
  * Publish page

The only take away here is that you wiill be creating one repository for every dataset, which can make your repository cluttered.

Next option is dropbox, hosting in dropbox is very easy. 

1. Create an account
2. Upload dataset to public folder
3. Copy Link

Thats it for now. I don't see any reason yet not to use dropbox.