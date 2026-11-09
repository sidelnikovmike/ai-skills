# Testing Coach

A skill for Claude based on Roman Savin's book «Тестирование Дот Ком» (*Testing Dot Com*), which many Russian-speaking testers call "the tester's bible". It helps newcomers learning the craft, working testers and QA leads, and people getting ready for their first QA job.

It pairs with [test-automation-coach](../test-automation-coach/README.md). This skill covers what and how to test; that one covers automation.

## What it does

| Mode | Example request |
|---|---|
| **Explain** | "What's the difference between smoke and regression testing?" |
| **Review requirements** | "Check this user story for gaps before we start" |
| **Design tests** | "Write test cases for the delivery cost rule" |
| **Bugs** | "Is my bug report OK?", "The dev says it's not a bug", "It only reproduces sometimes" |
| **Plan and manage** | "Release on Friday, 150 hours of regression, 36 hours available. What do we run?", "Management wants a bugs-per-month KPI" |
| **Career** | "Help me prepare for my first QA interview", "Run a mock interview with me" |

Ask in any language and the skill replies in the same one.

## What changes with the skill

We gave Claude the same five requests twice, once without the skill and once with it. Without the skill Claude already answers well, so the skill adds less here than test-automation-coach does. What it adds is consistent, though.

| Request | Without the skill | With the skill |
|---|---|---|
| **Test cases for a delivery-cost rule with gaps in it** | Finds the overlap and the gap at the boundaries, then writes a good table of tests | Opens with a spec review. Each gap comes with what happens to the user if it ships, plus a business question (what if the actual parcel weight differs from what was entered?). Says to file spec bugs in the tracker and to confirm answers in writing. Tests are prioritized by real order data. Test cases have an IDEA field and check the saved order, not just the screen |
| **A bug report that says "Payment doesn't work!!!"** | A solid critique and a template | Says that if a bug happens 1 time in 3, finding the condition is the tester's job, and gives a plan for it. Points out the most dangerous case to check first: money charged but no order. Adds a test-data block with an SQL check and a one-line justification for the priority |
| **Regression: 150 hours of work, 36 available** | A good plan built on risk | Does the arithmetic per suite: whole priority levels first, then the leftover hours go to the top cases of the next level. Skipped suites move to the front of the next release's rotation. Looks for forgotten changes in dependent features. Gives management four options to choose from, and none of them is overtime |
| **Management wants a bugs-per-month KPI** | A strong answer: Goodhart's law, DDP, a pilot | Adds a release-over-release matrix of escaped bugs (priority × area). Shows that the count says more about the code than about the tester, warns against a mirror KPI for developers, and calls for blameless root-cause analysis |
| **An accountant preparing for a first QA interview** | A good one-week plan | Maps each accounting task to testing language. Every question comes with what the interviewer is really checking. Gives the script "I don't know, but I'll figure it out" for factual questions, and a healthy answer to "can you work weekends?". Never suggests working for free |

## Installation

See the [main README](../../README.md#installation).
