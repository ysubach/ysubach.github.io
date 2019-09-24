---
layout: post
title: "App Ecosystems and Data Ownership"
tags: [data, container, api, security]
---

This blog post describes concept that allows companies to build application 
ecosystems while retaining control over sensitive user data.

The idea of _applications ecosystem_ or _applications marketplace_ is very 
powerful and already adopted by multiple companies (such as Apple, Facebook, 
Twitter, etc). But it is not widely used in industries that operate on sensitive 
user data. This applies to finance, healthcare, government, manufactoring and 
transportation sectors. To some extent social networks fall into the same 
category as users get more and more concerned about data they accumulate.

## App Store model

First, let's see how application marketplaces work using Apple App Store as an 
example.

For better understanding we need to take a step back to see how software 
applications operate in general. At a high level in order to produce some 
meaningful output or _end result_ we need three components:
* Hardware (computing resources)
* Data (input)
* Software (application code or algorithm)

In App Store model Apple provides access to _hardware_ and allow 3rd party 
application developers provide data and software. This pattern applies to 
majority of apps. There are some exceptions where Apple shares some data with 
3rd party apps (like user contacts) but that doesn't change overall picture.

Same model is used by cloud infrastructure industry (AWS, Azure, Google Cloud, 
etc) because essentially they allow clients access _hardware_ and use their own 
software to process/store their data. There are some layers on top of hardware 
(OS, databases, storage, messaging systems, etc) still _data ownership_ is 
responsibility of the client not the platform.

## API model

Social networks (like Facebook or Twitter) and many other companies use 
different approach to create platforms for 3rd party applications. Their main 
focus is on _data sharing_, so they use APIs to allow 3rd party apps access 
users' data they accumulated. In this case hardware and software part become 
responsibility of application developers.

This model allows easy data sharing but doesn't allow platform owner to retain 
_data ownership_. Essentially data is transferred to 3rd party apps and they are 
allowed to do anything they want (technically speaking).

We can summarize these two models using following conceptual diagram:

![App Store and API Models](/assets/blog/appeco-appstore-api-models.png)

## Domain platform model

Now let's talk about model that allows to overcome limitations of above 
approaches.

Proposed model allows to build platforms (for application ecosystem) with _data 
sharing_ features as well as guarantee of _data ownership_ (which means platform 
owner controls data and produced end result).

The key part is that this platform provides _hardware_ and _data_ and allows app 
developers to provide software component(s). More importantly platform retains 
control over _result_ (or output data) that will be presented to end user at 
some point.

Conceptual diagram for domain platform model:

![Domain Platform Model](/assets/blog/appeco-domain-platform-model.png)

The most important benefit is that adoption of domain platform model will 
__prevent sensitive user data leaks__ via 3rd party apps ecosystem.

## Implementation

Implementation of this platform requires development of new type of application 
containers that guarantees:
* specific data input/output conditions,
* code inside container doesn't produce any side effects.

This is very similar to pure functions concept from functional programming:

![Pure Function](/assets/blog/appeco-pure-function.png)

More on this later, implementation deserves it's own blog post(s).
