---
layout: post
title: Extracting Facebook Data
---

Everyone is using facebook today, it is a good opportunity to gather data through it. Maybe we can get insights that are not visible through facebook itself.

How are we going to do it? Facebook has an api called __Graph API__ that allows user get the data. Lets see how facebook define it.

> The Graph API is the primary way to get data in and out of Facebook's social graph. It's a low-level HTTP-based API that is used to query data, post new stories, upload photos and a variety of other tasks that an app might need to do.


Ok. How can I use it? Luckily we do not need go low to be able to use the api. __Facepy__, a python library wraps all these low-level api to a more expressive api. 

To install facepy, run the following in your terminal:

```
pip install facepy
```

Facepy doesn't cover authentication, it assumes you have an `oauth_acess_token` from facebook. An easy way to obtain the token is to visit [facebook explorer][facebook-explorer].

{% highlight python %}
from facepy import GraphAPI

graph = GraphAPI(oauth_access_token)

# Get my latest posts
graph.get('me/posts')
{% endhighlight %}

Now that we can use the api, we need to know what data do we exactly need. For the purpose of this example, we'll try to extract the connection between our friends. We're going to extract up to 2 degrees of connection.



[facebook-explorer]: https://developers.facebook.com/tools/explorer