---
title: Thoughts on green fields projects
tagline: If creating a green fields project is hard why does everyone want to work on a green fields project?  Is it because they wish to create a mess that they can call their own, to play with new technology and new techniques, or is it because they wish to go through the excruciating pain of creation?
date: 2016-04-12
release: no
keywords:
  - DDD
  - BDD
  - Repository Pattern
  - Green Fields
  - Brown Fields
---

I wanted to write a note on how to structure UI tests using Selenium.  The reason is because I often see an adhoc approach to UI testing which, over time, results in the UI tests decaying, teams losing faith in the tests, discarding the tests only to rediscover the importance of automated tests and then to restart this cycle by creating new tests.  To illustrate how to structure I wanted to create a sufficiently complex application so that the UI tests would be meaningful and the layout of the testing code could be sufficiently illustrative.  After formulating my business problem I set to work building the domain logic and, quite quickly, ran aground.

Roughly speaking this was my approach:

- I first created a use-case model and a domain model.  The domain model was sufficiently attributed and the relationships between domain elements was accurate.  It was good and I was pleased.
- I then prioritised my use-cases, created failing tests and started spitting out code.  The code was clean, the architecture was hexagonal and persistence was through a ```Repository```.  It was good and I was pleased.
- Apart from jUnit and Cucumber I had yet to reach for a framework and I reveled in the simplicity of my code. It was good and I was pleased.
- Progress was fantastic - I had one use-case left.
- I then hit a wall - my final use-cases was to update three entities simultaneously and I discovered that, if my behavior was properly dispersed into the entity classes that I needed a new concept - a transaction - a database transaction.  All of my neat abstractions and simple ```Repository``` fell on the floor.  The only way to introduce immediate consistency was to float a transaction into the business domain.  I could fudge the transaction by introducing a business concept called a unit of work or something else equally artificial to describe immediate consistency but, factually, this concept did not exist in the business domain that I was working with.
{: .default-ul }

It was at this point that I faced with a question that we often encounter - do I force the code to fit into my architecture and accompanying patterns or do I rethink.  In this case I stopped and rearchitected the entire domain replacing the ```Repository``` with an event based solution.

This experience got me thinking about Brown Fields projects where the architecture and patterns are typically repeated versus the promise of Green Fields where the architecture and patterns can be selected.


## Brown Fields and Green Fields ##

When I interview, almost without exception, everyone wants to work on green fields projects.  I try and understand why they want to do that because I find Brown Fields projects more of a challenge when rehabilitating a code base.

When I ask them to explain why they want to work on Green Fields the typical answers are:

- "I don't want any technical constraints",
- "I want to work with new technology", or
- "I want to use a new practice" - this could be a different architecture, xDD, devops, read-only infrastructure or anything that represents contemporary thinking.
{: .default-ul }

What I find illuminating in this conversation is when I ask what makes Brown Fields unattractive and what are they going to do on their Green Fields project to ensure that it does not become Brown Fields I get, without exception, one of three responses:

- I don't understand the question,
- I don't know, or
- We could ...
{: .default-ul }

It does not matter how I rephrase the question I never get a view as to how one would attack a Green Fields project.  As the last response indicates, at best, I get suggestions.


## Setting up a green fields project ##

What would I do to setup a green fields project?  A couple of the thoughtless obvious things:

- Get a [walking skeleton](http://alistair.cockburn.us/Walking+skeleton) working.  A walking skeleton is  wonderful in that it allows one to start getting feedback very early on from those that are going to use the application.  Truthfully a walking skeleton will mean different things to you depending on what your application is going to do.
- Get your application into version control and onto a CI server.  Over the last number of years I remain surprised how often I run into folk working on applications that are not using version control or, if they are, they are performing irregular and large check-ins.
- Implement BDD with Cucumber and TDD with xUnit - no exception.  I have found Cucumber to be wonderful in bridging the communication gap that exists between different communities and offering a functional scope into what must be built.  I do find that applications that I have written with BDD tend to be behaviourally richer when compared with to that I have written without which tend to be statefully richer.
- Use a hexagonal architecture - no exception.  I am sure that there will be circumstances where hexagonal does not make sense but those are on the edges of the bell curve.  Any application that interacts with one or more external resources will need to have that resource placed into a port and adaptor to protect the domain from vocabulary and abstraction leakage.
{: .default-ul }

A less thoughtless obvious thing is how to bind components together.  My recent experience from using the ```Repository``` pattern has been an eyeopener to me - it is never nice when a silver bullet is ineffective.  I am now exploring [DDD](https://www.goodreads.com/book/show/179133.Domain_Driven_Design) and [CQRS](http://martinfowler.com/bliki/CQRS.html).  DDD has already changed my vocabulary and how I think about software in much the same way that [Ivar Jacobson's](https://en.wikipedia.org/wiki/Ivar_Jacobson) use case models and the [entity/control/boundary pattern](http://www.cs.sjsu.edu/~pearce/modules/patterns/enterprise/ecb/ecb.htm) did.  CQRS looks very promising in enabling the building of scalable and adaptable software in a manner that appears to be better than any other architecture that I have encountered.
