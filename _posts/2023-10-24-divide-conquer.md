---
layout: tagged
title: "Divide & Conquer in Software Development"
tags:
    - SDLC
    - Mythbusting 
    - "Software Development"
categories:
    ask-yourself
permalink: /blog/divide-conquer
excerpt: >
    Forget the clichés - "Divide & Conquer" is only half the story. In this post, I challenge the blind faith in endlessly splitting everything into smaller parts and argue for a balanced, thoughtful approach that combines division and cohesion in software and teams. By exploring real-world consequences, I'll confront the myths and limitations of endless division, exposing how blindly following this mantra can lead to overengineering and missed opportunities. Instead, I'll show you how functional composition, smarter abstractions, and practical strategies can help you navigate complexity, adapt to change, and thrive without losing agility. Whether it's microservices, team structures, or system design, this is a call to rethink how we build, organize, and evolve - stripping away the hype and focusing on what truly works.
---

Software development has a rich history that spans several decades, and if we trace it back to the contributions of Augusta Ada King, Countess of Lovelace (née Byron), it even stretches nearly two centuries. Over this long journey, we have consistently rediscovered a timeless lesson that goes beyond the evolution of technology. Let's take a moment to reflect on this lesson from the perspective of today.


# Balancing Act

Read the following paragraph:

> *"Divide lengthy sections of code into reusable subroutines. Separate the human-readable code expression from the process of translating it into machine language. Split subroutines into self-contained and reusable categories, such as classes, modules, units, or packages. Opt for creating small, specialized yet reusable programs that excel in one specific task."*

Does it sound familiar? Have you heard it many times? Do we truly appreciate what these ideas teach us? They share a common theme: breaking down something larger into smaller, manageable parts. Now, let's rephrase it:

> *"Put together related sections of code into subroutines. Move everything related to writing machine code into higher-level programming languages. Bring related subroutines into classes, modules, and units. Package high-quality code around related functions into dedicated programs."*

This version focuses on combining, rather than dividing, yet it seems to address the same concepts. Which one is better? "Divide & Conquer" or "Combine & Conquer"?

In the real world, endless division leads to destruction – think nuclear fission. On the other hand, never-ending fusion results in collapse – think black holes. Both yield diminishing returns. So, how do we decide when to stop? Balancing these forces against something else might offer some guidance. Let's take a look at the other concepts we see frequently in the paragraphs above.

The division paragraph was about **"reusability"**. Eventually, reusability diminishes when a "part" becomes either too small to justify the associated overhead or too specific to be useful. The combination paragraph was about **"relatedness"**. Combining unrelated functions forces users to handle the unexpected, reducing usability. A specialized tool, like a screwdriver, is better than a generic one, like a Swiss army knife, for its intended purpose. However, most great meals have more than one ingredient.

Now, let's step away from code and apply this thinking to people and their expertise. Should expertise and teams be endlessly divided into the smallest reusable units? Should everything always be combined so that everyone knows everything? Do you think that's possible? If the extremes aren't practical, we need a balancing act between division and combination.

At this point, we need to introduce a well-known bit of reality:

> *"Any organization that designs a system (deﬁned broadly) will produce a design whose structure is a copy of the organization's communication structure."*
>
> -- Melvin E. Conway

This tells us that the balancing act we're considering affects both the system (technical) and the business (team) organization. If we try to establish different structures, inefficiencies will inevitably arise, causing the system's organization to align with the communication structure - whether we like it or not.

That's not necessarily a problem. Non-technical business leaders face similar challenges when deciding how to organize their companies. They just have different "logistical" challenges than technical people do. In either case, the problem we're solving is quite similar - both business and technical "logistics" need to be considered. What usually ends up happening is that we have units for each distinct, decided "domain". Even when geographically distributed, each individual domain tends to be similar across locations. We can apply the same thinking here and, through "Domain-Driven Design", arrive at "Domain-Oriented Architecture".

Breaking down large problems into smaller components allows for efficient, specialized solutions and promotes reusability. Combining related solutions can enhance productivity, though the outcomes may sometimes surprise us. It's important to find the right balance in division; too little can harm reusability and focus, while excessive division leads to unnecessary effort and overhead. Constantly evolving tools help mitigate these challenges. This process of problem-solving has historically been difficult, but advancements in programming languages and network technologies have made it more manageable.

Not too long ago, a significant advancement came with the rise of microservices. While they brought various advantages, they also created challenges, as clients had to understand all the services. Decisions regarding service breakdowns affect all clients, leading to performance and reliability issues. To address this, businesses either treated all clients uniformly or invested in costly dedicated services tailored to each client (e.g., BFF: Back-End for Front-End). Ironically, these frequently evolve into per-client monoliths.

Today, we can do better. Just as functional programming gained popularity, the realization that service APIs needed to be functional also emerged, enabling services to [collaborate through functional composition](functional-composition). With this approach, component services can remain modular without the need to develop countless BFFs. I'll dive into more details in a dedicated post.


# Labyrinth

![Map](/assets/divide-conquer/labyrinth-2.png){: style="float: right; width: 50%; margin-left: 1em;"}
Can you plan your road trips from start to finish? Do you? What about journeys that take months, if not years? You may have some goals in mind. You might even have a path mapped out. But things change. Some paths may close, others may open – the Labyrinth itself evolves. More importantly, as you experience and learn more, your interests may shift. You may even discover that the Labyrinth has other exits you hadn't considered before.

Rigid plans risk leaving you stranded or taking you to places you no longer want to be. And, let's not forget, in business, each journey is also a race. Who do you think wins the race – those who stick rigidly to initial plans or those who adapt to the developing situation?

We're facing two opposing forces here:

- The desire to plan as much as possible upfront to avoid taking the wrong path.
- The desire to start as soon as possible to get ahead, thus planning less.

Of course, neither extreme is ideal. The first is too rigid. The second leads to endless random wanderings. Before we move on, let's introduce another complication. I've noticed that indecisive people often think they're saving time by deferring decisions. But, if you think about it, choosing to defer something is a decision in itself. Does this mean there's only one option (planning everything upfront)? Not necessarily, because these two approaches don't deal with the same level of detail or risk.

There's another fallacy I've observed: some people think they can model a good decision by copying the simplest one they can think of and just going with it. There's another term for this: "wishful thinking", which often leads to the "I wish it were that simple" regret.

My point is, we can't just sweep important questions under the rug or assume that the simplest solution will always work. We need to be smarter about this, but trying to be fully prepared leads to "analysis paralysis" – and that's not helping anyone. So, what should we do? It's clear that we can't eliminate all decisions, and we shouldn't try to fully plan them all either. Since each decision involves evaluating alternatives, we can use the following tools to guide us:

1. Identifying plausible alternatives and understanding their differences.
2. Managing the risks associated with each alternative and knowing when to shift.

Both require effort. However, we can reduce the effort in identifying alternatives by managing risks effectively. In our case, "managing" risk doesn't just mean recognizing it; it's about decoupling or abstracting system components in a way that reduces the cost when our approach turns out to be wrong. We must accept change and the fact that we will inevitably make mistakes. These aren't risks, they're guarantees. By accounting for them regularly, we can respond quickly, leverage new solutions, and adapt to evolving market needs.

Think of this journey as a hike. The world isn't as convoluted as a Labyrinth. You can see some paths clearly around you. Clarity is greater up close and diminishes the further out you go, but you still have a rough idea of where you want to end up, so you pick one of those paths. Some of them might turn out to be dead-ends or require backtracking. Sometimes you'll need to jump over a stream or take a detour. That's okay; it's part of the deal. So, how do we make this okay, and reduce the pain of setbacks?

- Appropriate (de)coupling: avoid tightly coupling with overly simplistic assumptions. Leave space for better, evolving parts.
- Automate more: from development environment setup to testing and deployments. A more effective workforce can handle changes more efficiently.

Now, back to the main topic: divide and conquer. We have to accept that we'll make some wrong division/combination decisions. Functionality that we place in one system component might be better suited to another. Accommodating this, the way I often see it done, requires all clients of these components, including gateways, to adapt. Their code must change to reflect the reorganization of services. Wouldn't it be nice if we didn't have to do that, if there were some magical piece that could shield clients from the impact of service refactoring? A codeless gateway that just works? That brings us back to the point I made earlier about [collaboration between services through functional composition](functional-composition). This approach also makes us more agile when facing changes.


# Next...

Continue to my post about [common errors in Agile implementation](myths-in-agile) or 
(back) to [how to build collaborative services using functional composition](functional-composition).
