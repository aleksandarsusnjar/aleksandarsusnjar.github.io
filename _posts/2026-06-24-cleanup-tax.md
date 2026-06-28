---
layout: tagged
title: "The Cleanup Tax: AI Did Not Remove Review. It Moved the Bottleneck."
tags:
    - "AI"
    - "SDLC"
    - "Software Development"
    - "Software Architecture"
    - "Engineering Management"
    - "Quality"
categories:
    ask-yourself
permalink: /blog/cleanup-tax
excerpt: >
    AI can generate code quickly, but someone still has to understand, review, reconcile, and own what it produced. This post explores the cleanup tax: the review and quality burden that can erase raw generation gains unless review itself becomes structured, perspective-driven, partly automated, and honestly reconciled.
---

# The Cleanup Tax: AI Did Not Remove Review. It Moved the Bottleneck.

> **Series reading path:** 2 of 5: [Previous: AI Productivity: The Metric Was Already Broken](ai-productivity-metric). [Next: The Genie Problem: AI Treats the Unsaid as Negotiable](genie-problem)

![Map](/assets/cleanup-tax/illustration.jpg){: style="float: right; width: 50%; margin-left: 1em;"}
AI can produce code very quickly.

That sentence is both true and dangerously incomplete, which is an impressive accomplishment for seven words.

A model can generate a function, a test, a class, a migration, a configuration file, a README section, and a slightly overconfident explanation before a human has finished deciding whether the naming is unfortunate. This raw speed is real. It is why the conversation exists.

But software development was never only the act of producing text that resembles code. It is the act of changing a system responsibly. That includes understanding what should change, why it should change, what must not change, how failure appears, who depends on the behavior, which tests mean something, which tests merely applaud, and how the next person will maintain the result after today’s excitement has left the chat.

AI has not removed that work.

It has moved much of it into review.

## Review is not a button

There is a comforting fantasy hidden inside many AI productivity claims: the model writes the code, a human reviews it, and productivity improves because review is cheaper than writing.

Sometimes that is true.

Sometimes review is more expensive than writing.

A serious review of someone else’s change requires understanding the problem, design, assumptions, edge cases, tests, architecture, deployment path, failure modes, and surrounding code. A strong developer reviewing a weak or context-poor change may need to reconstruct the work mentally, identify what the change is trying to do, discover what it accidentally does, and decide whether the whole approach is wrong.

At that point, the reviewer is not saving time. The reviewer is doing the work through a fog machine.

This is not new. Humans have always produced “slop,” although we previously gave it more respectable names: spaghetti code, rushed work, legacy code, temporary hacks, unclear requirements, low-context changes, and “we will clean this up later.” When the damage survives long enough, we often promote it to the more professional title of technical debt. The difference is that humans produce slop at human speed. AI can produce it at industrial speed, with tidy formatting and a cheerful explanation.

That makes review architecture part of the productivity system.

## The human-review trap

If every AI-generated change requires deep human review, we may destroy the productivity gain we were trying to capture.

If every AI-generated change receives shallow human review, we may ship plausible damage.

If the best engineers become permanent reviewers of generated work, we create a new bottleneck and a professional problem. Reviewing is important, but continuous review of other people’s, or other things’, work is not the same as designing, building, experimenting, and learning. It can become boring, draining, and strangely unproductive-feeling even when it is organizationally necessary.

This is a bad long-term talent strategy. We should be careful before celebrating a world where the reward for expertise is to become the human lint trap at the end of a code-generation hose.

[GitLab’s 2026 AI accountability findings](https://ir.gitlab.com/news/news-details/2026/GitLab-Research-Reveals-Organizations-Are-Generating-AI-Code-Faster-Than-They-Can-Control-It/default.aspx) claim that organizations are adopting AI coding tools faster than they can govern them, and that many now see the bottleneck shifting from writing code to reviewing and validating it. Even allowing for survey and vendor-report caveats, the direction should not surprise anyone who has watched a codebase absorb a flood of locally plausible changes.

When generation becomes cheap, validation becomes the scarce resource.

The way out is not to pretend review is cheap. It is to stop treating review as one undifferentiated activity.

## Generic review gives generic feedback

Ask a human to “review this,” and the result depends on the human, the context, the time available, the reviewer’s mood, the team’s norms, and whether lunch is in the future or the past.

Ask the same human to review specifically for security boundary violations, and they will look differently. Ask for API compatibility. Or error handling. Or performance. Or test oracle strength. Or documentation drift. Or unnecessary duplication. Or whether this is the smallest change that preserves the wrong design.

The reviewer did not become smarter. The review became better aimed.

AI review has the same property.

We do not necessarily need to pit models against each other in a gladiatorial arena of mutually suspicious autocomplete. That may be useful sometimes. But a simpler and often effective approach is to ask for many focused reviews from many perspectives, each with a clear lens and output format.

One review lens examines security and abuse. A conformance lens checks whether the change still satisfies the specification. A design lens looks for superficial fixes where a deeper approach change is needed. A test lens asks whether tests are meaningful or merely aligned with implementation. Reuse review looks for duplication and missed shared utilities. Documentation review checks whether explanations stayed true. Operational review looks at failure modes. Maintainability review asks whether the next person can understand the change without reconstructing the entire conversation.

The point is not that AI will catch everything. It will not. Humans do not either. The point is that review becomes a structured system instead of a heroic act of generic vigilance.

Of course, once review becomes more focused, it also becomes more plural. That is where the next trap waits.

## Perspectives must be reconciled

Focused reviews create a new problem: disagreement.

The security reviewer wants stricter controls. The simplicity reviewer wants fewer layers. The performance reviewer wants fewer allocations. The maintainability reviewer wants clearer structure. The delivery reviewer wants the smallest responsible step. The product reviewer wants the customer-visible fix. The architecture reviewer wants to stop patching the same symptom.

This is not a bug. This is what responsible engineering has always been.

The difference is that AI can generate these perspectives quickly enough that we can afford to hear more of them. But someone still needs to reconcile them. “Run five reviews” is not a decision. It is an input to a decision.

A useful reconciliation process should ask:

- Which findings are confirmed versus speculative?
- Which risks are severe, likely, reachable, and costly?
- Which repairs conflict?
- Which concerns are local preferences rather than system risks?
- Which issues must block the change?
- Which should be recorded as follow-up?
- Which can be safely ignored?

Without reconciliation, review volume becomes another kind of slop.

## Tests are review artifacts too

AI-generated tests deserve special suspicion, not because AI is uniquely untrustworthy, but because tests are already easy to misunderstand.

A test suite can prove that code was executed. It can prove that a few examples behave as expected. It can prove that an implementation and its tests share the same misconception.

It cannot, by existence alone, prove that the real contract is implemented. That is why this connects to [Code coverage is an indicator, not a goal](code-coverage): evidence is useful only when it is evidence of the thing we actually need to know.

Imagine asking for a mathematical expression evaluator. The real contract is “parse and evaluate the expression,” but the visible examples all happen to produce the same result. `2 + 2` should produce `4`. `2 * 2` should produce `4`. `10 - 6` should produce `4`. A bad implementation can always return `4` and pass. That is absurd enough to be funny until one realizes it is merely an exaggerated version of a common failure: tests that encode examples but not intent.

AI is particularly vulnerable to this because it is often trying to satisfy the immediate visible constraints. If tests are available and specifications are weak, it may optimize toward tests. That is not “cheating” in a moral sense. The model has no professional shame. It is completing the task as framed.

The shame, if any, belongs to the workflow that made the wrong target easiest to satisfy.

If tests are one form of review, then review has already escaped the pull request. It needs to move even earlier.

## Review cannot be only after generation

If review happens only after code generation, the organization is paying to detect defects after they have already taken shape.

Some review needs to move earlier:

- Review the specification.
- Review the prompt or task framing.
- Review accepted and rejected examples.
- Review architecture assumptions.
- Review test strategy before implementation.
- Review whether the task is local patching or design repair.
- Review whether the model has enough context to act responsibly.

This is not bureaucracy. It is how one avoids asking the model to drive quickly through fog and then praising the airbags.

## Human review still matters, but differently

None of this removes humans. It changes where human judgment should be used.

Humans should focus on intent, architecture, consequence, taste, ethics, tradeoffs, and final accountability. Humans should inspect the review system itself. Humans should decide which findings matter. Humans should intervene where ambiguity, domain judgment, or organizational context matters.

But humans should not be the only scalable mechanism for catching every duplicated helper, every missing null case, every inconsistent doc fragment, every suspicious test, every shallow fix, every changed behavior, and every forgotten edge case.

That work must be partly automated, partly AI-assisted, partly test-driven, partly conformance-driven, partly documented, and partly human-reviewed.

In other words: software engineering.

We already had practices for mitigating imperfect human output. We should adapt them to imperfect AI output. Specifications, code review, testing, static analysis, conformance suites, ownership boundaries, documentation, release gates, incident review, and operational telemetry all still matter. They may matter more because the production rate of plausible change has increased.

## The real speedup

The speedup is not simply that AI writes code faster.

The better speedup is that AI can help create, challenge, review, test, document, reconcile, and improve the work faster, if the workflow is designed for that. Raw generation is the visible part. The system around generation determines whether the result becomes value or cleanup.

AI did not remove review.

It made review architecture unavoidable.

And perhaps that is not a disappointment. Perhaps it is the useful discomfort we needed. We spent years pretending review was a meeting, a pull request, or a checkbox. AI makes that pretense harder to maintain.

When code arrives faster than understanding, productivity has not improved. The bottleneck has merely moved to the place where understanding was always supposed to live.


## Continue the series

- Previous: [AI Productivity: The Metric Was Already Broken](ai-productivity-metric)
- Next: [The Genie Problem: AI Treats the Unsaid as Negotiable](genie-problem)
- Reading path: after review bottlenecks, continue to [The Genie Problem: AI Treats the Unsaid as Negotiable](genie-problem) to examine why under-specified work produces plausible wrongness.

## References and further reading

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
