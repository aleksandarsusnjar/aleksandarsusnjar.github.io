---
layout: post
title: "Collaborating Services by Functional Composition"
tags:
    - SDLC
    - API
    - REST
    - RESTful
    - GraphQL
    - Mythbusting 
    - "Software Development"
    - "Software Architecture"
categories:
    ask-yourself
permalink: /blog/functional-composition
excerpt: >
    Tired of fighting service fragmentation with bloated gateways and endless client-side juggling? In this post, I dismantle the myths of orchestration, expose why conventional strategies often collapse under real-world complexity, and make a case for a smarter, functional approach that's efficient, scalable, and already in your toolkit. Forget the chaos of overengineering - there is an alternative that correctly distributes ownership, eliminates inefficiency, and simplifies collaboration without adding layers of hype.
---

*Small streams make large rivers*

Do you have existing services that should collaborate but currently don't? Or are you considering breaking down a monolithic system? Perhaps you're starting a new project and want to set it up correctly from the outset. Whatever your situation, several key questions need answering:

- How should functionality be divided among these services?

- What strategies can be employed to minimize and manage the impact of changes on client synchronization with evolving services?

- How can development efforts and time be reduced to launch functionality sooner?

- What methods can be implemented to address errors in decision-making effectively?

In this "discussion", we will focus on the practical aspects of the decisions made to address these questions.

Your services will provide APIs that various clients will use. Today, it's common for each service to be accessed by multiple client types, such as web applications, mobile apps across different platforms, and other backend services, including automated processes. You may also expose these services and their APIs to your customers, enabling better automation and integration with their systems. Each client category might need a tailored version to suit its specific needs. Managing these diverse clients, many of which are beyond your direct control, can be challenging. With that in mind, let's dive into addressing our key questions.


# Who should see what?

Let's begin by considering the approach where each service directly exposes its API to clients. For now, we'll skip over basic API gateways or fundamental network infrastructure, like load balancers. Imagine we start with RESTful/HATEOAS APIs. These can easily manage load distribution to dedicated services by using full link URLs in responses, referring to content in other services. However, this approach brings its own set of challenges.

- Should authentication and authorization be standardized?

- Should every client be granted direct access to every service?

- Should clients adjust their behavior based on the endpoint address or media type?

- How does one update all clients when services are refactored (split or merged)?

Some have tried to address these challenges by creating *umbrella* gateways that *orchestrate* underlying services. While this umbrella can shield clients from internal service changes, it essentially acts as a monolithic service, requiring ongoing maintenance. To meet the diverse needs of numerous clients, the umbrella would either have to address each use case directly (which adds complexity) or be adaptable and responsive to clients' needs (which also introduces complexity).

Now, suppose those underlying services evolve and expose new functionality. It won't immediately be available to clients until the umbrella system is fully updated. Creating and maintaining this umbrella requires expertise that may be as demanding as, or even greater than, that needed for the individual services themselves.


# Consistency?

Should services be uniform and consistent in fundamental aspects, eliminating the need for data transformation between one service's response and another service's request? The "shared nothing" approach suggests that clients must handle the translation between services, whether these services are part of a unified umbrella system or not. This might not be a major issue if there is only one client. However, when dealing with multiple clients, it leads to duplicated efforts, requiring the reimplementation of basic access to all underlying services and the necessary logic to connect them for each client type. In this case, the "shared nothing" concept becomes problematic.

Establishing basic access and common connections between services is crucial. Creating consistency in authentication, authorization, data types, structures, and links across all services doesn't need to slow progress. In fact, it can simplify and streamline the work for everyone involved.


# Performance & Efficiency

How many network interactions will complex, diverse clients need to complete a use case? We need to consider the network bandwidth, sizing, and locations for each service they interact with. If there are only a few services, and if the clients are physically close (in networking terms) to those services, this may not sound like a problem. However, even in such cases, the fragmentation of requests makes it difficult to optimize efficiency by bringing processing closer to or into the service(s). How could we optimize this, in theory? Perhaps by:

- Not wasting time on what isn't needed.

- Doing everything that's needed while there, without requiring extra interactions.

- Enabling the ability to do the above for multiple services in the same trip.

Truly RESTful designs that follow an "all or nothing" approach pose challenges unless we redefine a "resource" to be precisely what is needed for each specific use case, as known during the development phase. But as discussed in earlier posts, it's not that simple anymore.

In response, some have implemented ways for clients to specify what should be excluded from a default state representation. Since clients can't exclude data they are unaware of in newer versions, that data must be excluded by default. This mixture of defaults results in convoluted combinations of "include this" and "exclude that", which only increases complexity.

The most capable approaches empower clients to dynamically combine multiple requests within a single trip at runtime, avoiding the assumption that they know every possible combination during the service's development phase. Would you implement this? How expensive would it be to develop and maintain? What format or representation would you use for this "batching"? Must the contained operations be entirely independent, or can they depend on each other's output to avoid extra network trips? In most scenarios where batching could help ordinary clients, these operations tend to be "dependent". Let's think this through a bit more...


# Perspective

Let's try to describe what we want:

> The clients should have access to all the services. They should remain unaffected by the (re)factoring of those services as long as they are still there. Instead of having to transform data between service calls, we want the outputs of services to be directly usable as inputs for other services. We don't want to be forced to make another request for related processing that can easily be done while we're already there. In fact, clients should be able to request all the processing they need, not just one task or pre-imagined combinations of tasks.

Now, let's rephrase that with a new perspective:

> The clients should have access to all the ***functions*** of all services. They should remain unaffected by any (re)factoring of ***functions*** of those services as long as they exist. Instead of having to transform data between service calls, we want the *outputs* of ***functions*** to be directly usable as *inputs* for other ***functions***. We don't want to be forced to come back for related ***functions*** that can easily be done while we're already there. In fact, clients should be able to request all the ***functions*** they need, not just one or pre-imagined combinations.

Does this sound familiar? Does it remind you a bit of functional programming? What if we made our API fully functional, from top to bottom – everything as a function, including access to the smallest piece of data? We could clearly define each function, its inputs and outputs, while putting in the effort to maintain data and type consistency.

With such an approach, clients could pass any number of functional expressions to a service for "evaluation". This service doesn't need to implement or even fully understand all the details of any requested function – it just needs to know who to call if it doesn't directly support it. Generic "umbrella" services become not just possible but feasible. They would simply need to discover which service supports which functions. Since this discovery process could be dynamic, any new function exposed by any service would be available as soon as it's deployed, with no additional coding required for the umbrella service.

Too bad this doesn't exist, right? I mean, creating a brand-new standard, community, and tools around it takes a lot of time, effort, and investment. It's not like this hasn't been tried.

Well…


# What if I told you this already exists?

![Map](/assets/functional-composition/topology-evolution.gif)
Yep, it does. And it's incredibly popular, very powerful, with a huge community and countless tools for a wide range of programming languages. And yes, there are multiple generic "umbrella" gateway implementations for you to choose from, both open-source and commercial. According to Google Trends, this approach is significantly more popular than RESTful or other alternatives.

Even if you're familiar with it, you might not realize exactly what I'm referring to, as the name implies something quite different and, for some, sounds unnecessarily intimidating. The name doesn't quite reflect what it is, and the terms in its specification don't match the ones used in this document. But it's almost a perfect match. I'm talking about GraphQL, and I'll be writing more about it and the surrounding confusion.

If you're in a rush to find your next umbrella gateway, here are some options (listed alphabetically):

- [Apollo GraphQL Federation](https://www.apollographql.com/federation){:target="_blank"} (with "Router" being the newer product and "Gateway" the older one)

- [Bramble GraphQL Federation Gateway](https://movio.github.io/bramble/#/){:target="_blank"}

- [The Guild's Mesh](https://the-guild.dev/graphql/mesh){:target="_blank"}

- [Nautilus Gateway](https://gateway.nautilus.dev/){:target="_blank"}

- [Tyk API Gateway](https://tyk.io/graphql/){:target="_blank"}

I apologize if I've missed any. I don't intend to give preference to any particular one.


# Next...

Continue to my post about [Divide & Conquer in Software Development](divide-conquer) for related but higher-level discussion.

Alternatively, learn about GraphQL by [first seeing what it is not](graphql-not) before getting to a [provocative '101' lecture on how to speak it](graphql-101).