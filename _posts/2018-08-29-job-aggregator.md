---
layout: post
title: 'Job Aggregator'
date:   2018-08-29 13:25:10 +0800
categories:
---
# Job Aggregator

A job aggregator is essentially a search engine, like Google or Bing,
but specifically designed to pull job postings from a variety of
career sites and job boards so they can all be viewed in one
location.

# Design

## Use Case

- User registers
- User logins
- User views all jobs
- User sorts job by salary, date posted, company
- User queries related jobs
- User views original job posting
- Service keeps the database updated of latest job posting

### Not scope

- User takes note of job posting
- User views company details and its related Job posts
- User add notes to company
- User view notes while browsing jobs
- Service hints user of bad reviews or notes so he can skip job
  posting

## Assumptions

- Few Users (low traffic)
- Write API is only during registration

# High Level Design

![high_level](/assets/job-aggregator-high-level.png)

# Core Components

## Scraper

There are only a few scraping libraries you can use in python. In top
of my head:

- Native Libraries (xml + httplib)
- Requests + Beautiful Soup
- Scrapy

I have to go with `scrapy` because its fast and has rich API I can
work with. It speed was mainly because its using `twisted` as its
server. This allows simultaneous fetching multiple URLs. And use
callbacks to parse each of the downloaded page.

## NoSQL

Job posting will be coming from different sites, each one of them
having different format or structure on one another. To be able to
store unpredictable data structure it is good to use a schemaless
storage. NoSQL are designed for that.

Top NoSQL databases are:
- Mongo DB
- Redis
- Couch DB
- Memcache DB

Its good to go with the most popular one, **Mongo DB**. Also it is the
preferred storage when using `scrapy`.

## Scraper Scheduler

For the aggregator to be updated it needs to extract data from the Job
site at a constant pace. To achieve this we need a service that
invokes scrapy periodically like daily, hourly etc.

Possible options could be:
- Crontab
- Celery
- Spiderkeeper + Scrapyd

**Celery** doesn't work well with `twisted reactor core`, so I can't use
it.

**Crontab** is the easiest implementation, but since I plan to deploy this
on docker it would be better to keep our host server clean from any
code or logic.

Scheduler rarely needs to be updated it is good to use an off the
shelf solution. **Spiderkeeper** is an admin UI to help manage `scrapy`
services using `scrapyd` that exposes HTTP API for managing
spiders/scrapers.

## ETL (Extract-Transform-Load)

Data being collected by scraper are unstructured and needs to be
transformed to uniform format if to display data to user.

Source data would be a NoSQL (Mongo DB) and the target is RDMS.

There are the options I think that can be done:
- Shell scripting
- SQL
- Python

MongoDB doesn't have a good cli integration so shell scripting is
out. SQL or mongo query because of the poor cli support this also is
affected and will be difficult to work with.

The only choice left is python, it is good since it can be integrated
with mongo db and almost any RDBMS technology available.

Python has library called `pymongo` used to connect to **Mongo
DB**. It has complete documentation and will be easy to use.

There is no need to change granularity of the data, each of the job
posting has no needs to be aggregated transformation will be done in
1:1 ratio.

Since we wan't out site to be near realtime I choose to run this
script every 90 seconds. If done right most of the time will do
nothing or at least load only a few jobs at a time.

We can do it buy triggering the ETL everytime there is a new data but
that is expensive there is not really a huge gain of having the given
to you 80 seconds earlier. Running the script periodically makes it
efficient and has less CPU footprint.


All the job posting we captured need to be put in a uniform format,
all the fields common to each other should be factored out. Also some
fields that are missing needs to be filled in. In this case if there
is a missing data we can just specify **Unknown**. This is to ensure
that all fields has value and making it easier to deal with things
later.

We haven't chosen a loader of the library we are going to use to load
the data it is because we haven't decided yet what web server to use
or what specific of RDMS is the target load. Later will choose one,
but this time we skip it.

## Web Server

Since python is the main langauge being used there not much of choice
here. Most popular framework are

- Django
- Flask

This case I will use `Django` mainly because it enables rapid
development and has clean design. It has rich toolkit to use. And
works well with deployment. Its preferred database is
`postgresql`.

Going back to the ETL part I am going to use `django's ORM` to
transformed job updates to database. To use it, we need to work well
withing django application.

For periodic or scheduling use case `celery` is the gold standard for
doing this. `celery` will be calling the ETL scripts and running it
every 90 seconds.

# Deployment and Scaling

All of this will be dockerized, therefore deployment is not a
problem. As a matter of fact.

All of the services are very light and can be deployed in a single ec2
`t2.micro instance`. It is running with **CPU utilization of 1-3% and
spiking to 40%** when there are a lot of new job posting ingested by
the system. **Memory Usage** is at **800+mb**

# Conclusion

At first I thought job aggregator service will require higher capacity
servers. But when done well it can be done a small instance in this
case. You can visit actual working prototype
[here](https://jobs.jezarciaga.com) 
