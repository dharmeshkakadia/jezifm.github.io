---
layout: post
title: 'Job Aggregator'
date:   2018-08-29 13:25:10 +0800
categories:
---
# Introduction

I was looking for a job, but rather than looking what I did was create
a job aggregator service so I can view it in one location

## Design

## Use Case

My use case if very simple a single location to monitor new jobs

- User view all jobs
- User sort job by salary, date posted, company
- User search job posting related to text
- User view original job posting

### TODO Constraint

### TODO Assumptions

# High Level Design

![high_level](/assets/job-aggregator-high-level.png)


The design is simple, from the multiple sources of data different job
hosting site, an extractor will gather or extract job posting and then
put it in a centralize storage which is available for display for the
user.

# Core Components

![core_components](/assets/job-aggregator-core-components.png)

## Object Store

Working from left to right, the multiple job sources poses a problem
of having a non-standardize data format. Some of them may use job
title some position. Because of the dynamic nature of the structure it
best to use a schemaless data storage for this. This case I decided to
use **Mongo** as object store.

## TODO Scraper

## Scheduler / Worker

Not all of the job sources have subscription available so I have to
monitor it periodically to check for job updates or new job posting. I
decided to make this hourly so it doesn't pay too much load on the
server.

## Transformation

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

## Relational Database

Having transformed each item to a uniform structure it is proper to
use a relational database. In this case I use **Postgres**

## Web Application

To view the data to the user we need to have a web application that
can query the job listing and present it to the user, I decided to use
**Django** in this case since I am familiar with it.

## TODO Static Files

## Web Server

For the server, I want it to be secured using *https* therefore I
decided to use **Caddy**.

# Deployment and Scaling

All of the services are very light and can be deployed in a single ec2
`t2.micro instance`. It is running with **CPU utilization of 1-3% and
spiking to 40%** when there are a lot of new job posting ingested by
the system. **Memory Usage** is at **800+mb**

The system can scale horizontally, as the job sources increases we can
load balance it across different scraper.

If we want to increate request per second eg too many user is viewing
the system we can create a `master-slave` setup, and then load balance
it across multiple slave read only database.
