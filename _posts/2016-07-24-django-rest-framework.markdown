---
layout: post
title: 'Django REST Framework'
date:   2016-07-24 13:00:01 +0800
categories: python rest web framework api endpoint
---

Article aims to support the [official guide](official-guide), by providing clearer explanation of parts that is still confusing even after going through the guide.

The tutorial covers the following:

* Serialization
* Requests & Responses
* Class based views
* Authentication & Permissions
* Relationships & hyperlinked APIs
* Viewsets & Routers

## Serialization

Being new to the framework, serialization seems to have no place in a RESTful framework. 

> What is the function of a serializer?

Serializers can do the following:

1. Converts model to dictionary and vice versa.
2. Validates a dictionary.
3. Create a model from a dictionary.

Basically it **maps** model fields to dictionary attributes, and provides validation on the process. One can skip the serialization and do the mapping in the view. But the problem with that is, similar views will have similar code and will violate the **DRY** principle.

**Serialization** has the most documentation in the **Django REST Framework**. It's important to be familiarize with the technique.

### Serializers


### Fields

### Relations

## Requests

> What is a request object, is it different from core Django's HTTPRequest object?

Django REST's `Request` is different from Django's `HTTPRequest`. REST developer's added a new attribute `DATA`  (not sure why is it in all caps) that can handle not just **POST** form data but also data coming from **PUT** and **PATCH** methods.

## Class based views

No reason to prefer this view. Class based views is the building block for `ViewSets`.

## Authentication

Basically it is responsible for populating the `request.user` and `request.auth` fields.

## Permissions

> Why is this in the views? why not put it in the serializer since validation is in there?

It uses the `request.user` and `request.auth` to determine if the request if permitted. 

## Relationships & hyperlinked APIs

Used to improve cohesion and discoverability. Not needed, optional.

## Viewsets

It implements standard way for retrieving a list of model. CRUD access of the models. As seen above, right now there is no reason to prefer class based view over viewsets.



[official-guide]: http://www.tomchristie.com/rest-framework-2-docs/
