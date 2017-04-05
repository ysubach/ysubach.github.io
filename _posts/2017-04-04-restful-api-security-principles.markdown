---
layout: post
title: "RESTful API Security Principles"
tags: [api, security, book] 
---

This is an excerpt from the "API Security" book I am working on. It explains
how high level security design princples can be applied to RESTful APIs.
Hope you find it interesting!

--------

Since RESTful API is an information system we can apply generic security design
principles to the design and implementation processes. In this section we'll
review key factors of information security together with main security design
principles and some extra principles applicable to our domain.


### Key Factors of Information Security - CIA

Confidentiality, integrity and availability also known as _CIA triad_ (or _AIC
triad_ to avoid confusion with the Central Intelligence Agency) are three key
factors used for high level security design. These factor are not directly
related to the computer security, they are known from other security related
domains (like military) and actually been used thousands of years ago.

#### Confidentiality

Confidentiality is a set of rules that limit access to the information. It means
data must be available for authorized users only, and protected from unintended
recipients during transit, processing or at rest.

#### Integrity

Integrity is the assurance that the information is trustworthy and accurate. It
ensures that data is protected from intentional and unintesional alterations,
modifications, or deletions. Important feature is reliable detection of those
unwanted changes, if they happen at any stage of data lifecycle.

#### Availability

Availability is a guarantee of reliable access to the information by authorized
people. This factor involves not only security aspect of the system, which still
is very important. It also impose availability requirements to the
infrastructure and application levels, combined with appropriate engineering
processes in the organization.

### Main Principles Explained

Three key factors are good and right, but there are too high level and difficult
to use directly when working on API security. We can use them to produce some
security design principles applicable to information systems in general, and to
RESTful APIs in particular.

Overview of the main principles is based on a widely cited paper
"The Protection of Information in Computer Systems" by J. H. Saltzer and M. D.
Schroeder (1975).

#### Economy of Mechanism
Keep design and implementation of the system as simple as possible. Complex
solutions are difficult to inspect and improve, they are more error-prone. From
the security standpoint minimalizm is a good thing. This is also true in
(almost) any other area of information systems development and usage.

#### Fail-safe defaults
Access to any resource (like API endpoint) should be denied by default. Access
granted only in case of specific permission. This approach represents
conservative design where protection scheme identifies situations when access
should be granted. The alternative, when protection scheme identifies conditions
when access should be restricted, is dangerous because any mistake in
configuration or implementation leaves system volnurable and it'll be most
likely unnoticed. Similar type of mistake with fail-safe defaults approach will
lead to access refusal, this is a safe situation and it'll be noticed by the
user immediately.

#### Complete Mediation
Access to all resources of a system should always be validated. It means, for
RESTful APIs, that every endpoint must be equipped with an authorization
mechanism. This principle brings security considerations to a system-wide level.

#### Open Design
Security design should not be a secret. It should be based on a well defined
security standards and protocols. In this case it's relatively easy to keep
small passwords and keys as a secret. Also this approach, where security design
or algorithm is separated from protection keys, allows many people to review and
contribute to this design without risk of being allowed to access the system. 

#### Least Privilege
Every user of the system should operate with minimal permissions required to the
job. This approach limits the damage caused by an accident or error related to
the specific user. For example, business analyst granted with read-only access
to the banking system, can not initiate bank tranfer even if this user account
will be compromised.

#### Psychological Acceptability
Security implementation should protect a system but not hamper users of the
system. For RESTful APIs, where typical user is the software engineer, it's
imprtant to make sure that API and it's security architecture is well documented
and easy to understand and use.

### Extra Principles

#### Minimize Attack Surface Area
Every system resource or functionality adds certain amount of risk to the
overall picture. This principle guides to minimization of points that can be
exploited by malicious users. This is similar to the _least privilege_
principle. For example, RESTful API which needs to provide callback API
endpoints to 3rd parties, needs to issues permission keys or tokens with a
limited scope. This minimizes potential attack surface area and damage in case
of security breach related to this 3rd party.

#### Defense in Depth 
Multiple layers of control make it harder to exploit a system. It means that
application of security mechanisms affecting different aspect of the system make
s it much harder to exploit. For example, SSH access to the server may require
specific private key. In addition, we can limit SSH access to the server to
several known IP addresses, this reduces probability of unauthorized access to
the protected resource.

#### Don’t trust services
Third party services or systems are often utilized by the developers. It's
important to treat these services as unsafe by default and implement all
relevant security measures. For example, RESTful API implementation can use
external partner's service to retrieve some data. This data need to be validated
and verified. Even if this seems like unnecessary step, it'll ensure system
security in case of breach related to this third party service.  

#### Fail Securely 
All systems, including RESTful APIs, are often fail to process transactions due
to incorrect input or other reasons. This principle require that any failure
inside the system should not overcome security mechanisms. Implementation logic
should deny access in case of failure. This is also stated by the _fail-safe
defaults_ principle.

#### Fix security issues correctly
Once a security issue has been identified, it is importnant to fix it in a right
way. It means developers and security experts need to understand root cause of
the issue, create a test for it, and fix with a minimal impact to the system.
Once fix is done the system should be tested in all supported environments and
on all platforms.
