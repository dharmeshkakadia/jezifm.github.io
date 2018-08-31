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

![core_components](/assets/job-aggregator-core-components.png)

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

---
# TODO

Each of the sources have different structure, I needed it to be
transformed into a more use-able format, since they will be
consolidated into one long list. The ratio is 1 source 1
transformer and 1 item corresponds to 1 posting. Having consistent
granularity makes it much easier to transform data.

Here I have to make a decision,

> How do I trigger the transformation? event-based? schedule-based?

I decided to go with schedule-based reason being is having the trigger
independent of the extraction frequency lessen coupling between
collection and transformation stages.

## TODO Relational Database

Having transformed each item to a uniform structure it is proper to
use a relational database. In this case I use **Postgres**

## TODO Web Application

To view the data to the user we need to have a web application that
can query the job listing and present it to the user, I decided to use
**Django** in this case since I am familiar with it.

## TODO Static Files

## TODO Web Server

For the server, I want it to be secured using *https* therefore I
decided to use **Caddy**.

# TODO Deployment and Scaling

All of the services are very light and can be deployed in a single ec2
`t2.micro instance`. It is running with **CPU utilization of 1-3% and
spiking to 40%** when there are a lot of new job posting ingested by
the system. **Memory Usage** is at **800+mb**

The system can scale horizontally, as the job sources increases we can
load balance it across different scraper.

If we want to increate request per second eg too many user is viewing
the system we can create a `master-slave` setup, and then load balance
it across multiple slave read only database.
