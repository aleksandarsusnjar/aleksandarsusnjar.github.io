---
layout: tagged
title: "AI Productivity: The Metric Was Already Broken"
tags:
    - "AI"
    - "SDLC"
    - "Software Development"
    - "Software Architecture"
    - "Engineering Management"
    - "Metrics"
categories:
    ask-yourself
permalink: /blog/ai-productivity-metric
excerpt: >
    AI coding tools can make software artifacts appear faster than ever, but artifact speed is not the same as net value. This post asks whether AI is helping us create more value than total cost, including quality issues, review burden, delayed harm, infrastructure, and human effort. It connects AI productivity claims to older metric traps: velocity that is not value, coverage that is not confidence, and roadmaps that are not maps.
---

# AI Productivity: The Metric Was Already Broken

> **Series reading path:** 1 of 5: Start here. [Next: The Cleanup Tax: AI Did Not Remove Review. It Moved the Bottleneck.](cleanup-tax)

There is a question being asked in boardrooms, engineering all-hands, budget reviews, vendor demos, hallway conversations, and probably a few slightly awkward one-on-ones:

**Is AI making software developers more productive?**

It sounds like a straightforward question. It has that appealing managerial shape: before, after, compare, decide. Buy more licenses or fewer. Hire differently or not. Mandate use or merely encourage it. Put a number on the slide and move on.

Unfortunately, software development has spent decades being impressively resistant to that kind of measurement. AI did not create this problem. It merely walked into the room, turned the lights up, and made the dust visible.

We already had velocity that was not speed, story points that were not value, code coverage that was not confidence, lines of code that were not output, utilization that was not effectiveness, and roadmaps that were sometimes less map than decorative calendar. This is the same family of problem I explored in [The Metrics Maze](kpi), [Code coverage is an indicator, not a goal](code-coverage), and [That's not a Road Map](roadmap): the moment a proxy becomes the goal, the proxy starts lying more fluently. Then AI arrived and offered something new: the ability to produce far more visible software artifacts, far more quickly, with far less typing.

That looks like productivity.

At least, it looks like productivity if we were already prepared to confuse productivity with artifact volume. If we were not, the question becomes harder, and more interesting.

## The demo is not lying. It is just incomplete.

The honest case for AI-assisted software development should not be dismissed. In [a controlled GitHub Copilot experiment](https://arxiv.org/abs/2302.06590), developers assigned a small JavaScript HTTP-server task completed it 55.8% faster with Copilot than without it. That is not a rounding error. It is the sort of number that makes budget conversations shorter and vendor conversations louder.

Then [METR studied experienced open-source developers](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/) working in mature repositories they knew well. In the early-2025 randomized trial, the AI-allowed condition took 19% longer. That is not a rounding error either. [METR later warned](https://metr.org/blog/2026-02-24-uplift-update/) that those historical results were already out of date and that later study designs were becoming harder to interpret because developers increasingly did not want to work without AI, selected tasks differently, and used multiple agents concurrently.

So which is it? Does AI make developers faster or slower?

That is the wrong first question.

A toy or bounded task can expose raw generation advantage. A mature repository exposes context, maintenance, design history, local conventions, fragile tests, implicit knowledge, and the annoying habit of reality to already exist before the tool joins the project. These are not contradictory worlds. They are different measurement frames.

If we measure typing, AI wins quickly. If we measure task completion in a context-rich system, the answer depends on the task, person, tooling, review cost, quality expectations, documentation, test strategy, and how much hidden system knowledge must be reconstructed.

Already the clean question has become less clean. Good. Now we are getting closer to software.

That distinction matters because the easiest numbers to collect are precisely the ones AI can inflate fastest. [DORA’s 2025 report](https://dora.dev/research/2025/dora-report/) makes a related point from another angle: AI tends to amplify the organization it enters, including its strengths and weaknesses.

## Activity is not productivity

The most dangerous productivity metric is usually the one that looks easiest to collect.

Lines of code? More can be worse. Less can also be worse if it is clever, compressed, unreadable, or impossible to maintain. The goal is not “more” or “less.” The goal is the irritatingly human one: just enough, shaped well enough, in the right place, for the right reason.

Commits? A developer can create many. A developer can create one. Neither tells us whether the system improved.

Pull requests? We can slice work until the metric smiles and the architecture weeps.

Story points? They are effort-ish, uncertainty-ish, complexity-ish team-local planning aids that somehow keep being invited to executive conversations wearing a fake mustache labelled “business output.”

Velocity? Velocity is useful if a team uses it carefully for its own planning. It becomes much less useful when treated as a universal measure of productivity across teams, time, or process changes.

AI makes all of this more awkward. If a model can generate twenty files in the time it once took a developer to write one, activity metrics may improve dramatically while value remains unchanged, quality declines, or review burden moves elsewhere. We may have moved the work from writing to reading, from implementation to validation, from code creation to code custody.

The [SPACE framework](https://queue.acm.org/detail.cfm?id=3454124) was created partly to counter this single-metric trap. It explicitly separates satisfaction and well-being, performance, activity, communication and collaboration, and efficiency and flow. [DevEx work](https://queue.acm.org/detail.cfm?id=3595878) similarly emphasizes that developer productivity is affected by friction, cognitive load, feedback loops, clear goals, code organization, releases, and psychological safety. None of this becomes less true because an AI assistant autocompletes a function.

If anything, it becomes more true, because AI can inflate activity faster than humans can reinterpret what that activity means.

If artifact volume fails, the next refuge is usually effort. That refuge is safer, but not safe enough.

## Effort is not output either

Perhaps we can avoid artifact-volume traps by measuring effort. After all, effort estimation has been part of software planning for a long time. Teams estimate work. They compare planned work to completed work. The process is imperfect, but at least it is human-shaped.

That helps until AI enters the estimation culture.

Early in AI adoption, a team may estimate work under pre-AI assumptions and complete it with AI assistance. That can look like a productivity gain. Some of it may be. But over time, estimates adapt. People learn what the tool can do. They begin assuming AI help. The unit silently changes.

Now the metric that seemed to show improvement normalizes. The underlying system may still be producing more value. Or less. Or the same visible value with more hidden harm. The estimate no longer tells us what changed because it absorbed the change.

There is another problem. Effort is not output. Small output can require large effort. Large output can require little effort. The relationship depends on architecture, clarity, prior investment, team knowledge, codebase health, and whether the work is fighting or flowing with the system.

A bad design can make simple work expensive. A good design can make valuable work look easy. If we then measure only effort, we may punish the people who made future work easier and reward those who heroically survive preventable complexity.

AI adds another layer: the human may spend effort specifying, prompting, reviewing, testing, reconciling, and correcting while the AI spends compute producing artifacts. The human may also think while walking the dog, staring suspiciously at a test failure, or realizing in the shower that the model patched the symptom and preserved the disease. Good luck fitting that cleanly into “time spent producing code.”

So we have to climb higher. The climb helps, but it does not make the ground disappear.

## Value is the higher abstraction, but it is not magic

If effort and activity are insufficient, value sounds like the answer.

It is closer. It is not easy.

A better direction starts somewhere else: with outcome value, not effort optics. It allocates value across visible features and enabling work, records quality damage as cost rather than trivia, compares results over long horizons, and tracks where human and AI involvement contributed to both benefit and harm. That still does not make the problem easy. It merely stops pretending the easy proxy was the answer.

But value has its own traps.

Business value can be subjective. It can be politically inflated. It can change over time. It can be delayed by prerequisite work. A foundational platform change may look low-value until it enables three high-value features. A small visible feature may carry high customer value but depend on years of invisible infrastructure. A work item may contribute to multiple outcomes. A feature may produce recurring value, one-time value, strategic option value, risk-reduction value, or mostly the comforting illusion of progress.

So the answer is not “just use business value.” The answer is to treat value estimation as a model, not as divine revelation.

Estimate value at the larger feature or outcome level. Distribute that value across the work that contributes to it, including enabling work. Allow one work item to receive value from more than one outcome. Track recurring value separately from one-time value. Compare estimates to real outcomes later. Use multiple independent estimators where possible to reduce peer pressure and political distortion.

This is harder than counting pull requests.

That is not a flaw. That is the point.

The simpler metric was simpler because it ignored the thing we actually cared about.

## Quality issues subtract from value, often with a delay

There is a common productivity illusion: value is counted when the feature ships, but the cost of quality issues is counted only when someone complains loudly enough.

That is convenient. It is also wrong.

Quality is positive. It is part of why the value exists in the first place. Quality issues are different: they are costs, risks, and value erosion that often arrive late. They belong in the total value proposition with the other expenditures: human salaries, infrastructure, AI tooling, review time, operational burden, support cost, and recovery work.

Quality issues are not merely defects in a tracker. They are lost trust, support burden, incident response, rework, slowed future development, operational risk, security exposure, and customer hesitation. The cost of fixing them usually grows with time. A defect found while the code is still warm is one thing. A defect found after documentation, tests, integrations, customer workflows, and team assumptions have grown around it is another.

It helps to treat quality issues as accumulating harm or cost. That lets us subtract something from apparent value, or at least track the two independently. A feature that creates customer value while quietly producing maintainability damage is not simply valuable. It is a mixed asset.

This matters for AI because AI can produce plausible artifacts quickly. It can also produce plausible defects quickly. Some defects will be obvious. Others will be structurally polite, idiomatically formatted, test-passing, and quietly wrong.

If our productivity measurement counts the production immediately and the quality costs later, the tool will look better at the exact moment we understand it least.

## Contribution is not authorship

To measure AI’s effect, we need some idea of AI contribution. That sounds simple until one tries to define it.

Is the AI contribution the time it spent generating code? That undercounts it because the model is fast. Is it the amount of code it produced? That inherits all the artifact-volume problems. Is it the direct subscription or token cost? That ignores human expertise, review, infrastructure, and long-term maintenance. Is it the percentage of a story? A story’s value is not usually divided by who typed which characters.

Modern AI-assisted work is mixed. A human frames the problem, identifies risks, prompts, rejects, edits, asks for alternatives, reviews, writes tests, deploys, troubleshoots, and later repairs. The model generates code, tests, docs, explanations, suggestions, and sometimes entirely new confusion with impressive confidence.

Contribution should be tracked for both positive value and negative harm. If AI helped create a feature, record that. If AI-originated code later causes a defect, record that too. If a human fixes AI-generated code, the fix does not erase the original cause. If AI fixes human-generated code, the fix does not make the original problem AI-caused.

This needs instrumentation. Commit-level or document-version-level tagging may be a practical start. Finer attribution would require understanding moves, copies, refactorings, partial edits, generated text, later human modifications, and artifact lineage. It is non-trivial.

It also must not become performance surveillance disguised as research. The purpose is to understand the system, not create a new way to punish developers for either using or not using a tool that the organization itself introduced.

## Time horizon is not a detail

Short-term AI productivity signals are noisy. The first week may show excitement. The second month may show dependency. The third quarter may show maintainability. The second year may show whether the organization learned anything.

Some effects are immediate: faster boilerplate, easier exploration, lower friction for unfamiliar APIs. Some take time: skill erosion, review fatigue, documentation drift, duplicated code, inconsistent patterns, developer satisfaction changes, trust changes, security exposure, and the gradual normalization of a new way of working.

[METR’s 2026 update](https://metr.org/blog/2026-02-24-uplift-update/) is instructive here not because it gives us a final answer, but because it shows how the measurement problem itself changes as AI adoption changes. Developers increasingly self-select tasks, decline no-AI conditions, work concurrently with agents, and experience quality differences between conditions. The measurement system becomes part of the phenomenon being measured.

That should make us humble. It should not make us helpless.

## The better question

So, is AI making software developers more productive?

Maybe. Often. Sometimes dramatically. Sometimes not. Sometimes it makes local work faster and global work more expensive. Sometimes it helps weaker developers build things they could not build before. Sometimes it slows experts who already know the codebase better than the model can reconstruct it. Sometimes it creates work that would not otherwise have been attempted. Sometimes it creates work that should not have been attempted.

If that sounds unsatisfying, good. It means the question is no longer doing too much work.

The better question is simpler:

**Are we creating more value than harm?**

That is the question I would put on the wall. The longer version still matters: over what time horizon, with what review cost, with what maintenance burden, with what quality risk, compared to what baseline, and with what effect on human learning and judgment? But the memorable center is value versus harm.

Value is not only delivery speed. It can be faster learning, better exploration, earlier discovery of existing approaches, improved documentation, broader design options, lower friction for small tools, and the ability for humans to spend more attention on higher-level consequences. AI may help not only by typing code faster, but by changing what humans can afford to think about.

Harm is not only defects. It can be review load, spaghetti code, duplicated helpers, security exposure, technical debt, lost ownership, stale documentation, skill erosion, or a system that appears to move faster while becoming harder to steer.

So yes, ask whether AI helps. But do not ask it as a speed question alone.

Ask whether the whole system is producing more durable value than accumulated harm.

The measurement problem was already there. AI did not break it. AI merely made the brokenness productive enough to matter.

## Continue the series

- Next: [The Cleanup Tax: AI Did Not Remove Review. It Moved the Bottleneck.](cleanup-tax)
- Reading path: after the measurement problem, continue to [The Cleanup Tax: AI Did Not Remove Review. It Moved the Bottleneck.](cleanup-tax) to examine the review bottleneck that raw generation speed creates.

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
