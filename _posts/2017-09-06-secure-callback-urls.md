---
layout: post
title: "How to Build Secure Callback URLs"
tags: [api, security, token, jwt]
---

Some API integrations require work with _callback URLs_ for proper data 
exchange. In this post we'll review some techniques that allow to use this 
integration feature securely. This is important for projects containing 
sensitive information such as personal, medical or financial records.

## Callback usage scenario

Let's say we are building an API or application that need a _3rd party service_ 
to perform some task. This task involves some heavy processing (that's why you 
outsourced it, right?) so the API provided to you is _asynchronous_.

Basically it means any task you submit to this service will be performed _later
in the background_. And as soon as it's done with processing the result will be 
sent back to you via __callback URL__. The diagram below shows how it works.

![Secure Callback Scenario](/assets/blog/secure-callback-scenario.png)

Our application/API has two components:
- _Sender_: creates the task and send it to external service, the _callback URL_ 
is part of the task
- _POST /results_: API endpoint for receiving result, it'll process result and 
save into our database

External service has two components as well:
- _POST /tasks_: API endpoint that accepts new task for processing, it places 
the task into internal queue
- _Processor_: performs actual task processing and submit result back to us via 
_callback URL_ (POST /results)

_Important feature_ of this scenario is ability to specify callback URL 
_dynamically_ as part of the task submitted to the external service. Techniques 
described below depend on this ability, so they may not be suitable for each API 
in the reality.

## Callback without token

Building callback URL is a straightforward task, we can use something like:
```
https://api.myapp.io/v1/results/{id}
```
where `https://api.myapp.io/v1` is an API root URL, `/results` is our callback 
endpoint, and `{id}` is a result identifier we are expecting.

This approach works but impose an obvious _threat_: an attacker who knows about 
this _callback URL_ can easily __rewrite all results__ we have by replacing 
`{id}` and sending requests to us. It means this approach is not suitable for 
projects where security is an important factor.

## Callback with static token

Let's introduce `{token}` component as part of the callback URL:
```
https://api.myapp.io/v1/results/{id}/{token}
```
Token in it's simplest form is a random string associated with an application 
`user`, so for every request coming in we can find appropriate user in our 
database and authorize access to our resources.

Assuming that `token` is accessible to _legitimate users only_ we can say that 
this approach is _safe enough for some projects_.

However this is not enough is we consider another potential threat: _data breach 
of the external service_. If this happens an attacker can retrieve our complete 
callback URLs from the database or log files. This, again, grants unrestricted 
access to our _results records_ and, potentially, to _all resources_ of users 
exposed to the attack.

## Callback with JWT token

The improvement to our callback URLs security is possible if we change the way 
of `token` generation. [JSON Web Token (JWT)](https://jwt.io/) is an open, 
industry standard method for representing claims securely between two parties.  
Just to be clear, callback URL structure remains the same:
```
https://api.myapp.io/v1/results/{id}/{jwt-token}
```

The main advantage of JWT in our case is ability to securely restrict 
permissions granted to external service with the token. This significantly 
reduces the _attack surface_ as opposed to user-based or static tokens 
(associated with user only).

![Secure Callback Attack 
Surface](/assets/blog/secure-callback-attack-surface.png)

### Tokens limited by path and resource

When we generate a JWT based token following information must be included:
- `_id`: user identifier
- `path`: allowed API endpoint (or path)
- `res_id`: allowed resource identifier

Each request to the callback URL must be accompanied with JWT based token which 
we need to validate to make sure requester is allowed to send us result data.  

Now in case of 3rd party service data breach the amount of resources exposed to 
attacker will be much smaller.

### Tokens limited by time

To further reduce abilities of potential attacker we need to use another 
dimension (not shown on the attack surface diagram) which is _time_. We can 
limit lifetime of each JWT token, this is a standard feature supported by [JWT 
libraries](https://jwt.io/#libraries). It means that after _certain period of 
time_ token becomes invalid and can not be used by anyone.

The concrete time-out value need to be carefully chosen depending on 
characteristics of the external service. Token lifetime must exceed normal 
processing time guaranteed by the service. Otherwise there is a risk of denying 
legitimate requests with valid results if processing took more time than allowed 
by our JWT token.

That's it! Hope it was useful.
