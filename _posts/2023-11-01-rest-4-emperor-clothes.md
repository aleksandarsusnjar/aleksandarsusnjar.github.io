---
layout: post
title: "REST API (4): Emperor's [New?] Clothes"
tags:
    - API
    - REST
    - RESTful
    - HATEOAS
    - Mythbusting 
    - "Software Development"
    - "Software Architecture"
categories:
    ask-yourself
permalink: /blog/rest-emperor-clothes
excerpt: >
    REST is still hailed by many as the ultimate goal for API design, yet the reality is far more complex. I challenge conventional thinking by exposing the struggles involved in designing, implementing, evolving, and maintaining REST APIs. From the varying interpretations - ranging from purist to pragmatic - to the complexities of balancing competing needs and managing resource states, blindly adhering to RESTful practices almost always leads to inefficiencies and wasted resources. It's time to critically examine whether REST is truly the best path, or if it's just another set of invisible "clothes" that not only mask, but also create, the real challenges in API design. If you're not questioning REST's place in your system, you're missing the bigger picture.
---

Let's talk about ***your*** motivation for choosing the REST route. How much thought have you given to this decision? Have you considered how the approach benefits you and whether its limitations and constraints align with your needs without hindering them? What alternatives did you explore? Or did you simply follow what seemed to be the common practice or the only option available? Can you justify your decisions based on positive outcomes in your specific case, without relying on the opinions of others, including mine?


# What kind of "REST" is it?

Why does this matter? Because there seem to be at least three different interpretations of REST, and in a room with two people, you're bound to encounter some disagreement. It's crucial to know which arguments apply, as they don't always overlap. Here are a few examples:

- **"Original"**: The "Representational State Transfer" architectural style, as defined by Roy Fielding's PhD dissertation in 2000. It leaves many details up to the API designer and focuses on high-level goals and constraints. It doesn't necessarily involve HTTP, JSON, or any specific technology.

- **"Evolved"**: A stricter, opinionated extension of the above, applying lessons learned since Fielding's work. It focuses specifically on HTTP, JSON, HATEOAS, and the mapping of verbs and nouns. It emphasizes that effects should only be triggered by clients submitting self-sufficient intended states to the server that do not depend on previous requests - no RPC or action endpoints allowed.

- **"Pragmatic"**: A catch-all term for mapping action requests to HTTP requests (endpoints), where inputs/arguments are passed via HTTP headers, query string arguments, and request bodies, and results are returned in HTTP response bodies. If you can describe it using an OpenAPI specification, it's considered a "REST API".

While not exhaustive, the list above highlights a range of understandings of REST in the wild. It starts to resemble the story of "The Emperor's New Clothes" - everyone pretends to see something that isn't really there. I challenge you to dig a little deeper and investigate some "REST" APIs to see where they truly fit, no matter what they claim. Here are my observations:

- "REST" is an incredibly popular term, and it has other meanings (vacation, break, remainder, etc.) that may atrificially inflate its popularity.

- Most libraries, frameworks, and tools with "REST" in their names focus on the "pragmatic" understanding (3), leaving it up to you to apply the style and constraints from the original and evolved definitions.

- Many people dismiss the original and evolved "camps" and embrace the pragmatic/business approach from the start. This aligns with my observation that I've yet to encounter a "REST" API that doesn't violate one or more of the other definitions in some way.

- There are "purists" from the "original" and "evolved" camps who are frustrated by this situation. This is evident in countless articles, testimonies, and job interview questions aimed at determining whether candidates "truly know what REST is". Roy Fielding himself has published what could be seen as an outcry against the misappropriation of the term "REST".

If you're curious, I've covered this topic in more depth in an [earlier post - part 1](rest-what). What's important here is recognizing that there's a division and confusion surrounding this issue, and it will come into play soon.


# Getting the skills

Suppose your team doesn't have the necessary skills to design a REST API. What can you do about it? If you decide to hire people who already have the skills, how would you go about this, and what outcome should you expect, given the division and confusion mentioned earlier? How many candidates would meet your desired level of understanding, and how much would you need to retrain them? What new skills would they bring to the table in that case?

Let's dive deeper. Beyond knowing concepts that are adjacent to REST but not quite REST - such as HTTP, libraries and frameworks, Open API specifications, and the like - what else should your team learn to create effective REST APIs? If you're on the extreme pragmatic end, the answer might be "very little, perhaps nothing". This approach could make it easier to find the right workforce, but your API would likely resemble others only in the most basic ways. Newcomers would need to learn the specifics of your API, and that expertise wouldn't be transferable to other projects.

On the other hand, if you're in the less pragmatic camp and care about applying the style "correctly" (whatever that means to you), you'll be looking for more. Your team will need to develop the ability to [break down resources effectively](rest-carving) and [tackle opposing forces](rest-tug-of-war). If you haven't already, check out those posts - understanding these challenges will be crucial as we move forward. The people you want on your team will need to be familiar with these concepts and able to reason through them. As we continue, think about how much existing experience solving these challenges is transferable to other projects. How long would it take to hire someone with the matching skillset, or to train them to think the way you do?


# Designing the API

You now have your star team. They know everything they need to about the mechanics and constraints. They are ready to design the new, shiny REST API. They push forward, tackling the challenges at hand, [carving out resource types](rest-carving) and balancing their decisions against [architectural forces](rest-tug-of-war). These are not easy problems to solve, and there is rarely, if ever, an exact science to them. Even if perfect answers exist, they almost always have to change when something else does - and you know things will change, even before the API is "released". This makes it a somewhat artistic exercise, highly dependent on initial assumptions. The fewer assumptions they try to make, the harder and bigger the problem becomes.

How long will it take to complete a clean yet workable design? I'm not just talking about the initial sparks of inspiration, nor limiting this to the pre-implementation phase. All follow-up design adjustments should be included as well.

The answer depends on how strictly you adhere to your definition of REST. Those in the pragmatic camp will spend very little time, as they aren't following specific constraints and can simply add whatever they need, as they need it. Those in the other two camps will argue that this approach will cause problems later and will invest significant time in thoughtful design. But how long does that take? Honestly, I don't know. I have no evidence that anyone has ever done it 100% cleanly, so there's no one I could ask for better stats beyond my own experiences. Another way to put it: "longer than the time available".

At some point, every project resorts to some form of compromise, often by tunneling RPC. Has your experience been different? And if so, are you prepared to defend your design under the scrutiny of REST purists?


# Implementation

The skill is there, and the API specification is ready. You've got your libraries and frameworks lined up, and you know how to use them. So, how much more work is left?

If you're in the pragmatic camp, your smooth sailing continues. At worst, it's little more than mapping action (RPC) endpoints to your code, each dedicated to a use case as needed - simple or complex, your choice. This ease of execution is one of the biggest reasons the pragmatic approach is so popular. It gets things done quickly.

Did that make you cringe? Do you belong to another camp and believe this shouldn't be about action endpoints but about transferring current and intended states? If so, how much time will you invest in efficiently composing the current state for responses? Better yet, how much effort will go into validating and applying intended states (a.k.a. "updates" and "creates") while accounting for concurrent updates, state transition validation, authorization, and more? If this isn't immediately clear or seems trivial, I'd recommend revisiting the [earlier post](rest-carving) I mentioned.

Now, let's talk scale. How many resource types will you have to implement this for? If you're dealing with multiple APIs (e.g. microservices), include those as well. Then multiply. From my own past experience, a star developer (fully up to speed) could cover a resource type in about two weeks (adjusted for overtime) for the first iteration. More complex types took significantly longer. And that's without reimplementing the business or data access layers. With a few hundred resource types in play, that translated to about a decade of effort.

How much does that cost, both in terms of compensation and the lost opportunities to build other things?


# Evolution

![Map](/assets/rest-tug-of-war/topology-evolution-no-federation.gif){: style="float: right; width: 50%; margin-left: 1em;"}
Your API is live. Now come the performance complaints and feature requests, new clients. What can you do? Here's what's in your toolbox:

**Performance Issues Only**

- Optimize the implementation of existing endpoints without changing inputs or outputs.

- Leverage caching - but only if you can achieve a good cache hit rate on authorized or public content. Both clients and servers will need updates to manage the risk of stale data. And it's not just about displaying outdated information; it's also about the workflows that follow.

**Performance & Functionality**

- Enhance existing endpoints so clients can be more specific about what they need. This, however, negatively impacts caching and requires client-side code changes. Modifying request parameters or response structures introduces breaking changes - missing expected data or unexpected new data could cause failures.

- Tackle N+1 and other complex issues by designing and implementing new endpoints that overlap with existing ones. While this can help, it also makes the API less canonical, further complicates caching, and requires client-side updates.

How long does this take? The second group usually means back to the whiteboard, reopening earlier design discussions and all the challenges that come with them. If API versioning is required, and you've been through this before, you know it's not trivial. You'll either need to version the entire API, either keeping each version and all its endpoints available throughout its lifecycle, or find alternative solutions.

If you haven't dealt with this before, check out the Evolution vs. Compatibility section in my [earlier REST post](rest-tug-of-war). Now, how long do you think it will take to do this safely?


# Love affairs or boxing matches?

We live in an API-driven world. Everything we do depends on them. Yet, more often than not, our APIs don't have a fully known set of clients. We might start by developing and controlling the initial clients - say, a web UI and mobile app. But then come public APIs, third-party integrations, and enterprise customers with custom needs.

You might build BFFs (Backends for Frontends) for your own clients, but let's be honest - probably not for others. You can't predict their needs in time. And since BFFs are also meant to be best friends forever, each must be designed around its specific use cases, leading to its own optimized API. This means separate design, implementation, and evolution efforts. That's more work, again. Remember that decade-long effort I mentioned? That was just for an in-house web UI. Now, go ahead and multiply it by a factor that's not insignificantly greater than one.

But wait - what sits behind those BFFs? They work with the same concepts and data, so what do they rely on to get the job done? Who designs and implements that? How much does that cost?

Instead of maintaining separate BFFs, pragmatists take another route: merge everything into a single API with a variety of endpoints that collectively serve all needs. But both approaches face the same fundamental problem - supporting all client types without actually knowing most of them. That leads to assumptions, which often result in bloated states attempting to cover multiple use cases at once.

The outcome? Servers send increasingly large, bloated responses, while clients resort to rapid-fire requests to deal with lingering N+1 issues. Neither side is happy. The back-and-forth between clients and servers starts to resemble a boxing match more than a harmonious exchange. And this problem isn't exclusive to any particular camp - only the specifics of the struggle differ.


# What if?

What if we reassess API styles, approaches, and constraints and create our own? An aspirational goal could be to ensure that each use case requires only a single API trip (no more than one request per UI interaction) while maintaining efficient communication without unnecessary bloat. Since we don't know all clients upfront, they would need a way to clearly and comprehensively express their needs within those single trips, getting everything done at once.

By definition, this isn't possible under the purist view of REST. It also goes beyond what most pragmatists are willing to do, as it requires significant effort to design flexible ways for clients to express their requests.

That hasn't stopped people from trying. Many have done it, but their solutions remained proprietary and, as a result, never gained widespread adoption. Some have shared their approaches while continuing to call them "REST", with varying levels of success and complexity, such as OData and TreeQL (and possibly others). Others have abandoned REST altogether in favor of something like GraphQL.

Have you explored these alternatives? Are you sure you understand them? My experience suggests that misconceptions abound - some believe REST clients can't easily access these approaches, while others think they're inherently risky. If you design APIs, you owe it to yourself to see how these compare. Otherwise, can you confidently name a single benefit of REST that applies to your needs - and defend it?

# Next...

Now that we've contemplated what REST APIs are and/or are not, head to [GraphQL: What it is not!](graphql-not) to do the same for GraphQL. You owe it to yourself to have at least one more perspective.