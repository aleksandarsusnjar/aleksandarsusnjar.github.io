---
layout: tagged
title: "Code coverage is an indicator, not a goal"
tags:
    - "Software Development"
    - "Quality"
    - "Testing"
categories:
    ask-yourself
permalink: /blog/code-coverage
excerpt: >
    This post challenges the misconception that higher code coverage equals better testing. We highlight how blindly pursuing 100% coverage can lead to unnecessary complexity and dead code, emphasizing that the real goal is to align tests with the actual needs of the code, not just increase coverage for its own sake.
---

The typical message "out there" often presents code coverage as something that should ideally be 100%. That part is fine. What's not fine, however, are the assumptions and suggestions about how to achieve this, based on the misconception that code coverage directly reflects the quality of your testing. While there's some connection, it doesn't quite work that way. Let's take a closer look at code coverage, break it down, and understand what it truly represents.

**Unit**

Let's start with the "nice" case. Code coverage typically measures the portion of the desired and necessary code that gets exercised during testing. But how much of that code is covered, and how much is missing? Consider the following code:

```javascript
if (condition 1) {
    instructionA1();
    instructionA2();
    instructionA3();
    …
    instruction161();
}
else if (condition 2) instructionB1();
else if (condition 3) instructionC1();
else if (condition 4) instructionD1();
else if (condition 5) instructionE1();
else if (condition 6) instructionF1();
else if (condition 7) instructionG1();
else if (condition 8) instructionH1();
else if (condition 9) instructionI1();
else /* condition 10 */ instructionJ1();
```

This example highlights a well-known and non-controversial issue. Suppose the tests only exercise "condition 1" and ignore the remaining 9 conditions. This would cover 10% of the branches. However, note that the condition being tested contains the most instructions  -  161, in addition to the condition check. If we count each condition check as a single instruction, that's 162 instructions exercised out of 180 total, which yields 90%. So, is this 10% or 90% coverage, or neither, since it doesn't account for real-world occurrences?

In some cases, branches are hidden and can be expressed as branchless functions. Consider the following two approaches for a function returning the absolute value of a number:

1. `(x >= 0) ? x : -x`
2. `x * sign(x)`

Assume `sign(x)` itself is branchless and uses bitwise operations with the sign bit to calculate its value. If `sign(x)` returns -1, 0, or +1 depending on the value of `x`, this works. However, if we're wrong about it and, for example, it literally returns the raw value of the sign bit extended, it may return -1 for negative values but 0 for all others, including positive values - and that wouldn't work. If our test runs only a single negative value case, it will pass (100%), covering 100% of the instructions and branches for implementation (2), but not for (1). 

If you think this only happens with trivial cases like this, you'd be mistaken. As cases become more complex, there are more opportunities for a single apparent branch to miss something expected, requiring further breakdown one way or another.

At the very least, this should shake any belief that you can simply pick a target code coverage percentage (anywhere from 10% to 90% in the first example) and be content with it - or that there's certainty associated with 100%. What we're left with is that a higher coverage number is better than a lower one, but only if measured the same way.


# Missing Coverage

What can we do to increase coverage? A typical suggestion I've observed is to identify the code (branches and/or instructions) that aren't covered and write new tests to exercise those. While doing this, the following may happen:

- You cover new, previously uncovered cases that are very much needed.

- You cover new, previously uncovered cases that may not be needed immediately but could be if we extrapolate the intent of the code.

- You cover new, previously uncovered cases that will never be exercised in production, but you don't realize it because of constrained visibility. You end up covering dead code while adding complexity for future enhancements that must adhere to artificial constraints imposed by the new tests.

- You discover that it's impossible to reach a particular case at all. You're left wondering whether this is a defect or dead code that could be removed.

- You remove the uncovered code, effectively increasing the coverage percentage while all tests continue to pass. But you risk production failure if that code is still relied upon in any way.

Here's a bonus question: is that code truly not covered, or is its coverage simply not measured because you only consider a subset of test types? Either way, it should be clear that the missing coverage doesn't necessarily point to missing tests - it might indicate the exclusion of tests or dead code that shouldn't be there in the first place. Blindly chasing a greater code coverage percentage will lead you down rabbit holes of unnecessary complexity, where no one benefits and everyone suffers, all while hiding dead code. That brings us to…


# False Coverage

The pursuit of ever-greater code coverage, without considering the true reasons behind it, often leads to artificially inflated coverage percentages with code that shouldn't even exist in the first place. We're talking about:

- Remnants of the past that are no longer in use.
- Extrapolations that will never be used.
- Coded misunderstandings.
- Downright defects. After all, to err is human.

Covering any of these simply turns unwanted code into enforced code, all while continuing to pay the price of maintaining it. Worse, these unwanted behaviors may end up being relied upon if someone "draws inspiration" from those blind coverage tests, inadvertently creating unintended coupling. Such couplings make it harder to evolve the true intent, as the system must remain compatible with the literal abuse.


# Code Uncoverage

Code coverage is an indicator, a metric, and a tool to help us make decisions. It should never be viewed as a direct goal. It doesn't tell us how good our tests are, nor how much of what we need, or even expect, is covered by them. Think about it: tests are there to check if the code performs as expected. In that sense, our real goal should be 100% "***need*** coverage". You might encounter "needs" expressed differently in your context, or not expressed at all. In the past, these were often referred to as "specifications", a practice that seems to have largely disappeared.

What I often see instead is code being written from somewhat vague user stories and/or requirements, followed by the expectation that the same developers who wrote the code should also write the tests, with little opportunity for debate. This approach results in all the misunderstandings, misconceptions, and reasoning errors that developers have after completing the work being coded as the desired behavior for the future. Yes, developers will discover some defects along the way and fix them, but that's not the whole story.

So, what does this mean? It means someone else is needed to cover the blind spots and errors of the code developer. Code reviews aren't enough - if they were, we wouldn't need tests in the first place. But who should these additional testers be? They shouldn't be influenced by the code developer, as they might share the same misconceptions. Instead, they should bring different perspectives - and therefore different misconceptions. If the misconceptions don't align, the result will be better testing. If tests fail because of these differing perspectives, it indicates that something needs to be corrected, typically by clarifying the foundational specifications. All good stuff!

Note that Test-Driven Development (TDD) doesn't address this issue. In some ways it augments it by suggesting that the same developer writes both tests and main code. While TDD guides developers to work in a particular way that improves some aspects, it may also sacrifice others. Whether TDD is suitable for your case is a separate decision. It doesn't directly tackle the coverage issues described here, as it mainly just flips the order of coding and testing - both done by the same person. In fact, TDD may even inflate coverage numbers by achieving near-100% coverage with code that would never be exercised in production.

So, what's the conclusion? Less than 100% coverage helps you uncover code that you either don't need or don't test. The key is to investigate which of the two it is - not to assume either way. The covered code doesn't tell you much either. It could all be unnecessary, enforced legacy code. 100% coverage may seem like the ideal, but even with passing tests, it doesn't guarantee that you've covered all the cases, especially with hidden branches. You should absolutely look at code coverage, but be sure to interpret it wisely.
