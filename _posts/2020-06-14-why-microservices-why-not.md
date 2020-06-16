---
layout: post
title: "Why choose microservices (and why not to)"
tags: [microservices, distributedsystems]
---

Microservices are often seen as a default architecture for numerous new 
projects, as well as a target architecture for legacy systems migration. But in 
reality it's not always suitable approach. In this post we'll review advantages 
and drawbacks of the microservices architecture. 

## What are microservices?

The term "microservices" is a little bit vague, so people can understand it 
differently. For this blog post we'll reuse definition from Sam Newman's 
"Monolith to Microservices" book. Partial quote below.

> Microservices are _independently deployable_ services _modeled around a 
> business domain_. They communicate with each other via networks ... also have 
> the advantage of being _technology agnostic_.
>
> ... microservices expose the business capabilities that they encapsulate via 
> one or more _network endpoints_ ... They also _encapsulate data storage_ and 
> retrieval, exposing data via well-defined interfaces.

Let's break it down and list the most important qualities of microservices.

**Modeled around a business domain**

The most important and often overlooked property of a microservice. It has to 
implement some business specific function, in a given business domain. Quite 
often this property is ignored and that leads to sub-optimal design decisions. 
For example microservice called "UserService" is not a business function of the 
insurance broker business, so most likely you don't need this type of service. 
There is no one way to define services you need but remembering to model things 
around business domain helps to move in the right direction.

**Independently deployable**

Mean that you can update, fix and redeploy single service without affecting 
other parts of your system. This is a very useful quality of the microservices 
architecture. Modern tools make isolated deployments much easier and available 
for multiple environments. However this independence of services is not 
absolute. They still need each other to do their jobs and this creates 
_intersystem dependencies_ which are not affected by any deployment techniques. 
To improve services independence entire system must be designed with _loose 
services coupling_ in mind. Again it's not always possible but a very good, 
healthy principle to follow.

**Expose business capabilities via network endpoints**

It means that network based interaction is the only way to communicate with a 
microservice. This property is the core reason for  multiple advantages of
the architecture. But it also comes with significant difficulties or costs 
because of the nature of networks. Essentially microservices architecture is a 
type of _distributed system_.

**Encapsulate data storage**

This property requires microservices to be responsible for it's own data. Any 
databases it uses are hidden underneath the service interface and so they become 
an implementation detail. This principle improves service independence and quite 
easy to implement for microservice that follow business domain modeling 
approach. However it's often impractical to strictly follow this rule of 
database isolation because it can force a lot of extra work which outweighs 
benefits of database separation for some businesses or projects.

## Reasons to choose microservices

Let's analyze the reasons why we may choose microservices architecture. This is 
basically the list of their advantages which is split into _non-technical 
(organizational)_ reasons and _technical_ reasons.

### Non-technical reasons

**Improve teams autonomy**

Each team is responsible for one or multiple microservices can operate 
independently from other teams. The only constraint that remains is network 
interface(s) exposed by a microservice. Other things like technology stack, 
deployment methods can be chosen by teams more or less freely. The same 
applies to internal team organization, task management tools, testing approach, 
etc.

**Scale the number of developers**

Assuming that teams operate autonomously addition of new developers becomes 
easier. New people are able to adapt, learn about the service structure and 
become productive quicker because each microservice has much smaller scope in 
comparison with entire system.

**Reduced time to market**

Any changes or features planned in the _scope of a single microservice_ can be 
implemented, tested and deployed to production faster. The main reason is 
independence of services and teams which eliminate the need for complex and 
timely cross-team coordination.

**Cost-effective scaling**

Microservices as well as infrastructure components they depend on (like 
databases) can be scaled up or down independently. This gives an opportunity to 
identify and scale only parts of infrastructure that require more resources.

### Technical reasons

**Different technology stacks**

Oftentimes entire system is difficult or impractical to implement using unified 
development stack. In that case you'll be forced to split it into services 
connected via network.

**3rd party software integration**

This is a special case of the previous point. Here you may need to utilize some 
software that you can't control, it may even come without the source code. The 
easiest thing to do is to deploy it as a standalone service. If necessary add 
thin layer on top of it to make interaction with other services easier.

**Different scaling requirements**

Sometimes your system consists of parts that must be scaled independently. For 
example, stateless API instances can be easily scaled behind the load balancer. 
But stateful parts that handle persistent connections will require different 
approach.

**Different availability requirements**

Let's say the system consists of:
- front facing APIs,
- background workers, and
- scheduled tasks.

While APIs need to be online 24/7 some down time for background workers is 
tolerable. As for scheduled tasks / workers they need to be online for a few 
minutes a day (maybe hours in some cases). In a situation like that splitting 
work between isolated microservices allows to choose optimal development and 
deployment strategy. 

**Security requirements**

Isolation of microservices allow to satisfy security requirements that maybe 
present for some projects and industries (such as healthcare and finance). 
First, you are able to separate sensitive data from the regular data, or 
separate sensitive data that belongs to different clients. Also the isolation of 
services reduces potential attack surface (for each service) and potentially 
minimize exploit of break-ins.

## Disadvantages (costs) of microservices

Microservices architecture has a number of disadvantages. They are significant 
so it's important to understand them to be able to make an informed decision 
when _not to use microservices_.

**Network communication is slow and unreliable**

In comparison with internal function calls interaction with other services will 
be slower and it'll sometimes fail. This is caused by the nature of networks. As 
result you'll need to detect all possible errors, implement some handling for 
them (for example retry policy) and take in account networks latency.

**Eventual consistency**

Since every microservice runs it's own database the entire system is subject to 
eventual consistency. It takes time to update all databases and since this 
process is coordinated via network it's quite error-prone. Each service must be 
able to deal with these consistency issues.

**Transactions are hard**

This is an extension of consistency issues described earlier. Even if we design 
microservices to work with one transactional database regular ACID transactions 
are not possible. Assuming business logic requires cross-service interaction. 
It's possible to implement transactional semantics but this is going to be much 
more complicated and most likely each situation will need unique approach.

**Testing and troubleshooting is hard**

End-to-end testing, finding bugs and performance issues is complicated because 
the information is scattered across multiple services. Extra tools and 
techniques such as logs aggregation, metrics collection, distributed tracing are 
required in order to deal with these issues.

Overall any activity that requires understanding of the system as a whole thing 
becomes more difficult with microservices. In general this can be addressed by 
using advanced operational tools and by introducing some level of 
standardization.

## Conclusion

Microservices architecture has a number of serious pros and cons. Teams or 
engineers deciding to use it (or not to use) need to understand them in order to 
come up with decision optimal for a specific project.

In many cases monolithic architecture is more suitable. Especially for startups 
or projects in a prototype phase (when business domain is not fully understood). 

Sometimes hybrid approach makes more sense. In that case majority of the code 
developed in a monolith, but some parts (which really need to) are developed as 
a separate microservices.
