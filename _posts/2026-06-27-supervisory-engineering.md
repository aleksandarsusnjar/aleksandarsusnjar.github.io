---
layout: tagged
title: "Supervisory Engineering: The Work AI Creates While Removing Work"
tags:
    - "AI"
    - "SDLC"
    - "Software Development"
    - "Software Architecture"
    - "Engineering Management"
categories:
    ask-yourself
permalink: /blog/supervisory-engineering
excerpt: >
    AI does not merely reduce work. It moves work toward direction, verification, judgment, learning design, and system stewardship. This post examines why AI is not just a compiler for human intent, why pure management and pure solo engineering both become insufficient, and why a new hybrid role may be emerging.
---

# Supervisory Engineering: The Work AI Creates While Removing Work

> **Series reading path:** 5 of 5: [Previous: Documentation Was the Problem. Now It May Be the Product.](documentation-product). End of series

![Map](/assets/supervisory-engineering/illustration.jpg){: style="float: right; width: 50%; margin-left: 1em;"}
AI is supposed to remove work.

That is the promise, or at least the slide-shaped version of it. The model writes the code, the developer becomes faster, the team ships more, and somewhere in the distance a dashboard turns a confident shade of green.

But what if AI removes one kind of work by creating another?

A [recent longitudinal study of AI coding assistants](https://arxiv.org/abs/2605.23135) describes a shift from creation toward verification activities and names a new category: supervisory engineering work, the direction, evaluation, and correction of AI output. The term is useful because it names what many developers are beginning to feel: the job is not simply becoming less work. It is becoming different work.

Different is not automatically worse.

It is also not automatically better.

## From maker to supervisor

Software engineers have always supervised tools. Compilers, test runners, linters, static analyzers, build systems, deployment pipelines, package managers, code generators, and occasionally suspicious shell scripts have all done work on our behalf.

AI changes the character of the supervision because the tool produces larger chunks of plausible intent-shaped work. It does not merely say, “This line violates a rule.” It says, “Here is the implementation, tests, documentation, and a soothing paragraph about why this approach is robust.”

Now the engineer must decide:

- Did it solve the right problem?
- Did it preserve the design?
- Did it invent a shortcut?
- Did it duplicate existing code?
- Did it write meaningful tests?
- Did it miss a security boundary?
- Did it create future maintenance cost?
- Did it misunderstand the requirement in a way that looks correct?

This is not passive review. It is active supervision.

That sounds like loss only if we assume the old work shape was sacred. It was not. Some of the shift can be genuinely valuable.

## Supervision can be higher-value work

There is a positive version of this story.

If AI handles routine coding, boilerplate, mechanical transformations, test scaffolding, documentation variants, exploratory prototypes, and tedious first passes, humans can spend more time on architecture, product understanding, design, security, tradeoffs, system behavior, and learning.

That is a real opportunity.

A skilled operator, in the stronger sense of someone deliberately directing a powerful machine rather than merely clicking through a tool, can ask AI to survey existing approaches, compare design options, explain unfamiliar technology in project-specific terms, generate examples, review alternatives, and identify relevant patterns from areas the human has not recently studied. This can accelerate learning. It can widen the option space. It can let a human focus on higher-level consequences while the model explores lower-level detail.

This is not merely delivery acceleration. It is cognitive augmentation.

The best outcome is not that humans type less while doing the same thinking. The best outcome is that humans think at a higher level because some lower-level exploration has become cheaper.

But the same shift has a shadow. Higher-level work is not guaranteed merely because lower-level work was delegated.

## Supervision can also become botsitting

There is another version.

The engineer becomes a minder of generated work. The model produces. The engineer checks. The model changes. The engineer checks again. The model forgets context. The engineer re-explains. The model fixes a symptom. The engineer asks for the root cause. The model apologizes with the emotional sincerity of a toaster and tries again.

After enough cycles, the engineer may not feel augmented. They may feel like they are watching a very fast associate with perfect confidence, no memory of yesterday, and access to the production codebase.

That is not an insult to associates, juniors, or interns. Many of them learn quickly, and some are astonishingly capable. The point is not seniority. The point is accountability, memory, ambiguity, and whether the work product can be trusted without rebuilding the reasoning behind it.

The danger is not only productivity. It is skill growth, ownership, motivation, and professional identity. Humans learn deeply by doing. They learn by struggling with design, making mistakes, debugging consequences, and feeling the shape of a system through direct work. If their role shifts too far toward reviewing generated artifacts, what happens to that learning path?

We should not assume the answer is fine because the throughput graph improved.

This is where engineering discomfort and management reality need to meet instead of talking past each other.

## This is not the same as trusting a compiler

One tempting analogy says we have seen this before. Programmers once wrote assembly. Then we trusted compilers. Now perhaps we will trust AI to write source code while we operate one level higher.

There is a useful hint in that analogy, but it breaks at the most important point.

Compilers translate from a strict source language into machine code under a defined language specification, target architecture, and implementation. Compilers can have bugs, but once those bugs are fixed, they are expected to be stable and repeatable. Given the same source, compiler version, flags, target, and environment, the output is not a fresh interpretation of what the programmer probably meant.

AI coding assistants are not compilers for human intention. Human language is ambiguous. Project context is incomplete. Requirements hide assumptions. The same request can produce different code on another run, and both outputs may look plausible. The model is closer to a sidekick or associate writing source code from instructions than to a deterministic compiler translating source code into machine code.

That distinction matters. Moving one level up is real. Pretending the lower level became compiler-like is not.

## Management has lived in the grey area for years

Engineers often want software to be correct. This is admirable and occasionally inconvenient.

Managers and leaders live in a world where systems are imperfect, people are uneven, processes leak, customers are impatient, budgets are finite, and “good enough” is not a moral failure but a survival concept. They already think in terms of risk, thresholds, controls, incentives, capacity, defects, customer value, and tradeoffs.

This matters because AI-generated work is not alien to management. It is another imperfect production source. [DORA’s 2025 AI-assisted development report](https://dora.dev/research/2025/dora-report/) frames AI as an amplifier of organizational strengths and weaknesses, which is exactly why the management system around the work matters.

Human teams already produce imperfect work. We mitigate that with design review, code review, tests, release gates, observability, incident response, documentation, ownership, training, standards, and occasionally a calendar invite nobody wants. AI-generated work needs analogous controls.

The mistake is to expect AI output to be perfect because it came from a machine, or to reject it because it is imperfect. We never had perfect humans. We had systems for managing human imperfection.

Now we need systems for managing mixed human-AI imperfection.

## The bottleneck moves upward

As AI handles more low-level work, the bottleneck can move upward:

- from typing to specifying
- from implementation to design
- from writing tests to defining oracles
- from producing code to governing provenance
- from knowing syntax to knowing consequence
- from doing one task to coordinating several agents
- from local correctness to system coherence.

This is where experienced engineers may become more valuable, not less. But only if organizations understand what expertise is now doing. This connects to [The Limits of Influence](influence): expertise is not only possession of knowledge, but the ability to apply, delegate, teach, and shape decisions across a system.

If expertise is reduced to “review AI output,” it will feel like cleanup duty. If expertise becomes “design the system of constraints, reviews, context, and decisions through which AI can safely accelerate work,” it becomes leverage.

That distinction matters.

## The human should not disappear from the work

There is a tempting but dangerous management interpretation: if AI writes code and humans supervise, perhaps fewer engineers are needed.

There is an equally tempting engineering interpretation: if engineers can direct AI to do more of the work, perhaps many pure management roles become less useful.

Both reactions are understandable. Both are also incomplete. Before celebrating either disappearance, ask which humans you are removing from which learning loops.

Are we removing junior engineers from the implementation struggle that teaches judgment? Are we removing senior engineers from the direct contact with code that keeps architecture honest? Are we removing managers from the work-shaping knowledge needed to make responsible tradeoffs? Are we removing everyone from the shared understanding that lets a team know when the model’s answer is locally plausible and globally harmful?

Who will become the next person capable of supervising if people never build enough to understand what they supervise? Who will recognize the architectural smell that does not fail tests yet? Who will maintain judgment when the craft is outsourced before the craft is learned?

If AI changes the work, leadership must redesign growth paths. Pairing, review, rotations, guided implementation, design exercises, incident analysis, and explicit teaching may matter more, not less. Otherwise the organization may preserve today’s experts by consuming tomorrow’s.

That is not productivity. That is liquidation.

## Supervisory engineering needs tools

A supervisor of AI output needs more than chat.

They need:

- specifications that define intent
- tests that check contracts rather than examples only
- provenance tracking for generated artifacts
- review lenses and reconciliation rules
- documentation that stays current
- architecture maps
- conformance suites
- dashboards that show review burden and rework
- policy around when AI may act independently
- clear escalation paths for ambiguous decisions.

Without these, supervisory engineering becomes ceremony with a merge button.

## What do we call the new role?

The future I want is not one where humans become passive approvers of machine output.

It is also not one where humans heroically refuse tools that can remove drudgery, accelerate learning, and widen the option space.

The better future is one where AI handles more of the mechanical and exploratory work while humans become better at intent, design, tradeoffs, verification, and judgment. That requires documentation, measurement, review architecture, and leadership that understands the difference between visible activity and value.

But this also means the old role split becomes less useful. Pure management, detached from how the work is shaped, is not enough. Pure “I will do it all myself” engineering leaves leverage on the table. The emerging role sits between and above both: part engineer, part architect, part reviewer, part teacher, part product thinker, part systems gardener, part risk manager. It is not merely management of people, and it is not merely production of code. It is design of the human-AI work system.

Maybe we will call it supervisory engineering. Maybe that name will not survive. Names often arrive after the job already changed.

What matters is the question underneath the name:

**Who designs the system in which AI can safely help?**

Done well, this is not less engineering. It is engineering one level up.

Done badly, it is botsitting until everyone involved forgets why the code exists.

That distinction is worth measuring.

## Continue the series

- Previous: [Documentation Was the Problem. Now It May Be the Product.](documentation-product)
- Full path: start with [AI Productivity: The Metric Was Already Broken](ai-productivity-metric) if you want the measurement argument from the beginning.

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
