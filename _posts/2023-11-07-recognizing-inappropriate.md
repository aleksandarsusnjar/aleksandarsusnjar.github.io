---
layout: post
title: "Security: Recognizing the Inappropriate"
tags:
    - API
    - API Security
    - REST
    - RESTful
    - GraphQL
    - Security
    - Authorization
    - "Software Development"
    - "Software Architecture"
categories:
    ask-yourself
permalink: /blog/recognizing-inappropriate
excerpt: >
    I challenge the conventional approach to security, especially when it comes to rate limiting and authorization. While many stick to outdated methods like IP or HTTP rate limits, I argue that these approaches fall short in real-world scenarios. Instead of just slapping constraints on your infrastructure, we need a deeper, more flexible solution that accounts for the human factor, tools, and evolving user behavior.
---

![Map](/assets/recognizing-inappropriate/bubbles-3.jpg){: style="float: right; width: 50%; margin-left: 1em;"}
There are some things I left out in my [earlier post on authorization](authorization-thought-scaffold). One of those things is how to handle situations where something might pass through checks when divided into sufficiently small pieces but becomes undesirable in a larger context that spans time. In these cases, it's less about leaking data or allowing actions that the apparent requestor is not authorized for, and more about detecting compromised requestors or mitigating DoS (denial of service) attacks.

Some may consider this separate from authorization, while others may argue that the recognition process is essentially a form of authorization decision. It all comes down to how you frame the question. For example: "Is this user authorized to make this many requests within the current time period?" or "Is this user authorized to perform such a large action all at once?" To me, it matters less what we call it and more how and where we manage, administer, and apply these constraints. But before we dive into that, there are a few things we need to figure out first…


# What is appropriate?

Let's step away from the servers and network infrastructure for a moment. Let's talk about what you expect from your users and other clients. If you expect them to do something, that "something" must be appropriate. Conversely, at some point, the "unexpected" might be seen as "malicious" or simply as a misunderstanding of expectations. But how do you form these expectations? Based on what? Here are a few examples to consider:

- How fast can a real human type, click, or use the UI?
- How many computers can a real human use concurrently?
- How quickly can a real human move from one location to another?
- How long can a real human work before needing rest?
- How many distinct resources do they need to access within a certain period? (e.g., clients by support agents)
- What are the tools those humans use meant to do? If they're doing more, it might suggest other tools are involved - proxies, gateways, middleware, etc.
- Do other practical limits apply, such as known low bandwidth or high latency? Speeding through these could raise suspicion.
- Are certain abilities implied, like being smart enough not to keep repeating the same or similar requests?
- Do known users, tools, or clients have a recognizable pattern in their requests, such as specific details and metadata?
- How frequently are certain errors expected? Too many "not found" errors could indicate random fishing for entities or resources.

Now, consider whether all your users and tools are the same. I'm assuming you'd like to address these examples, and the challenge is figuring out how to do so. I'll approach this from the perspective of "beyond immediate user input, everything is an API". Serving static web pages? That's the API browsers use to get HTML. Sending IP packets to another computer? That's the API one computer uses to ask another to handle those packets.

It quickly becomes clear that we have different levels of exposure, context, details, and abstraction with various types of APIs. Think about how difficult it would be to implement security covering all these factors at the IP packet level alone - without leveraging higher-level protocols. Payloads would likely be encrypted, so you'd be limited to headers and timing. You wouldn't even know ***who***, or ***what***, is at either end of the communication. As you move up the abstraction layers, your visibility increases, but your reaction time slows, and you stop recognizing communications that are invalid at that level. That tells you something…


# One constraint can't cover everything

Each level of abstraction handles its own details, which aren't present at other levels. Since we can only constrain the details we know, this means constraints need to be applied at their respective levels of abstraction, not above or below. For example, applying an IP packet rate limit to a specific link, perhaps between pairs of IP addresses, might help curb unexpected communication, but it says nothing about users or the type of data being transmitted. Such constraints must be generous enough to allow for all valid cases.

So, what happens if we take a higher abstraction and apply the same concept, a rate limit, to HTTP requests? First, we lose visibility into how many packets are involved in a request. There are many ways to intentionally put more stress on the system while still appearing as a single, valid HTTP request. Of course, we gain visibility into higher-level interactions, such as authentication, request details, and payloads. This makes it possible to express more specific expectations and constraints. But is an HTTP rate limit enough?

Not by a long shot. HTTP is still very chatty and disconnected from real-world activity. The days when a single human generated a single HTTP request are long gone, if not gone within the first few hours or days of the HTTP standard. Just think about how many CSS, image, script, and other requests are needed to display a single page. How many hundreds, sometimes thousands, of additional HTTP requests are made to gather extra resources after the initial REST API call (due to N+1 issues)? If you're not familiar with what I'm referring to, I recommend checking out my series on REST APIs: [what they are](rest-what), [carving states](rest-carving), and the [opposing forces](rest-tug-of-war), to name a few.

An HTTP rate limit is only slightly more relevant to the user use case than an IP rate limit. especially when it comes to REST APIs. REST APIs are not use-case APIs; they are resource state transfer APIs. A single use case often requires multiple state transfers. Not only will you find it difficult to determine a proper limit, but you'll also need to inflate it significantly to account for valid edge cases, leaving your system vulnerable to attackers who can exploit the extra room. Additionally, rate limits don't control the types of data being transmitted, although you could introduce multiple independent rate limits for each HTTP verb and endpoint (or REST resource). Even then, most REST API requests follow strict patterns and rigid output structures, which makes it difficult to perform request style/signature recognition or track output variations. You think you can? Well, that's probably not truly REST anymore. Be sure to check out that series of posts.


# Options

So, how do you map your real-world, use-case-based expectations into implementable constraints? Do you have a strategy?

1. Do you go to an even higher level of abstraction above REST and HTTP? If so, which one?

2. Or, do you use an approach that aligns use cases with HTTP better than REST does?

3. Perhaps, do you abandon HTTP altogether and switch to something that aligns better?

Answer (1) is theoretically obvious, but do you have a practical solution? I don't. That leaves us with two other choices if we want to get it right… and neither is strictly "RESTful." A well-designed gRPC API could be the answer for (3), but it may not be suitable for all contexts. There are multiple contenders for (2), with one of the major ones being Microsoft-initiated OData, and a minor contender I know of being TreeQL. For reasons that deserve a separate post, I wouldn't recommend either unless you're already working with them.

That brings us to the most popular, widely supported option that's taken the world by storm: [GraphQL](graphql-101). It allows clients to state exactly what they need for the use case, enabling almost ideal mapping between use cases and GraphQL/HTTP requests. As a bonus, the style and signature of requests become apparent and can be considered. Some who are only familiar enough with GraphQL to be dangerous might argue that a single GraphQL/HTTP request can demand too much. They're 100% correct. What's also 100% correct is that this becomes apparent and can easily be accounted for and mitigated. GraphQL doesn't suffer from the limitations of REST because it isn't rigid, and there are more tools available beyond simple rate limiting. You begin to see more than just nails when you have more than just a hammer. A huge ecosystem of tools is out there to help, including my open-source project, [Paniql](https://github.com/aleksandarsusnjar/paniql). You should also explore Apollo GraphQL contracts, complexity-based limits, and Stellate's complexity-based rate limiting.


# Continuous Administration

Coming full circle back to my opening analogy on authorization, let's consider where limits, constraints, budgets, and quotas should be expressed. How granular should they be? Per user? Per system? Per service? Why not all of the above? Who will maintain and update them as use cases evolve and as the needs and capabilities of people and tools change? Should these be manually embedded into your infrastructure configuration by the IT team? Could they even manage it, assuming they could keep up? Or would you benefit from more sophisticated tooling? Does your ecosystem already have such tools? Are you even aware that these tools exist? And would you consider moving away from legacy approaches to leverage them?

No, simply applying authentication and rate-limit constraints mechanically isn't enough, as these are relatively easy to bypass. We have the opportunity, and the tools, to do much better. We need to continuously manage our constraints to align with our evolving expectations for every player in the system, whether human, machine, or infrastructure. If we want to do this well, frequently, and with many parameters, we shouldn't rely on manual labor to update derived numbers. While the enforcement of constraints can and should happen in places where authorizations may not, what drives it - the actual limits and quotas, would benefit from authorization-like management. This should occur within the same tools, viewing both human and synthetic actors as security subjects and authorizing the entire chain (or graph, really) of "broken telephones" along the way.


# Now what?

Do you really believe that a single number, or just a few of them, used as limits can protect your system? I sincerely hope your answer is no more positive than "Maybe, just as a starting point." True protection against the inappropriate requires an understanding of real-world expectations, not just their "Nth derivatives." But understanding alone isn't enough - it must be applied. This means we need to express and enforce those expectations, likely across multiple levels of abstraction, from the lowest network layers all the way up.

You're going to need to think carefully about how your use cases map to REST APIs and decide whether it's time to move on (if you haven't already). There are other reasons why REST APIs are notoriously insecure, with the primary one in this context being that they tend to consume too much development time, diverting attention away from security considerations. Additionally, they make it nearly impossible to have generic tools that can help.


# Next...

See my earlier post about [aspects of data access authorization](authorization-thought-scaffolde) or series about [RESTful](rest-what) or [GraphQL](graphql-not) APIs.