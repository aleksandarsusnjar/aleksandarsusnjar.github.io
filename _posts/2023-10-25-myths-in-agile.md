---
layout: tagged
title: "Agile Mythodology"
tags:
    - SDLC
    - Mythbusting 
    - "Software Development"
    - "Software Architecture"
categories:
    ask-yourself
permalink: /blog/myths-in-agile
excerpt: >
    Explores common misconceptions and mistakes in the implementation of Agile methodology, highlighting real-world observations and their consequences. While Agile itself remains a powerful framework, the way it is often applied can lead to inefficiencies, confusion, and missed opportunities. From misunderstandings around simplicity and team structure to misguided priorities and the misuse of time metrics, the post aims to shed light on how these errors can hinder progress. The goal is to foster a deeper understanding of Agile practices, encouraging teams to adopt a more thoughtful, informed approach to achieving true agility.
---

That "y" in the title is no typo – it's a nod to the myths that many believe when they choose to apply "Agile Methodology". I promise I won't make it any easier for you to make decisions, but my hope is that it will at least make things a bit clearer. Still with me? What does "agility" mean to you? In the world of software development, many equate it with "speed", as if that's the ultimate goal. But is it? Let's explore that.


# Can You Speed Up Art?

Think about an artistic process. How does it work? Can you prescribe it or script it? Let's start with an easier question: How do you recognize art? I think history has taught us that this is incredibly difficult, if not impossible. If it were easy, we wouldn't have so many artists recognized only posthumously. We'd also be able to engineer that recognition - and whenever we manage to do that, we're pretty close to being able to create it too. Would you say that generative AI can produce art, or is it the prompts and subsequent recognition that humans make which drive it? Art requires something more: a context, a goal, a perspective - something we can't yet quite define, but that evokes individual thoughts and feelings. It isn't about perfection, which is why it may be the only thing that can be perfect.

**How Does Art Compare to Engineering?**

Engineering is about making choices about how to work with physical realities, followed by the actual building of a creation. In engineering, the building part is prescribed; in art, it's a part of the creative process. Engineering decisions tend to be more limited and straightforward. If a bridge needs to be built, that's what needs to be built - not something that might evoke thoughts of a bridge, no matter how it's interpreted. Art, on the other hand, has many more degrees of freedom and decision-making in its creative process. We can see the two intertwine, as when art influences engineering to produce striking buildings. However, when engineering affects art, it's often seen as a necessary but undesirable compromise.

**Is Software Just Engineering?**

Let's think this through. At first glance, software might seem purely like engineering, but it doesn't really deal with the "real world" in the same way. The hardware limitations today are laughable compared to what they were at the beginning. Other factors have become much more important than the physical, engineering efficiency of the code. It matters how the code will be thought of, how it will evolve, and how it will interact with future, yet-unknown code. There is no "building" part in the traditional sense. Once the creative process is done, the software is done. Unlike constructing a bridge, no one has to dig foundations, lay bricks, or connect beams. The only creative part of making software is much closer to art than engineering.

**Pure Engineering vs. Art**

In pure engineering, we can reasonably estimate how much time it will take to build whatever is required. We have solid data on how long decisions take and even better data about simple mechanics - such as how long it takes to paint walls, cut wood, or record sounds. Now, switch to what may appear to be mechanically similar but isn't: how long does it take to paint an oil painting, carve a sculpture, or compose a symphony? Suddenly, we run out of answers.

**So, why is everyone trying to prescribe how to make software?**

Why are some intent on applying purely engineering practices to software development? "We have a greater supply of nuclear green paint, and it sold well. It must be good to use more of it in your paintings". Could that work? Let's stretch our imagination and assume it could. Should other "artists" apply the same suggestion just because it worked for one?

The software development business is… well, business. Business has its own realities and constraints, always striving for better ROI. Pressures are high, and the difference between success and failure is often slim. Imitating the success of others seems like a less risky path than doing something very different or disruptive. The trouble is that businesses aren't all the same. Those that differentiate themselves well tend to do better. What made one successful may be the cause of failure for another.


# An Example

I'll call out a major example that has been applied significantly, yet often incorrectly, to software development.

Toyota, a car manufacturer, found its lean manufacturing practices to be incredibly valuable as they optimized the logistics of parts needed for assembly, thus reducing overhead. But here are some questions for you:

Did Toyota apply the same practices to the pre-manufacturing process of designing their new products? If you think they did, how far did they take it?

Which part of the software development lifecycle do you think the assembly and part-sourcing logistics maps to? The creative part or the process of producing and stamping physical media with ready-made software? When was the last time you saw that done? Do you even know what I'm talking about? (Hint: If not, software used to be sold in physical boxes, usually made of cardboard, containing CDs, diskettes, and printed manuals. These had to be manufactured, assembled, and each part came from a different source.)

Am I bashing "lean software development"? I'm trying **not** to. What I **am** trying to do is point out the frequent misapplication of it. There are plenty of valuable lessons to be learned, but they need to be understood well. Without this understanding, we can end up making mistakes. I'll share the errors I've observed. You may find them controversial - they're meant to provoke thought, challenge your current perspectives, and give you the chance to either double down on your opinions or adjust them accordingly.


# Common Errors

**A quick reminder**: Please understand that these are **not** critiques of what I, or the authors of well-known practices, intended. Instead, they reflect my observations from real-world experiences - what's actually being done and the consequences that have followed.


![Map](/assets/myths-in-agile/rattlesnake-kiss-2.png){: style="float: right; width: 50%; margin-left: 1em;"}
# K.I.S.S. of Death

Is it "Keep it Simple, Stupid" or "Keep it Stupid, Simple"? I'm sure you know which one it *should* be, but does that match how it's actually applied? Is simplicity always good, or is there a point where it becomes a liability? This "KISS" philosophy often comes up in conversations like the following:

> *There are no requirements for the immediate work at hand. Who cares about anything beyond what's needed right now? After all, requirements may change, so we should be lean. KISS!*

That "kiss" is typically followed by assumptions - and sometimes even arguments - that any future possibility is equally likely, so we should always go with the simplest one. Depending on your perspective, you might notice a few related false premises in there:

- We don't know or care about the long-term direction. Only the next step matters.
- Choosing what seems easiest in the moment is good enough.

Now, imagine you're traveling to a faraway destination, but you're not flying. You'll make several stops along the way to eat and work to replenish your funds. Do you:

1. Pick the nearest or best work and eating opportunity, regardless of where it is in relation to your intended destination?
2. Steer your travel in the general direction of your destination, even if there are closer options that seem better?

**Bonus question:**

Do you trust yourself enough to pick a clear destination, or are you just roaming around, picking the easiest option?

Do you see where this is going? Option (1) will likely leave you stuck in local minima or wandering aimlessly. Don't take this route unless you're still figuring out what your business is about. In other words, the simplest solution can often turn out to be more "stupid" than "simple". Make informed decisions. If you don't know enough at the moment, consider holding off or abstracting things in a way that considers other probable possibilities beyond the most straightforward one. 

Remember: Keep it as simple ***as possible***, but ***not simpler than that***!  


![Map](/assets/myths-in-agile/tafapamh.jpg){: style="float: left; width: 50%; margin-right: 1em;"}
# Type as Fast as Possible, Add More Hands! (TAFAPAMH!?)

This is a common interpretation of "extreme programming", heavily relying on "KISS". The focus is on achieving immediate results, justifying the means by the short-term ends. That's all well and good when there are no "further ends" to consider. But what happens when the product needs to evolve beyond the first "release"? What if maintainability isn't considered a priority? Well, you end up with an unmaintainable product. 

For some vendors, this might seem advantageous since new requirements can bring more work. But simplifying things for the long term - making them easier to maintain - often gets overlooked. That's not how I see things. How about you?


![Map](/assets/myths-in-agile/fire-fighting.jpg){: style="float: right; width: 50%; margin-left: 1em;"}
# Fire-Fighting as the Only Priority (FFATOP?)

"Fixing burning issues is all we do" is not a motto you want to adopt. How did you end up in this situation? How could you have prevented it? That's a separate discussion. For now, let's say you're in this predicament. Are you investing all your resources in putting out fires? Is there a real end in sight? Are you giving yourself the time and space to reassess, notice patterns, and address multiple issues with a single solution? Are you investing in growth, or are you letting your competition leapfrog you?

Some view this as a form of final life support - "milking the cow" while it can still be done. But consider the long-term outcome: Is this really profitable? After putting out the current fires, will your next steps involve more of the same, or will you take a different approach? Wouldn't it make sense to start working on that new approach now?



![Map](/assets/myths-in-agile/jacks-of-all-trades.jpg){: style="float: left; width: 50%; margin-right: 1em;"}
# "Jacks of All Trades = No Bottlenecks"

There is a common, yet somewhat shallow, assumption among those trying to apply agile processes they've read about: having many small teams with broad and equal expertise and responsibilities means there are no bottlenecks. On the surface, it seems true - if one team is busy, another can be allocated. Even better, this approach appears to mitigate the risk of employee churn, as everyone, including entire teams, is easy to replace (or dismiss) with minimal impact. On the other hand, having individual experts, SMEs, and specialized teams is often seen as creating bottlenecks, as fewer people/teams can be assigned to any particular task at any given moment. But is all this truly the case, or is it simply hiding some side effects under a proverbial rug?

Let's flip this around. Does this mean experts aren't needed or wanted? No. It simply ignores the assumption that experts are easily available everywhere. You shouldn't need a wake-up call to realize this isn't true. Decades ago, it was possible for one person to know everything about computers and software. That time has long passed. You can no longer expect any one person, or even a team, to be experts in ***everything***. And even if you could, you probably wouldn't want to. Why?

Let's entertain a bit of a fairy tale scenario: all your people are experts in everything. How do you get them to agree on things? Oh, wait, that's part of the fairy tale too - they just do, because their equal expertise results in equal opinions every time. No discussions needed; it happens naturally. They are perfect clones of one another. Let's continue.

As you assign different tasks to different teams, their experience - and, by extension, their expertise - will start to diverge. Will you try to manage that by distributing work in a way that ensures everyone gains similar experience? Doesn't that risk bringing a bottleneck back? How costly or even possible do you think this is? How quickly do you think expertise will develop if you're constantly shifting focus? One day, they work on task A, the next on task B, and so on. But let's dig deeper:

1. How much effort will it take them to remember the last state they left off at?
2. How much effort will it take to discover what others have done to that state since then?
3. How will they decide on the approach to take? Will they have a clear vision or direction to follow? Whose vision? Where does it come from? Will they form their own, or follow someone else's?
4. In light of (1)-(3), what will motivate them to stay engaged?

Bottlenecks don't disappear. A bottleneck is simply the risk of not being able to do work at the desired time. In the scenario described above, that ***risk*** is replaced by a ***guarantee*** of consistently slow performance and the ***elimination of ownership*** due to constantly shifting focus. Both factors decrease motivation - one because progress is slow, the other because lack of ownership obstructs career growth and misaligns with individual interests.

In a world where perfect generalists (know-it-alls) are impossible, specialization remains key to career growth. Specialization can take many forms - vertical, horizontal, or even diagonal 😊 - technical or managerial. It must be allowed and leveraged, not prevented. While broad expertise is valuable and necessary for understanding how different specializations fit into the bigger picture, it shouldn't be the only driving force. I'm happy to accept the risk of temporary bottlenecks if it means fostering a happier, more capable workforce that accomplishes goals more efficiently and with greater satisfaction. And no, that's not a fairy tale. It is entirely possible.


# Bonus: The Space-Time Delusion

How much work can a team accomplish in a sprint? This is, of course, crucial to avoid taking on work that can't be completed, either because it's too large or because it doesn't fit with other tasks. But how do you figure that out? It's essentially a numbers game – if your estimate doesn't exceed the limit ("capacity"), you're good. I'm not concerned here with the process you use to arrive at those numbers, but with what those numbers really represent. What does a "story point" actually mean?

If you dig into this, you'll find that:

- There's a strong desire to avoid relating story points to time units, in an effort to avoid committing to specific timelines.
- Ideally, story points should be additive - meaning, the total points of individual work items should closely match the points for the entire body of work.
- New teams often struggle to synchronize their understanding of what points mean, but they eventually figure it out… until new members join. Think about how each contributor comes to understand the meaning of a "point".

The concept of velocity is then introduced: the number of points a team can complete in a sprint. Sprints are typically of a fixed duration.

Now, we're not naive. We all studied enough math, even in elementary school, to understand that to travel 60 km, a car driving at 60 km/h will take 1 hour. That's the basic equation of distance, speed (velocity), and time. We've got the velocity (points per sprint). We know the distance too – a single sprint, usually lasting a number of weeks. Divide the two, and you get how many points can be completed in a week. It becomes a unit conversion exercise. Whether appropriate or not, points effectively start to become time units within the team (for capacity planning), and it's easy for others to do the same. At this point, we may as well stop pretending this isn't the case. Pretending doesn't save time - it wastes it, and doesn't accomplish what we hoped it would.

But let's go further. Is this even supposed to work this way? Should agility only apply to individual teams, or should it extend across the entire business? Shouldn't you be able to align the meaning of story points across different teams? The business side of the "story" also has its own need for agility - sprints, capacities, and a very real definition of points: money. Capacities translate to budgets, and sprints are bounded by deadlines for delivering work to achieve a desired outcome. Customers don't care about understanding abstract "points" - they mostly care about cost and availability at the right time. 

Can the business be agile if it's not given the same opportunities as you? And if it isn't agile, what does that mean for you? Can you really be agile, or are you just pretending?

Time units are real, and they aren't inherently bad - what matters is how we use them. The problem arises when they're misused or overlooked, which is something we should actively avoid. We all rely on time to plan our work (including software development) and to understand when we can expect dependencies from others to be completed. No, we shouldn't attempt to estimate precise ETAs for every single user story, but we do need to have a general awareness of when we can expect key milestones or larger buckets of dependencies to be completed. This helps us better manage expectations and plan our work around those critical dependencies.


# Cogito, ergo sum

"I think, therefore I am." What works for others may not work for us. Our decisions are our responsibility, and blaming others after our own failures won't help. Due diligence ***is*** crucial - and it must be repeated, as we need to stay on top of the ever-evolving landscape. Blindly following someone else's recommendations will only bring us closer to what they're already doing, but never quite get us there. To truly leapfrog them, we must do better than that.


# Next...

Continue to my post about [roadmaps](roadmap) to see what agile yet long-term planning looks like vs how it is often done.

For insights on how (not) to measure team performance, please check out my post [*The Metrics Maze: Uncovering the Dark Side of KPIs*](kpi).