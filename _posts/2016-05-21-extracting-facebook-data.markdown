---
layout: post
title: Extracting Facebook Data
---

Everyone is using facebook today, it is a good opportunity to gather data through it. Maybe we can get insights that are not visible through facebook itself.

How are we going to do it? Facebook has an api called __Graph API___ that allows user get the data. Lets see how facebook define it.

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

Getting the data seems not straight forward sql style approach. Lets go over these.

# Graph API Basics

The Graph API is named after the idea of a 'social graph' - a representation of the information on Facebook composed of:

* nodes - basically "things" such as a _User_, a _Photo_, a _Page_, a _Comment_
* edges - the connections between those "things", such as a _Page's Photos_, or a _Photo's Comments_
* fields - info about those "things", such as a person's birthday, or the name of a Page

To query a node url is:

```
graph.facebook.com/{node-id}
```

for edge:

```
graph.facebook.com/{node-id}/{edge-name}
```

To be able to query my friends, the url is `me/friends`. But running the command returns only 3 result. Based on the summary I should have 555 friends.

{% highlight json %} 
{
  "data": [
    {
      "name": "random name",
      "id": "123456"
    },
    {
      "name": "random name",
      "id": "12345690"
    },
    {
      "name": "random name",
      "id": "12345683"
    }
  ],
  "paging": {
    "cursors": {
      "after": "QVFIUjZAUdENMT1F3ckJxcnA1ZADY4cmxSbWJJU014X2VINjBHeWhhUTNORVdiTzFnVFVHVHR0dzJERXIwYm9TU2ZAJaDFRQjlWQ014TG8zMDZALajU4eGVpa2VB",
      "before": "QVFIUk1PdmNIMnJrNjZA3WkxQdGp6d2pLSUlHRjlsa2dtUlVZAb3VLcGRObVNvcEVlMEJIVDhqaHZAPZA3QtSm1SaExXaTkZD"
    }
  },
  "summary": {
    "total_count": 555
  }
}
{% endhighlight %}

Possible solution is:

* Set result limit
* Use cursor to go to next page

Lets try to use the cursor. According to the documentation.


> When you make an API request to a node or edge, you usually don't receive all of the results of that request in a single response. This is because some responses could contain thousands of objects so most responses are paginated by default.

> __Cursor-based pagination__ is the most efficient method of paging and should always be used where possible. A cursor refers to a random string of characters which marks a specific item in a list of data. Unless this item is deleted, the cursor will always point to the same part of the list, but is be invalidated if an item is removed. Therefore, your app shouldn't store any older cursors or assume that they will still be valid.

> When reading an edge that supports cursor pagination, you will see the following JSON response:

{% highlight json %}
{
  "data": [
     ... Endpoint data is here
  ],
  "paging": {
    "cursors": {
      "after": "MTAxNTExOTQ1MjAwNzI5NDE=",
      "before": "NDMyNzQyODI3OTQw"
    },
    "previous": "https://graph.facebook.com/me/albums?limit=25&before=NDMyNzQyODI3OTQw"
    "next": "https://graph.facebook.com/me/albums?limit=25&after=MTAxNTExOTQ1MjAwNzI5NDE="
  }
}
{% endhighlight %}
> A cursor-paginated edge supports the following parameters:

parameter|definition
---|---
before|This is the cursor that points to the start of the page of data that has been returned.
after|This is the cursor that points to the end of the page of data that has been returned.
limit|This is the number of individual objects that are returned in each page. Note that this is an upper limit. If there are not enough objects remaining in the list of data, then fewer will be returned. Some edges have an upper maximum on the limit value for performance reasons. In call cases, the API returns the correct pagination links.
next|The Graph API endpoint that will return the next page of data. If not included, this is the last page of data. Due to how pagination works with visibility and privacy, it is possible that a page may be empty but contain a 'next' paging link. Stop paging when the 'next' link no longer appears.
previous|The Graph API endpoint that will return the previous page of data. If not included, this is the first page of data.


[facebook-explorer]: https://developers.facebook.com/tools/explorer