---
layout: post
title: 'Adding search engine optimization'
date:   2017-03-26 12:29:22 +0800
published: false
categories:
---
After generating few posts, It would be great if I can increase online exposure of this blog. One solution I can think of is by having this blog to be shown in google search results. A popular term comes to mind and it is SEO. So what exactly is SEO? Lets see what [wikipedia](https://en.wikipedia.org/wiki/Search_engine_optimization) has to say...

> Search engine optimization (SEO) is the process of affecting the visibility of a website or a web page in a web search engine's unpaid results—often referred to as "natural", "organic", or "earned" results. In general, the earlier (or higher ranked on the search results page), and more frequently a site appears in the search results list, the more visitors it will receive from the search engine's users; these visitors can then be converted into customers.[1] SEO may target different kinds of search, including image search, local search, video search, academic search,[2] news search, and industry-specific vertical search engines.

Exactly what I need, I'd like to increase the visibility of this website, as quoted above the earlier the result its more likely to receive visitors from the search engine. Currently when I search for my name this blog never came to the result, even at 2nd page.

I think we know what we want and it is SEO. So, how do I enable SEO? A quick search at github documentation shows how easy it is to enable SEO, you can follow the instruction [here](https://help.github.com/articles/search-engine-optimization-for-github-pages/).

- Add plugin to `_config.yml` file:

```yaml
gems:
  - jekyll-seo-tag
```

This specify that we are going to use a plugin called `jekyll-seo-tag`. It is a jekyll plugin to add metadata tags for search engines and social networks to better index and display your site's content.

- Of course after installing the plugin we need to insert the html code snippet to our default page. We need to place this after the `include header.html` line.

```html
...
<body>
	{% raw %}
    {% include header.html %}
    {% seo %} <!-- add line -->
	{% endraw %}
    <div class="page-content">
...
```

You need to run `gem install jekyll-seo-tag` to ensure you have the package installed.

Now that we have the SEO enabled, we need to ensure that we are using it propertly. See sample `_config.yml` below:

```yaml
title: Mauris ac felis vel velit tristique imperdiet.
email: mauris-felis@gmail.com
description: >
	Pellentesque dapibus suscipit ligula.  Donec posuere augue in
    quam.  Etiam vel tortor sodales tellus ultricies commodo.
    Suspendisse potenti.  Aenean in sem ac leo mollis blandit.  Donec
    neque quam, dignissim in, mollis nec, sagittis eu, wisi.
    Phasellus lacus.  Etiam laoreet quam sed arcu.  Phasellus at dui
    in ligula mollis ultricies.  Integer placerat tristique nisl.
    Praesent augue.  Fusce commodo.  Vestibulum convallis, lorem a
    tempus semper, dui dui euismod elit, vitae placerat urna tortor
    vitae lacus.  Nullam libero mauris, consequat quis, varius et,
    dictum id, arcu.  Mauris mollis tincidunt felis.  Aliquam feugiat
    tellus ut neque.  Nulla facilisis, risus a rhoncus fermentum,
    tellus tellus lacinia purus, et dictum nunc justo sit amet elit.
author:
  twitter: mauris-felis
social:
  name: Mauris Blog
  links:
    - https://twitter.com/mauris-felis-ac
    - https://www.linkedin.com/in/mauris-felis-ac
    - https://github.com/mauris-felis-ac
    - https://www.kaggle.com/mauris-felis-ac
```

To be sure that the plugin is working I have to check my current seo score [seositecheckup.com](https://seositecheckup.com). Currently the blog scores **78 out of 100**, hopefully this will increase afterwards.

_Update!!_ For some reason the score went down to **74**. I think this has something to do with `jekyll` using `h2` tag for each post. But before jumping into conclusion I need to find out how really SEO works.
