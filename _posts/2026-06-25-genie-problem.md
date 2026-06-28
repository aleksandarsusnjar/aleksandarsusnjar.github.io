---
layout: tagged
title: "The Genie Problem: AI Treats the Unsaid as Negotiable"
tags:
    - "AI"
    - "SDLC"
    - "Software Development"
    - "Software Architecture"
    - "Testing"
categories:
    ask-yourself
permalink: /blog/genie-problem
excerpt: >
    AI is powerful because it fills gaps, and dangerous for the same reason. This post looks at underspecified prompts, tests that encode examples rather than intent, KISS as both help and trap, context-window gravity, randomized hidden tests, and why skill still matters when the software has real consequences.
---

# The Genie Problem: AI Treats the Unsaid as Negotiable

> **Series reading path:** 3 of 5: [Previous: The Cleanup Tax: AI Did Not Remove Review. It Moved the Bottleneck.](cleanup-tax). [Next: Documentation Was the Problem. Now It May Be the Product.](documentation-product)

![Map](/assets/genie-problem/illustration.jpg){: style="float: right; width: 50%; margin-left: 1em;"}
There is an old story shape in which someone finds a genie, makes a wish, and then discovers that words are dangerous instruments. The genie technically obeys. That is the problem.

AI coding assistants are not genies. They do not live in lamps, they usually have worse taste in method names, and they are less likely to appear in suspiciously ornate smoke.

But the metaphor earns its keep in one uncomfortable way: AI is very good at treating the unspecified as negotiable.

If the requirement says what to produce but not what must be preserved, the model will choose. If the prompt asks for a fix but not the design intent, the model will infer. If the tests show examples but not the contract, the model may satisfy the examples. If the codebase has a shared helper outside the context window, the model may write another one. If the organization has conventions that live only in senior engineers’ heads, the model will not politely wait for those engineers to have time.

It will fill the silence.

The silence may be expensive.

That silence is where the familiar old rule returns in a slightly more dangerous costume.

## Garbage in, plausible garbage out

“Garbage in, garbage out” used to sound almost too simple for software engineering. It still is. AI adds an extra twist: garbage in, plausible garbage out.

A weak prompt does not always produce obviously weak output. It may produce coherent, idiomatic, nicely formatted, extensively commented output. It may even produce tests. It may explain itself with the confidence of a person who has never had to support a production system at 2:13 a.m.

That fluency changes the risk. The result may not look like garbage. It may look like something a tired team is grateful to accept.

Where does the model get the missing pieces? From patterns. Some patterns are excellent. Some are mediocre. Some are shortcuts that helped someone win a deadline and lose maintainability. Some are cargo-cult fragments copied across the internet so often they acquired the protective coloration of truth.

The model does not know which missing constraints you considered too obvious to mention. It only knows what context it has and what patterns help complete the task. Research on [Copilot robustness](https://arxiv.org/abs/2302.00438) has also shown that semantically similar natural-language descriptions can produce different code, which should surprise nobody who has watched human language try to impersonate a programming language.

If an important constraint is not stated, visible, inferable, or enforced, it is not important enough to reliably survive.

Call this, if you like, the Law of Unspecified Requirements:

**Anything important enough to matter is important enough to constrain.**

## The always-return-four school of testing

Imagine asking for an expression evaluator and giving it these tests:

- `2 + 2` should produce `4`
- `2 * 2` should produce `4`
- `3 ^ 2` should produce `4`
- `10 - 6` should produce `4`

A correct evaluator should parse and evaluate expressions. A useless implementation can return `4` every time and pass every visible test. This is the same deliberately silly trap from the previous article, now viewed from the specification side rather than the review side.

This example is absurd, which is why it is useful. It exposes the difference between examples and intent. Tests that cover examples do not necessarily cover the contract. As I argued in [Code coverage is an indicator, not a goal](code-coverage), code coverage can tell us that code ran. It cannot, by itself, tell us that the important behavior was tested.

AI makes this more acute because AI can be very good at satisfying the visible target. If the target is a weak test suite, it may satisfy the suite. If the target is a vague story, it may satisfy the story-shaped words. If the target is “make the build pass,” it may make the build pass while preserving the wrong design.

The model is not malicious. It is not aware that “passing tests by missing the point” is professionally unacceptable. The workflow must make the point visible.

There is a famous old [tree swing cartoon](https://en.wikipedia.org/wiki/Tree_swing_cartoon) that shows what the customer wanted, what was specified, what was designed, what was implemented, and what finally arrived. The joke survives because it is not really about swings. It is about every place where intent was assumed instead of transmitted. AI does not remove that problem. It gives the missing-intent problem a faster typist.

Weak tests are one way to under-specify intent. When AI agents can see both the implementation and the test code, the danger becomes sharper: the easiest target may become passing the visible tests, not satisfying the hidden intent. One protection is to isolate test specifications and test code from the implementation agent, and to report failures without revealing the exact values that would let the agent overfit the next patch. Another, more controversial protection, is to use randomized values where possible. Yes, that can increase the risk of intermittent failures if done carelessly. It also makes coding to a handful of visible examples harder.

That experience makes pure Test-Driven Development uncomfortable in this setting. TDD already has a tendency to shape APIs around what is convenient to test rather than what is best for the design. With AI agents, the pure form becomes even riskier because the test can become the target instead of the specification. Tests remain essential. Letting tests become the specification is the part that should worry us.

Over-local simplicity is another way to under-specify intent.

## KISS can become a trap

“Keep it simple” is good advice.

Unfortunately, good advice becomes bad when applied to the wrong problem.

AI coding tools often prefer small, local, immediate changes. That can be exactly right. A small bug should not become an architecture expedition. But a deep design flaw should not receive a decorative patch because the patch is smaller.

The smallest change can be the change that leaves the root cause intact.

Humans do this too. We call it pragmatism when we like it and technical debt when we do not. AI merely makes the pattern easier to repeat: local symptom, local patch, local green test, global confusion preserved.

The question is not whether a change is simple. The question is simple relative to what?

- Simple relative to today’s diff?
- Simple relative to the conceptual model?
- Simple relative to future change?
- Simple relative to the person who must debug it later without the conversation that produced it?

A superficial patch is often simple in the smallest visible frame and complicated in the frame that matters. Those questions belong before the patch, not after the incident review.

There is a positive side here too. When a larger change really is required, AI can reduce the emotional and practical pain of churn. It does not have feelings about yesterday’s code. It will not sulk because a previous implementation has to be thrown away. If the design direction is clear, the same raw generation speed that makes superficial patches dangerous can also make necessary refactoring less painful.

That makes KISS more important, not less. The trick is to keep the right thing simple: the model, the responsibilities, the paths of change, the review surface, and the explanation the next maintainer will need.

Local patches become especially tempting when the system is too large to hold comfortably in view.

## Context-window gravity

Large systems have gravity. They pull every change toward accumulated history: shared helpers, conventions, tests, domain concepts, deployment assumptions, performance scars, undocumented decisions, and several layers of “please do not touch that unless you understand why it looks wrong.” Concerns about duplicated code and reduced reuse are not imaginary; [GitClear’s 2025 AI code-quality research](https://www.gitclear.com/ai_assistant_code_quality_2025_research) reported rising clone and churn patterns in the AI-assistant era, which fits the same risk shape even if any one organization must measure its own reality.

AI tools have context windows, retrieval systems, indexes, embeddings, project memories, and sometimes spectacular marketing pages. Even with large context windows, project reality is not just text volume. It is the relationship among specifications, code, tests, documentation, ownership, deployment, incidents, and the expectations of people who have been hurt before.

As projects grow, AI acceleration tends to drag:

- The model misses existing utilities and reimplements them.
- It duplicates patterns that should be centralized.
- It loses track of specification-to-implementation-to-test mapping.
- It optimizes local changes because the global design is expensive to reconstruct.
- It burns more tokens to understand what previously fit in a small prompt.
- It spends effort navigating tests, fixtures, generated code, and historical decisions.

This does not mean AI stops helping. It means the help becomes more dependent on architecture, documentation, retrieval, task decomposition, and review discipline.

A small codebase asks, “Can you write this?”

A large codebase asks, “Can you change this without misunderstanding everything nearby?”

Those are different questions.

## The bar moved, but it did not vanish

There is a real democratizing effect here, and pretending otherwise would be silly. For smaller, simpler, low-stakes projects, the bar to get something working has dropped. A person who could not previously assemble a useful script, prototype, internal tool, or small application may now get something running. That matters. It is not fake progress merely because it arrives through a model.

But “something running” and “something we can responsibly deliver to many customers” are different thresholds.

For exploratory work, personal tools, one-off automation, or throwaway prototypes, AI can compress the distance between idea and artifact. The result may not be elegant. It may not survive a year. It may not need to. Sometimes the right standard is not architectural purity; it is whether a small tool helps a real person do a real thing today.

The standard changes when the software becomes mission critical, customer facing, security sensitive, regulated, expensive to recover, or depended on by many users. Then the lowered entry bar is not enough. The work must be correct enough, maintainable enough, secure enough, observable enough, and understandable enough for consequences beyond the first successful run.

That is where skill remains essential.

The person directing the AI is not a passive user in the shallow “click the shiny button” sense. They are closer to an operator in the older computer-room sense: someone responsible for running a powerful system deliberately, even if the machine does much of the mechanical work. That operator must know what to specify, what to leave open, what to reject, what to test, what to ask next, what smells wrong, what hidden assumption has appeared, and when the model is confidently applying a pattern from a world that is not this one. Past mistakes matter. Experience matters. Not as authority, but as scar tissue that recognizes risk early.

A novice may prompt for a solution. An expert prompts for a solution inside constraints, with edge cases, failure modes, test strategy, integration requirements, performance considerations, security boundaries, and maintainability expectations. The expert also knows that the first answer may be impressive and still wrong.

That is not “prompt engineering” in the shallow sense. It is engineering.

## Specifications are how we stop whispering intent

If requirements live in a ticket, architecture lives in a diagram, conventions live in senior engineers’ heads, examples live in tests, and exceptions live in Slack archaeology, AI will assemble a partial reality and act on it.

This is not an AI flaw. It is an organizational mirror.

Humans have been compensating for missing specifications through conversation, memory, habit, review, and shared context. AI lacks much of that unless we externalize it. The work that used to be hidden inside experienced humans must become inspectable.

That includes:

- what the feature is for
- what the system must not do
- accepted and rejected examples
- invariants
- shared utilities
- naming conventions
- security boundaries
- test obligations
- performance expectations
- documentation update rules
- when a local patch is not enough.

This does not mean writing endless bureaucratic prose. It means writing enough durable context that the model, the reviewer, and the future human can reason about the same thing.

## The genie was not the villain

In the genie story, the lesson is often “be careful what you wish for.”

That is close, but not enough.

The better lesson for AI-assisted development is: be careful what your system makes wishable.

If the easiest request is “fix this test,” you will get test-fixing behavior. If the easiest request is “implement this ticket,” you will get ticket-shaped implementation. If the easiest request includes specification, examples, constraints, review lenses, and conformance expectations, the model has a better chance of producing something aligned with the real goal.

AI treats the unsaid as negotiable.

So stop relying on the unsaid.


## Continue the series

- Previous: [The Cleanup Tax: AI Did Not Remove Review. It Moved the Bottleneck.](cleanup-tax)
- Next: [Documentation Was the Problem. Now It May Be the Product.](documentation-product)
- Reading path: after the specification problem, continue to [Documentation Was the Problem. Now It May Be the Product.](documentation-product) to examine why documentation becomes operational grounding for AI-assisted work.

## References and further reading

- [Wikipedia, *Tree swing cartoon*](https://en.wikipedia.org/wiki/Tree_swing_cartoon)

- [Antonio Mastropaolo et al., *On the Robustness of Code Generation Techniques: An Empirical Study on GitHub Copilot*](https://arxiv.org/abs/2302.00438)

- [DORA, *State of AI-assisted Software Development 2025*](https://dora.dev/research/2025/dora-report/)
- [Sida Peng, Eirini Kalliamvakou, Peter Cihon, Mert Demirer, *The Impact of AI on Developer Productivity: Evidence from GitHub Copilot*](https://arxiv.org/abs/2302.06590)
- [METR, *Measuring the Impact of Early-2025 AI on Experienced Open-Source Developer Productivity*](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/)
- [METR, *We are Changing our Developer Productivity Experiment Design*](https://metr.org/blog/2026-02-24-uplift-update/)
- [Nicole Forsgren et al., *The SPACE of Developer Productivity*](https://queue.acm.org/detail.cfm?id=3454124)
- [Abi Noda et al., *DevEx: What Actually Drives Productivity*](https://queue.acm.org/detail.cfm?id=3595878)
- [GitClear, *AI Copilot Code Quality: 2025 Data Suggests 4x Growth in Code Clones*](https://www.gitclear.com/ai_assistant_code_quality_2025_research)
- [Annie Vella and Kelly Blincoe, *The Impact of AI Coding Assistants on Software Engineering: A Longitudinal Study*](https://arxiv.org/abs/2605.23135)
- [GitLab / ITPro coverage of the 2026 AI accountability findings](https://www.itpro.com/software/development/enterprises-are-shipping-so-much-ai-generated-code-they-cant-control-or-secure-it)

Cogitatorium posts:

- [*The Metrics Maze: Uncovering the Dark Side of KPIs*](kpi)
- [*Code coverage is an indicator, not a goal*](code-coverage)
- [*That's not a Road Map*](roadmap)
- [*The Limits of Influence*](influence)
