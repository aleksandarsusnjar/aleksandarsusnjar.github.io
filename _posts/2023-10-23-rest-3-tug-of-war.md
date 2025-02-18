---
layout: tagged
title: "REST API (3): Tug of War"
tags:
    - API
    - REST
    - HATEOAS
    - Mythbusting 
    - "Software Development"
    - "Software Architecture"
categories:
    ask-yourself
permalink: /blog/rest-tug-of-war
excerpt: >
    REST API design isn't as straightforward as it may seem - there's a complex tug of war between competing factors like client needs, performance, security, and maintainability. While many treat REST as a simple, one-size-fits-all solution, the reality is far messier. From managing resource states and ensuring performance to addressing failure and evolving with clients, every design decision pulls in different directions. I explore alternative approaches, such as dynamically adjusting resources or reimagining them for specific clients, and challenge the conventional wisdom by questioning the assumptions others often settle for. By diving deep into these complexities, I urge you to think critically about the real-world challenges of API design and the profound impact your choices have on the system's functionality and longevity.
---

![Map](/assets/rest-tug-of-war/tug-of-war.png){: style="float: right; width: 50%; margin-left: 1em;"}
Every engineering design effort is an exercise in juggling opposing forces. In [part 2](rest-carving), we explored how to carve the entire state into resources. This time, we'll dive deeper into the key considerations for designing good APIs based on transferring the "current or intended states" of resources. These include:

- Supporting varied, and possibly unknown, clients and use cases
- Handling transactions: ensuring multiple operations succeed together or not at all
- Optimizing runtime performance and caching
- Resilience to failure and graceful degradation
- Authorization and security
- Who's the boss: where does the business logic live?
- Ensuring evolution and backward compatibility
- Tightness of coupling
- Overall effort required

Let's get into it!

# Client variation

![Map](/assets/rest-tug-of-war/topology-evolution-no-federation.gif){: style="float: right; width: 50%; margin-left: 1em;"}
In [part 2](rest-carving), we explored just how non-trivial it is to determine which types of resources to expose, even for a single, known client in a single, concrete example. The approach we choose has profound effects not only on communication but also on what code needs to be written, how much of it, and where it resides. The differences in code size and complexity are significant and cannot be ignored. 

Now, imagine considering a different type of client with different needs. Do you think you would make exactly the same decisions about how to carve those resources? I doubt it. Some choices might remain similar, but others could shift - certain details might become unnecessary, while new ones could prove essential.

I've observed several approaches to address this challenge, in no particular order:

- **Fusing resources and data so that all potentially needed information is always present**. Clients are expected to ignore any extra data they don't need. This results in larger state representations but may reduce the number of requests required for a particular use case.

- **Splitting resources into fine-grained ones, where each is either tailored for a specific client or reusable across multiple clients**. This keeps state representations smaller but increases the number of requests needed to gather the full picture.

- **Reimagining resources to best suit each client**. This often provides the best experience for consumers but requires careful attention to consistency and typically involves more effort.

- **Dynamically adjusting resources based on the client's request**. Additional control data is sent with each request to specify what should be included, embedded, or excluded from the current or intended state. However, this complicates caching since different combinations of included and excluded fields alter the response, caches (shared, private, or local) must account for variations, even when the differences are as subtle as the ordering of parameters (e.g., "name, email" vs. "email, name").

Each approach has trade-offs. Which one do you lean toward? Here are some key questions to consider when evaluating each of the approaches above:

1. **Impact on Development** - How much does the server need to change? What about the clients? How much effort does this place on developers? Are you planning to introduce Backends for Frontends (BFFs) to tailor responses per client?

2. **Server Workload & Efficiency** - How much processing does the server need to do? How much of that work ends up being wasted because a particular client won't even use the extra data? What's the overhead of basic request handling - like reauthentication and reauthorization, before any meaningful processing even starts?

3. **Communication Overhead** - How much extra data is being transmitted? What impact does this have on latency and overall response time?

4. **The N+1 Problem (and Worse!)** - Does this approach alleviate or worsen the issue where fetching one resource requires issuing N additional requests to get the missing details? Could it be even worse - requiring M additional calls for each of those N? And is unnecessary data being sent in every communication?

5. **Handling Transactions** - How does this approach affect the ability to perform all-or-nothing operations when multiple changes need to be made together?

6. **Coupling Between Clients and Servers** - How tightly coupled do the client and server become? Does this create long-term flexibility or introduce fragility?


Here are a few bonus questions for the dynamic approach:

1. **Standards or DIY?** - Are you aware of any standards for this, or are you planning to wing it yourself? Hint: I know of a few, though they're not strictly REST: OData, GraphQL.

2. **RESTful or RPC?** - Does your approach, or any relevant standards, remain RESTful, or does it start to lean more toward RPC (Remote Procedure Call)?

3. **Existing Tools or Building from Scratch?** - Is there existing code, a library, or a framework that can help you implement this approach efficiently? Would you need to apply this solution consistently across each resource type, or is there flexibility in how you apply it? Why or why not?

These are all important to think through. Each decision could impact efficiency, scalability, and the future evolution of your API.
Anyway, suppose that you made your decision, built it, and it works beautifully. Everyone's happy - so happy, in fact, that your API is a market success. Because of that another client appears, soon followed by ten more. Even more follow, each evolving in its own way to the point where you can't control or fully pay attention to all of them, even if you had the time and permission to do so. Some are custom mobile apps, while others are created by your enterprise customers as part of evolving integrations with their different systems.

What do you do now? Are you at risk of this? Does it even matter? Does it help to start on the right foot, or is it OK to keep rebuilding from scratch or build "BFFs" (backend-for-frontends) for everyone? Do you think "the right foot" can truly exist, or is it better to adopt an approach flexible enough to accommodate all kinds of "feet"?


# All or nothing: Transactions

Let's say you want to move funds from one bank account to another. That's one action affecting at least two resources. Here are the some approaches people might take:

1. Updates all of them together, transferring the states of all affected resources at once.

2. Update all of them individually but within an extra context that controls their joint application, which means you'd need to have a concept of that "context" and the ability to finalize it and apply it when ready.

3. Apply them individually, with client-driven "undo" to roll back in case of failure.

While "transactions" are often seen as necessary only for actions that cause changes, they are sometimes also needed to gather multiple chunks of data in a consistent manner. With that in mind, think about:

- What effect would option (1) have on your design, particularly how you carve the resources? Do you know which combinations are needed to directly support them? How does client variation affect this, and how does it impact caches?

- Is option (2) truly RESTful, considering the principle from *Fielding 5.2.2: "each request contains all of the information necessary for a connector to understand the request, independent of any requests that may have preceded it"*? How can caches "know" that a later step failed and need to revert their view? Better yet, how can they isolate state updates until they are committed?

- Who is in control in option (3)? Can that even work in the face of communication failure? What about transaction isolation - how do you prevent others from seeing the effects until it's complete? Should you only undo the changes made to the intended parts of the state, leaving concurrent changes to other parts, made by other clients, untouched? Or should that happen at all, if those clients depend on the interim state being rolled back? How can they be enabled to react, and what would they need to do?


# Steady vs Transitional States

API designers often think of representational states as steady and modified RESTfully by client requests. But what happens when your API needs to manage computers, and one of its tasks is to restart them? Which approach would you choose?

1. **Turn off, then on again**: Have the client update the "state" property from "started" to "stopped", wait a bit, then update it again to "started".

2. **Introduce a server-managed transitional state "restarting"**: The client would update the state from "started" to "restarting", and the server would handle the workflow through all necessary interim states. What considerations arise if other state updates come in while the server is still processing the restart?

3. **Introduce a new "computer restart" resource**: Create it for to the computer being restarted and enjoy the show.


# Performance & Caching

The performance of a design isn't just about how long a server spends handling a single request. A more complete picture includes factors like N+1 issues and the following considerations:

- **Total server workload** - How long do all servers spend handling all the requests needed for a single use case?

- **Network overhead** - How much time is spent in transit and how much latency does the network introduce?

- **Caching benefits** - To what extent can caching improve performance, even in the ideal case?

- **Cache efficiency** - Given variations in state representation, what will the cache hit rate be? Is caching more helpful than it is cumbersome to implement and maintain?

- **Access control constraints** - Is shared caching even allowed due to security and authorization requirements?

- **Freshness and consistency** - Do requirements permit caching at all?

- **Client-side workload and complexity** - How much effort is required to aggregate, filter, and transform the necessary data, potentially from multiple servers?

- **Code efficiency** - How optimized is the code on both the server and the client? How much time do developers have to refine performance versus simply managing complexity?

Achieving good performance means balancing all these factors. However, improving one often comes at the cost of complicating another. Addressing N+1 issues requires either development-time or runtime (dynamic) specialization per use case, which can reduce cache hit rates. Maximizing cache efficiency, on the other hand, demands standardized representations across cacheable resources, potentially worsening N+1 issues.

Hand-coding specialized resources multiplies the codebase in ways that are hard to keep consistent. These implementations aren't exact duplicates, yet they often perform similar functions, making maintenance a challenge.


# Resilience

Failure is always an option. But what happens when it affects only part of the whole? For instance, what if we can't retrieve a small portion of the resource state? Should we fail the entire request or respond with the data we were able to retrieve, apologizing for the missing parts? And what status code should we use for that partial response? It certainly isn't 206, which is reserved for "Range" requests. How do we "apologize" for the missing data so it doesn't cause confusion, especially for caches and clients that expect strict conformance to the media type structure (response format)? Could that apology even be considered a state representation?

One way to address this is to break resources down into sufficiently fine-grained units. This allows the client to request multiple resources and receive separate success indications for each, facilitating graceful degradation. But then, consider the impact on performance - what about the N+1 issue?

Finally, what should happen if the data we fail to retrieve isn't at all wanted by the client?


# Authorization

Similar to failures, a lack of authorization can prevent part of a request or response from being handled properly, and it requires similar considerations as those discussed in the previous section on resilience. However, there are additional complexities and nuances here. While failures are typically not something we try to conceal, a lack of authorization often requires certain parts of the data (sometimes specific values within a collection) to be quietly omitted as though they don't exist at all. This ***is success*** from an access control perspective. But does this really solve the problem? Not so fast. Consider a roundtrip scenario:

1. The client receives a state representation with some values omitted from a multi-value property.

2. The client adds and/or removes values it sees there, unaware of the omitted ones.

3. The client sends the updated representation of the entire resource back to the server as the "intended" state.

What should the server do? The new intended state won't include the hidden values. Should the server remove them and use the new intended state verbatim? Or should it leave them in? Is the new combination of values (intended + hidden) valid or not? If it's invalid and the server indicates that, could this be exploited to "fish out" the hidden values? Additionally, should the server be able to infer which values were hidden based on the current authorization data and incorporate that into its processing? Would this violate the requirement for requests to be complete and independent of previous ones *(Fielding 5.2.2)*?


# Recognizing malicious load

When all requests look alike, how can you recognize which one is malicious, perhaps part of a DDoS-style attack? A common approach that has been widely adopted is request rate limiting, sometimes applied separately for each type of resource (and potentially set to 0 for some client-resource pairs). This is based on the assumption that regular clients, driven by typical needs, such as serving human interactions, have defined behaviors with their own physical limitations. The goal is to translate those limitations into request rate limits.

However, there's a disconnect here. Client (and human user) behaviors have limitations that are defined within their own higher-level use case domains. Take, for example, a single human clerk's live interaction with a customer. It's not just about how many clicks are involved, but how many trips between the client and the server might be needed to support that interaction.

Unless each use case maps 1:1 to an API request, we'll need to account for more requests than the client's use case actually runs. Even worse, we'll have to handle many different, yet realistic, combinations of use cases over time and be prepared for the worst-case scenario: the maximum rate they could generate. This issue becomes especially troublesome if the N+1 problem exists, which, in most cases, it does. It amplifies human interactions by a significant factor, ultimately leading to a reasonable rate limit that doesn't hinder regular operations.

This is where malicious clients come in. They can target these inflated rate limits by sending the most expensive requests on purpose. To simplify the example, imagine a joint rate limit intended for multiple simple resource fetches by ID, but malicious clients send requests with complex searches, data transformations, and massive responses each time.

How did you solve this problem in your REST API? Did you solve it at all, or do you have more pressing matters to address?


# Who's the boss?

Servers serve representations of the current states of resources and accept representations of intended states. Beyond simply doing this and, when possible, complaining about (in)validity, what else do they do? Where is the business logic, beyond data gathering, validation, and storage?

If it's not on the server, then it must be on the clients, right? Is that a good solution? How do you ensure that all clients implement at least compatible business logic, if not the same one? How do you guarantee that all clients have up-to-date code at any time, including while they are running? And will this include polite requests to malicious clients as well?

In any case, that brings us to…


# Evolution vs Compatibility

What do we need to consider when evolving an API while supporting existing clients that we can't change? It's widely understood that we don't want to remove what those clients depend on, such as properties or parts of state representations they use when fetching the current state or sending the intended state. But that's not the whole story - far from it.

We also don't want to send additional data they don't expect in the current state or assume they expect it in the intended state. People often suggest that clients should simply ignore what they don't expect, but that doesn't always work for several reasons, including:

- Extra data could lead to buffer overruns.

- Completely ignored parts of the current state won't be included in the intended state during updates.

- There may be relationships between the extra data and the existing data that need to be considered together during updates.

To work around these issues, it's become common practice to version APIs, allowing legacy clients to continue working as they did before while new clients can move forward. Now, put your thinking cap on:

Do you have to update all resources (types) to new versions whenever any one of them changes?

1. **Yes**: How much work would that be? Would you hand-code everything for optimization, or handle it dynamically? What's the risk? How many versions of your API will you end up creating? Will it be possible for clients to iteratively migrate their code from one version to another?

2. **No**: Consider the client following the indicated links to related resources. Should the representations of these links include version information?

    - **No**: The client must be able to build the entire URL, including the desired version, knowing that not all versions exist across all resources and may not match.

    - **Yes**: How do you know which versions the client supports? Suppose the client follows that link. It's now looking to follow the reverse link. How do you ensure it returns to where it came from and not to the "latest version available before the source version"? In other words, how do you ensure a compatible roundtrip? Bonus question: The client stores those IDs in its own database. After several API version upgrades, the client looks up the ID and tries to get the resource. By this time, it may not just use a newer version, but require it. What does it do?

Let's add another challenge: Do links include full URLs with hostnames? This sounds convenient, as clients don't need to build those URLs themselves and can store and reference them later. It also leverages existing DNS and gateway infrastructure to support distributed services seamlessly. But does it?

Servers and services move, split, or merge. New, closer ones may be added, while older ones get decommissioned. How will you address that? Will you use tombstone proxies or gateways for decommissioned hosts? Will you force comprehensive client updates? Or do you have a better idea?


# Overengineering? (Im)possible?

Am I leading you toward clear, blatant overengineering? It may seem that way, but that's not my intention. I'm not the one making these decisions - I'm simply pointing out how many factors need to be considered and how significant the consequences of a wrong turn can be. Designing an API based on resource state transfers that will remain effective for the long term requires taking into account all relevant considerations. The same applies to any other API approach, though the set of considerations may differ. Should you explore those alternatives, or are you comfortable with your mastery of this particular tug of war?



# Next...

Continue to [part 4](rest-emperor-clothes) for thoughts on motivations and human costs involved in making and maintaining a REST API.

If you're interested in data security be sure to check out my posts on [authorization levels](authorization-thought-scaffold) and [how to recognize inappropriate activity](recognizing-inappropriate) as these can be particularly challenging with REST APIs.