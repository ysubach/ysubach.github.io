---
layout: post
title: "Domain Specific Containers (DSC): Concept"
tags: [dataflow, container, security, cloud, mobile]
---

Domain specific containers (DSC) are building blocks of _domain platform_ model 
described in [previous post](/2019/09/21/app-ecosystems-and-data-ownership/). 
DSC allow applications to be executed in a safe way that guarantees data 
ownership will be retained by the platform.

## Overview

The general idea of containers implies some form of isolation and control. 
Modern _application containers_ provide means to control low level machine 
resources (CPU, RAM, disk, network). They are perfect tools for cloud 
infrastructure providers which allow them basically to rent out computing 
resources.

The core idea of _domain platform_ model is ability to retain data ownership. It 
allows company to rent out user's data. It means user's data can be retrieved, 
processed and presented to end user -- and since this is data rent it cannot be 
taken away. Domain specific containers (DSC) aimed to provide infrastructure 
that makes _rent out data_ model possible.

DSC designs requires understanding of applications structure and their 
requirements. These aspects explained below in more detail.

## Data flow models

In this section we analyze data flow between different components of system.  
This is high level overview that creates basis for next steps.

### One-way flow

The most simple model where application can only read data, process it somehow 
and present results to user.

![One-way flow diagram](/assets/blog/dsc-concept-pic21.png)

Blocks on this diagram.
- Primary Data Source (DS) -- contains original user data, applications can only 
read from it
- Application -- application code executed in DSC
- UI -- presents data to user

### One-way flow with application data source

Same as previous model with additional Application Data Source (DS).

![One-way flow with DS diagram](/assets/blog/dsc-concept-pic22.png)

Application Data Source (DS) is fully accessible for Application code running in 
DSC. It supports full set of read and write operations.

### Two-way flow with application data source

This model adds important application ability to receive user input from UI 
part.

![Two-way flow with DS diagram](/assets/blog/dsc-concept-pic23.png)

As result Application executed in DSC is able to:
- Read data from Primary DS
- Read data from Application DS
- Write data to Application DS
- Show results in UI
- Receive input from UI

## Deployment models

Here we need to understand how all application components will be deployed.  
It'll allow to define requirements for specific DSC implementations.

All models assume that Primary Data Source is deployed remotely (it's server 
based).

### Client only

![Client only diagram](/assets/blog/dsc-concept-pic24.png)

All components (excluding Primary DS) are deployed on the client device.

### Client with remote DS

![Client with remote DS diagram](/assets/blog/dsc-concept-pic25.png)

Similar to previous model. The difference is that Application DS is deployed 
remotely. It introduces some latency and access reliability concerns but enables 
applications to:
- persist data reliably,
- share data between application instances (only in some special case because 
general data sharing enables potential data leaks).

### Client with remote Application

![Client with remote app diagram](/assets/blog/dsc-concept-pic26.png)

This model assumes remote (server based) deployment of Application code running 
is DSC.

### Client-server

![Client-server diagram](/assets/blog/dsc-concept-pic27.png)

Combines previous two models by allowing part of Application to be deployed 
remotely and part on the client side. Application DS is remote but we can also 
allow some persistence on device as well (not shown on the diagram).

> From analysis of data flows and deployment models we can see that DSC 
> implementation required on the client side and on the server side. More 
> specifically for: cloud environments, mobile devices and web browsers.

## Communication channels

Let's review communication between system components in more detail.

![Channels diagram](/assets/blog/dsc-concept-pic28.png)

1) Data based communication (read only)
  - Application sends read request
  - Receives data response

2) Data based communication (read/write)
  - Application sends CRUD requests (create / read / update / delete)
  - Receives data response or status

3) UI based communication
  - Application sends UI components
  - Receives user input events (click, text change, key press, etc)

DSC implementations together with core platform should be able to support all 
these communication methods.

> To maintain platform integrity and data safety all communications between 
> components must be strictly validated at run time.

At this point DSC concept is described at a high level, some adjustments are 
still possible but they are not going to be dramatic. Further work need to be 
done on planning of actual DSC implementations and defining communication 
protocols / restrictions.
