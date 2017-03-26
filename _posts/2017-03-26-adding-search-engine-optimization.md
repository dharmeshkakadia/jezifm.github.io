---
layout: post
title: 'Adding search engine optimization'
date:   2017-03-26 12:29:22 +0800
categories:
---
After generating 10 posts, I wish to improve online exposure of this blog using google. This blog uses github pages with the help of Ruby's jekyll to generate static content. A quick search shows there is a SEO tag plugin for github pages, instruction [here](https://help.github.com/articles/search-engine-optimization-for-github-pages/).

To enable SEO,

- Add plugin to `_config.yml` file:

```yaml
gems:
  - jekyll-seo-tag
```

- Add html snippet to default page

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

After enabling SEO, I need to ensure that I'm using the plugin properly. Looking at the *usage* section from the docs, we can add SEO tags in `_config.yml`

```yaml
title: My Awesome Blog
email: this-is-awesome@gmail.com
description: This is awesome
author:
  twitter: awesomeblog
social:
  name: Awesome Blog
  links:
    - https://twitter.com/awesomeblog
    - https://www.linkedin.com/in/awesome-blog
    - https://github.com/awesome-blog
    - https://www.kaggle.com/awesome-blog
```

But before I enable SEO I need to have a way to check if it really does work. Simplest I can think of would be using online SEO ranking too like [seositecheckup.com](https://seositecheckup.com). The site got score of 78 out of 100. Now lets see what's going to happen after adding SEO.

#### TODO re-evaluate SEO metric
