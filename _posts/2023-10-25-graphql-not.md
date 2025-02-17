---
layout: post
title: "GraphQL: What it is not!"
tags:
    - API
    - GraphQL
    - Mythbusting 
    - "Software Development"
    - "Software Architecture"
categories:
    ask-yourself
permalink: /blog/graphql-not
excerpt: >
    Tired of hearing the same old buzzwords about GraphQL? It's time for a reality check. In this post, I'll challenge what you think you know about it—GraphQL isn't a "graph query language", nor is it a database query tool, despite the name. Instead, it's a framework to help you design your own APIs. By busting myths and tackling misconceptions, even those held by seasoned developers, I'll strip away the hype and reveal what GraphQL really is. With a clear mind and a fresh perspective, you'll see how GraphQL can offer practical solutions to your existing problems without being as odd or intimidating as it's often made out to be.
---

Maybe you've heard of this thing called "GraphQL". Would you have any use right now for "GQL", a sort of "SQL" for graph databases? No? Well, if you did, GraphQL wouldn't be what you're looking fo - it's not that. Yet, the name "GraphQL" tends to create strong first impressions, and they couldn't be more wrong. I made the same mistake.

I'll write a separate post on what GraphQL is, but before we get there, it's important to clear up misconceptions - what it isn't. Believe it or not, even experienced GraphQL practitioners have misunderstandings that hold them back. So, even if you're sure that you know what GraphQL really is, I'd still appreciate you reading this. Maybe you'll discover something new - or maybe you'll teach me something.

Each section addresses a misconception I either held myself or have frequently encountered:

{:toc}

Let's dive in through.

# Graph? What Graph?

Take a look at the GraphQL specification - let's say the [October 2021 version](https://spec.graphql.org/October2021), which was the latest when I wrote this. Try to find the word graph in there. Ignoring the 269 instances where it appears as part of the name "GraphQL", here's what I found:

- It appears **once** in a comment within an example of questionable importance-something that could easily have been omitted: "user represents one of many users in a graph of data."

- It appears **once** in a sentence specifying what must ***not*** happen: "The graph of fragment spreads must not form any cycles including spreading itself."

Let's dig deeper:

- The word "edge" appears twice-once in "edge-cases" and once in "acknowl**edge**ment."

- Neither "vertex" nor "vertices" appears anywhere.

So, no-it has nothing to do with graphs. Sure, it can be used with graph-based data, but it's hardly unique in that regard. The spec itself describes GraphQL as hierarchical and mentions trees a few times. That's the closest you'll get. But don't call it [TreeQL](https://www.treeql.org) - that name is already taken, and its website looks suspiciously similar to GraphQL's.

In this context, graph seems to be used in the broadest sense- - verything in the world can be modeled as a graph. While GraphQL doesn't directly deal with concepts commonly associated with graph theory (as some languages do), it can be used effectively to handle graphs - specifically, acyclic ones.


# QL? What QL?

In the same specification, there's no direct explanation of what "QL" stands for. The phrase "query language" appears three times, always next to GraphQL, implying that GraphQL is, indeed, a query language. Okay, but what does query language mean to you? Do you associate it with a way to express search needs using a specific syntax? Should it include search criteria or conditions? Well:

- The word "*criteria*" doesn't appear in the spec at all.
- The word "*expression*" appears exactly once, in this sentence: "... therefore GraphQL lacks the punctuation often used to describe mathematical expressions."
- The word "*operator*" appears once, but not in the way you might expect. GraphQL has no data manipulation or comparison operators. You won't find anything that lets you write a = b, c < d, or e + f.

But guess what? The word "*condition*" appears 30 times. Even better, it's related to whether some data should be included or not. Sounds promising, right? But if you're thinking of SQL's WHERE clause - this isn't it. It's actually more like defining which "columns" to include, similar to the projection clause right after SELECT in SQL. And no, this has nothing to do with SQL. You won't find search criteria hidden under a different name anywhere in the spec - it simply doesn't exist.

Technically, GraphQL is more of a *projection* language than a *selection* language. The term "query" in GraphQL is overloaded and used in at least four different ways:

1. As a synonym for an API request - GraphQL is an API request language.
2. As a classification name for read-only operations (which you define, not GraphQL itself).
3. As the name of the standard output structure type - again, something you define; without your definition, it doesn't exist.
4. As a special keyword construct, acting as a sort of named singleton that gives clients access to whatever you put in (3).

But I'll dive into the details in another post.


# Database? What database?

There is exactly one mention of the word database, and it's merely a side note about how a particular implementation might rely on a database for asynchronous execution. The word repository doesn't appear at all.

The word storage does make an appearance, in this quote:

> GraphQL does not mandate a particular programming language or **storage** system for application services that implement it.

As for "*column*", the only references are to text column numbers, always alongside line numbers.

The letters "*table*" do appear, but only as part of words like execu*table*, prin*table*, repea*table*, represen*table*, s*table*, no*table*, and predic*table*. 

And row? It only appears within the word "th*row*" ( an error).

So… does that clear things up?


# GQL?

Some people try to shorten *GraphQL* to *GQL* and pronounce it "*jequel*". Not only does that reinforce incorrect associations with SQL, but it also happens to be the name of something entirely different: [GQL](https://www.gqlstandards.org).

Yes, GQL is a real thing. No, it's not GraphQL. Yes, it actually is a "graph query language" - as in SQL for graph databases. And yes, unlike GraphQL, it includes graph-specific concepts and terminology.


# Bulk updates and deletes and other scary stuff

Neither "bulk", "batch", "update" nor "delete" appear anywhere in the GraphQL specification. There's nothing about these operations. Instead, you'll find the somewhat intimidating word "mutation", but it's essentially like "query" - just a classification for operations you define yourself. It simply means the operation you're performing "has side effects" or "writes" something, as opposed to being "read-only." That's all.

Here's the kicker: GraphQL doesn't actually specify anything out of the box. It's completely empty. I mean, literally nothing there - zero, zilch, nada. No predefined operations, no assumptions about functionality. Sounds pretty useless, right?

Well, not quite. To make sense of it, think of GraphQL not as a single query language, but as two languages. One is a meta-language, used to create the other, which is a domain-specific language. Don't worry - it's simpler than it sounds. The key idea is that you have the freedom to design what it is and what it can do. GraphQL provides the structure, the scaffolding, and you build on that.


#  "It's probably binary or otherwise nasty"

GraphQL responses are strictly JSON, to a fault. That's it - it doesn't support binary data. GraphQL requests are plain text, often wrapped in JSON. While GraphQL can be exposed through other means, I'm only aware of implementations over HTTP. Simple HTTP GET requests can execute queries, and POST requests can do more. You can even curl a GraphQL query from the command line.

Both requests and responses are concise and efficient. If you're thinking back to SOAP with its "thick envelopes" and cumbersome structures, you can safely erase that memory. GraphQL responses will be smaller and more direct than, say, those of REST.


# "Current clients I have can't use it"

If a client can use REST APIs, they can use GraphQL - no exceptions. In fact, some people even confuse the two, thinking that GraphQL is simply a feature of a REST API. It's not. Every GraphQL API can be described by a [single Swagger/OpenAPI spec](https://github.com/aleksandarsusnjar/graphql-openapi-spec), so if you can interact with those APIs, you can use GraphQL too.


# "Must be hard to implement a server"

If you're using a framework for both, the mechanics of implementation are about as straightforward as building a REST API. There are [ready-made frameworks](https://graphql.org/code/) for just about any language you can think of. However, designing the API is much easier with GraphQL than with REST because the paradigms and constraints are quite different and align more closely with how the real world works. Not convinced? Try going through the exercises I described in my earlier posts on REST APIs. Start with [part 1](rest-what) before moving on to [part 2](rest-carving) and [part 3](rest-tug-of-war). Then, [learn about GraphQL](graphql-101) and work through the exercises again. It won't take long at all.


# "It's a niche thing, nobody is using it"

Check again. For example, take a look at the [GraphQL "Landscape"](https://landscape.graphql.org). Okay, that one might be biased. How about [Google Trends](https://trends.google.com/trends/explore?cat=5&date=today%205-y&q=RESTful,GraphQL,gRPC,OData,RESTAPI&hl=en-CA)? There's even a [documentary](https://www.youtube.com/watch?v=783ccP__No8) about it. Do your own research—just keep one thing in mind: "rest" isn't just "REST"; it has [many other meanings](https://www.thesaurus.com/browse/rest) (vacation, break, calm, pause, remainder, etc.), and plenty of people use the term [without really knowing what it is](https://cogito.susnjar.net/blog/rest-what).

Pay close attention to related queries in Google Trends. There's no ambiguity with GraphQL, but with "RESTful", the results lean toward "rambles" and "ASMR" even when restricted to the "Computers & Electronics" category. When unrestricted, they include "music", "sleep" and "meditation." And if you search for "REST" instead of "RESTful" you'll find it associated with Bruno Mars, Maher Zain, Pentatonix, Sefo, Lil Skies, Ludacris, David Guetta, and more (at the time I wrote this).


# "It's just a wrapper, will slow things down"

Very often, when transitioning from approach "A" to approach "B", the initial step is to make "B" a complete or partial wrapper around "A". This isn't unique to GraphQL - it applies broadly. However, it doesn't have to stay that way.

GraphQL can be implemented as a wrapper, but it doesn't have to be. It can start as one and then evolve into a native implementation, unlocking functionality and performance that weren't previously possible. Even in its wrapper stage, GraphQL can provide performance benefits by optimizing communication latency and payload size. It can also facilitate micro/macro service refactoring through functional composition.

So, if GraphQL starts as a wrapper, that phase is either temporary, beneficial, or both.


# "It's harder to secure"

Statements like this often come from people who have experience securing other technologies but haven't yet had the opportunity to work with GraphQL. Understandably, they may feel uneasy due to some of the misconceptions mentioned earlier - I had a few myself at first. The reality is that GraphQL more accurately reflects the real world than, say, REST. This allows for a more direct mapping of client behavior to GraphQL request and response structures, which, in turn, enables more precise access control and better detection of malicious requests.

I plan to dive deeper into this in a future post, but in the meantime, take a look at [Paniql](https://github.com/aleksandarsusnjar/paniql), [Apollo's complexity limits](https://www.apollographql.com/blog/announcement/backend/prevent-graph-misuse-with-operation-size-and-complexity-limits/), [TypeGraphQL Query Complexity](https://typegraphql.com/docs/complexity.html), and [Stellate's Complexity-based Rate Limiting](https://stellate.co/docs/graphql-rate-limiting/complexity-based).