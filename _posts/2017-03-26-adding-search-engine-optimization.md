---
layout: post
title: 'Adding search engine optimization'
date:   2017-03-26 12:29:22 +0800
categories:
---
After generating few posts, It would be great if I can increase online exposure of this blog. Currently it is hosted with github pages and uses `jekyll` to generate static content.

It is easy to enable SEO, you can follow the instruction [here](https://help.github.com/articles/search-engine-optimization-for-github-pages/).

- Add plugin to `_config.yml` file:

```yaml
gems:
  - jekyll-seo-tag
```

- Also, add html snippet to default page after the `include header.html` line.

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

You need to run `gem install jekyll-seo-tag` to ensure you have the right package installed before re-buidling the site if not you may run into some issues like `unknown tag seo`.

Now that we have the SEO enabled, we need to ensure that we are using it propertly. At the *usage* section of its docs, the following `tags` from `_config.yml` are used.

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

To make sure the SEO plugin is working I have to check my current seo score [seositecheckup.com](https://seositecheckup.com). Currently the blog scores **78 out of 100**, hopefully this will increase afterwards.

_Update!!_ For some reason the score went down to **74**. I think this has something to do with `jekyll` using `h2` tag for each post. But before jumping into conclusion I need to find out how really SEO works.
