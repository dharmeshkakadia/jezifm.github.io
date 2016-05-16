---
layout: post
title:  "D3 in Jekyll"
date:   2016-05-16 19:00:01 +0800
categories: d3 visualization jekyll
---

I will be using jekyll to provide my blog. Since visualization is one the main reason why I did create a blog. It is important that the blogging technique I'll be using is capable of doing such feat. 

{% include blog-head.html %}

I did a quick search on google and stumble upon this blog [Embedding D3.js in a Jekyll blog post][embed-d3]. There are 3 things that keeping me from creating a blog post with d3 in it.

1. Add javascript in jekyll.
2. How to put the visualization in the layout.

According to the [blog][embed-d3], to append a javascript to a post you just need to add the following lines below your markdown file.

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/d3/3.4.12/d3.js"></script>
<script src="{% raw %}{{site.baseurl}}{% endraw %}/js/embed-d3.js"></script>
```

After adding the following lines you need to place your javascript in ```<jekyll_repo>/_site/js/embed-d3.js```

Now the next problem is how do we add our visualization in the layout. Luckily for us, jekyll allows us to place raw html in the markdown file. We just need to add the following lines below our markdown.

```html
...markdown...

<div id='chart'></div>
```

To see if it is working see example below.


[embed-d3]: http://zeptonaut.com/coffeescript/d3/jekyll/2014/10/16/embedding-d3-in-jekyll.html

<div id="element"></div>
<script src='https://code.jquery.com/jquery-2.2.3.min.js'></script>
<script src='https://d3js.org/d3.v3.min.js'></script>
<script src='https://d3js.org/queue.v1.min.js'></script>
<script src='https://d3js.org/topojson.v1.min.js'></script>
<script src='https://code.jquery.com/ui/1.11.4/jquery-ui.js'></script>
<script src="{{site.baseurl}}/js/2016-05-16-adding-visualization-jekyll.js"></script>