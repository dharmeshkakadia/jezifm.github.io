---
layout: post
title: 'Django REST Framework'
date:   2016-07-24 13:00:01 +0800
categories: python rest web framework api endpoint
---

Article aims to support the [official guide](official-guide), by providing clearer explanation of parts that is still confusing even after going through the guide.

The tutorial covers the following:

* Serialization
* Class based views
* Authentication & Permissions
* Relationships & hyperlinked APIs
* Viewsets & Routers

## Serialization

Serializer transforms request parameter to model or vice versa. A typical flow would look like this

![serializer activity diagram](/assets/djangorest-serialization.png)

Serializers can do the following:

1. Transform model to form
2. Validate form
3. Save form to database creating a model in the process

**Serialization** has the most documentation in the **Django REST Framework**. It's important to be familiarize with the technique.

## Class based views

A url/resource can be assigned to a class, you will be defining how each HTTP method will be handled. Inside the class you need to implement `get`, `post`, `put`, `patch`, `delete` methods.
 
Class based views is the building block for `ViewSets`, a higher abstraction of Django REST framework.

## Authentication

Authentication in Django REST works by intercepting the request then passing it to all the authentication backend. If one succeed then `request.user` and `request.auth` is populated. `request.auth` is the authentication backend used, Eg. `facebook`, `google` or `Django` itself.

## Permissions

After authenticating the user, this check if `request.user` has the rights to access the `resource` being requested.

## Relationships & hyperlinked APIs

Used to improve cohesion and discoverability. Few `API` have these, if you can implement this then you have a good RESTful interface.

## Viewsets

It implements standard way for performing `CRUD` to the model. It will generate two resource namely **List** and **Detail**. **List** resource will be responsible for `create` model and `retrive list` of record. While **Detail** resource for `retrieve`, `update`, `delete` single record.

[official-guide]: (http://www.tomchristie.com/rest-framework-2-docs/)
