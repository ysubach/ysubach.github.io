---
layout: post
title: "Monolith vs microservices: doing it the right way"
tags: [microservices, distributedsystems]
---


Monolithic and microservices based architectures are wide spread across our
industry. Former is portrayed as an old approach, while latter as relatively new
and more preferred in modern software engineering. This post is an attempt to
shift discussion from "monolith vs microservices" topic by addressing the core
problem. And using this knowledge outline balanced approach to building software
architectures "the right way".

## Define the problem

To better define the problem (or problems) many teams are facing daily let's
review some imaginary project which represents a typical implementation of a
monolithic architecture.

In this project each module has vague responsibilities because they are all
created and modified as-needed. Therefor communication between modules is
chaotic and it all looks similar to the next picture. Bear in mind real projects
are much more complex because the have more than a few modules. It is a
_Spaghetti Monolith_ architecture.

![Monolith spaghetti](/assets/blog/mono-vs-micro-pic1.png)

At some point work on this project becomes very challenging. Bug fixes,
improvements and any changes take forever. Team members are confused and unable
to work efficiently. There are many other problems that it would take another
article to list the all and review. So we can stop here and talk about possible
solutions to this mess.

Solution that is often recommended in this situation is to break down monolith
and _migrate to a microservices architecture_. This is a lengthy process with
it's own set of challenges and solutions. There is no point in discussing all
these details because we need to skip to the end of this process and review the
result. For our imaginary project it will look like the following diagram.

![Microservices spaghetti](/assets/blog/mono-vs-micro-pic2.png)

Every module is transformed into an independently deployable service. Database
can be split into several independent data stores (not part of this diagram).
While this independency can provide some benefits overall situation became
worse. In addition to Spaghetti architecture that once was confined in a
monolith migration to microservices introduced drawbacks of this architecture
(discussed [here in more details](/2020/06/14/why-microservices-why-not/)).

> Straight migration to microservices architecture is not a good solution
> because it does not address the core problem: __complexity__.

## Pragmatic approach

Let's come up with a bit more pragmatic approach the architecture evolution.
Since complexity is the core problem here the logical course of action would be
to deal with it first.

Overall, there are two steps:
1. Resolve complexity issues
2. Create microservices as needed

Ideally they need to be performed sequentially. But this is hard to accomplish
in real projects, so they could be done in parallel (almost). For example, part
of the project could be reorganized to have better structure (step 1), which
makes it a good candidate for becoming an external service (step 2). This can be
done while similar reorganization is happening for other parts.

In more detail, step 1 is a transition to a _Structured Monolith_ state. At this
point many issues rooted in previously discussed Spaghetti architecture will be
resolved, or at least significantly minimized. Project schema will look like the
following diagram.

![Monolith structured](/assets/blog/mono-vs-micro-pic3.png)

Next step 2, migration to a microservices architecture is _completely optional_. 
For some teams or projects it might be enough to create a few external services. 
Anyway it's going to relatively easy transition because complexity problem was
addressed before. For example, it may look like the this.

![Monolith structured plus service](/assets/blog/mono-vs-micro-pic4.png)

The most critical part of this pragmatic approach is to _address complexity
problems_ before scaling the system to be microservices based. In this case
teams see earlier results during transition to a Structured Monolith (step 1),
and further transition to microservices (if it is necessary) becomes more smooth
and gradual.

## Dealing with complexity

Even if approach described above looks logical and clear, one question still
remain. How to move from Spaghetti mess to a Structured architecture? There is
no simple answer as far as I know but we'll review one wrong way and one right
way of moving to a Structured state.

### Layered architecture

Migration to a layered architecture sometimes performed to deal with a
Spaghetti monolith issues. In that case (typically) application gets additional
abstractions like "API layer" and "DB layer" in one form or another. This is not
solving complexity issue at all. Quite the opposite it becomes worse. Check out
the following diagram, complexity is just hidden beneath layers.

![Monolith spaghetti plus layers](/assets/blog/mono-vs-micro-pic5.png)

### Lego architecture

This architecture allows to address the system complexity issues by building
modules that have:
- Narrow interfaces (or low cost)
- High complexity which is hidden (or depth, or value)

More precisely this is not an architecture pattern, but a set of requirements
for each module. We can say:

> Any system consisting of Lego-style modules implements Lego architecture.

Few more details about modules. Following diagram shows some examples of "good"
and "bad" modules.

![Lego style modules](/assets/blog/mono-vs-micro-pic6.png)

Some comments about each example.
* Bad - shallow: wide interface, low depth; must be avoided.
* Very bad - too complex: wide interface, high complexity; must be avoided,
  essentially this is a Spaghetti monolith.
* Good: narrow interface, high complexity; this is the most desired module type.
* Okay: narrow interface, medium complexity; this module time is the most common
  in practice, can be used widely.
* Useless: narrow interface, low complexity; can be easily discarded.

Overall the system must be designed to have majority of "Okay" modules and some
"Good" modules. In reality it is not always practical to have all modules with
high complexity (or high value).

As a final note, building well structured systems is a key to successful
completion of software projects. Using this knowledge you will be able to
navigate "Monolith vs Microservices" dilemma in the most efficient way.
