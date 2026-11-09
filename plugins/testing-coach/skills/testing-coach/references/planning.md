# Planning and managing testing: estimation, test plan, regression, metrics

Use when a tester or lead asks how to estimate testing, whether and how to write a test plan, what to put into a regression run that doesn't fit the time, what to do about an ever-growing regression, whether to automate it, or how to measure testing and testers.

Savin's stance throughout: honest numbers from the tester, the trade-off decision from management; prioritise ruthlessly because there is never enough time; judge testing by what users find, not by what testers found.

## Terminology map (recognise Savin's, answer in modern terms)

| Savin | Modern term | Note |
|---|---|---|
| «тест приемки» | smoke test / build verification | NOT acceptance testing in the ISTQB sense |
| «тест сдачи» | final acceptance / certification check | done in the pre-release buffer |
| «тест-смета» | test estimate | |
| «тест-комплект» | test suite (in a TMS) | |
| «продюсер» | product manager / product owner | |
| «замораживание кода / спеков» | release branch cut, feature-complete build; stories refined and accepted | |
| «тестирование новых фича» → «регрессивное тестирование» | stage 1: new-feature testing, stage 2: regression | Savin's two stages of execution |
| «проблема противоречия» | regression growth vs fixed capacity | |

If the user writes in Russian with Savin's words, use them back; otherwise use the modern term and map once if useful.

---

## 1. Execution stage 1: new-feature testing

Entry: code integrated and frozen (today: release branch cut or feature-complete build on the test env), smoke check passed.

**Smoke checklist («тест приемки»).** Don't do it ad hoc. Keep a short written list of the most basic checks (sign up, log in, open the main page, run the core user action, save something) and walk it line by line on every new build. The build is accepted for testing only when every line passes; a failed line returns the build to the devs. Modern form: the same list as a CI smoke job plus a 5-minute manual pass on what isn't automated.

**2-minute feature presentation.** At the end of test preparation or the start of execution, each tester presents their new feature to the rest of the team in about two minutes: what it does, what it touches, where the risks are. Everyone then knows what is coming and who owns what, which helps with bug routing and with picking regression groups (section 5). Modern form: a slot in sprint planning or the release kick-off.

Then the work itself is deliberately unglamorous in Savin's telling: run the cases, file bugs, argue priorities, verify and close.

---

## 2. Test estimate («тест-смета»)

Purpose: reconcile what the business wants with what testers can physically do. It is a negotiation input, not a promise.

**An estimate is two numbers:**
1. **Preparation**: writing test cases, test data, test tools.
2. **Execution of new-feature testing**: running those cases, including retests.

Regression is planned separately (section 5) from what's left of the execution window.

**Procedure**
1. The spec/story is written; the lead asks the tester to estimate.
2. The tester reads it, then **talks substantively to the PM and the dev** (what's really changing, what's risky, what's unclear). An estimate from reading alone is like kung fu learnt from a book: real work uncovers nuances you couldn't see.
3. Estimate from that plus experience, consult colleagues who tested something similar.
4. **Add 10%** for the unforeseen (e.g. an indecent number of blocking bugs).
5. Hand both numbers to the lead, **with the leftover capacity shown**.

**Five factors to price in**
1. **Complexity** of the feature: more nuances, more time.
2. **Your experience** with similar features: a domain veteran is faster than a newcomer.
3. **Track record of this dev and PM.** Some ship clean, unit-tested code and cooperate; others throw code at the wall to see what sticks. Budget more for the latter. Savin says it openly; say it tactfully, as "history of returned builds and reopened bugs" rather than a judgement of the person.
4. **Third-party / vendor integration latency.** When their test system misbehaves, the chain is tester → our PM → vendor contact → their dev, who may be busy. You can't give orders to another company's devs. Always add time for this.
5. **Test tools needed?** Add the time to build *and test* the tool, subtract the time it realistically saves during execution.

**Show leftover capacity, leave the trade-off to management.** Worked example (fresh numbers): a 2-week window per phase, one tester, 80 h each.
- Preparation: 38 h cases + 12 h data tool = 50 h, +10% = 55 h. 25 h spare → reserve or helping colleagues.
- Execution: new features 46 h, +10% ≈ 51 h. **29 h left for regression.**
- If the regression selection (section 5) needs 60 h, report: "Regression needs ~60 h, we have 29. Options: cut scope, move the date, add a tester from another team, or run a reduced regression; here is what the reduced run skips." Testers give honest numbers; the business call belongs to management. Don't silently absorb the gap with overtime.

Default LLM tendency to avoid: leading with PERT/WBS/test-point analysis. Those are fine add-ons; Savin's core is the conversation, the two numbers, the people factors and the visible leftover.

---

## 3. Entry/exit criteria

Each of the two phases has its own start and end condition, four in total:

| Phase | Entry | Exit |
|---|---|---|
| Test preparation | specs frozen (stories refined and accepted) | test cases, data and tools ready |
| Test execution | code frozen (release branch / feature-complete build on test env, smoke passed) | new-feature testing and regression done, **no open P1 and P2 bugs** |

- "No open P1/P2" is the release gate. P3s should normally be fixed too, but they don't block; any shipped P3 needs an explicit owner and date for the fix.
- Modern equivalents: Definition of Ready / Definition of Done, CI quality gates. The principle is the same: each phase starts and ends on a stated condition, not on a mood.

---

## 4. Test plan

**Hierarchy:** test case = compares actual vs expected; suite = container for cases; **test plan = the document that generalises and coordinates testing.** In practice "test plan" means different things in different companies (a suite, a list of spec excerpts, notes in the margins); clarify which one the user means.

**When to write one.** Savin writes a plan **only when he leads a project with several testers**. Working alone, put the useful bits (what's not tested, assumptions, dependencies) into the suite headers instead. You never need every element; a trimmed plan is normal. Push back on a default 20-section IEEE 829 / ISO 29119 template for a one-person feature.

**Elements worth having (compressed)**
- **What is tested**: features/stories, each with a priority; source docs with their priorities.
- **What is NOT tested, and why.** E.g. a spec carries performance requirements, but there is no perf engineer or tool and none planned: state that performance is out of scope for lack of resources. This protects the team and forces a conscious decision.
- **Scope / types of testing**: e.g. "system test covers the whole flow from UI action through DB values to the partner's sandbox response".
- **Test documentation and tools** to be produced.
- **Entry/exit criteria**: all four (section 3).
- **Assumptions**: e.g. the release branch is cut on the agreed date.
- **Dependencies**: e.g. new test devices, a vendor sandbox account, a licence for a region.
- **Suspend/resume thresholds**, with hysteresis: stop testing when open P1s reach a level where further testing is pointless (e.g. 7), resume only after fixes bring it down (e.g. 2). Different numbers prevent stop-start flapping.
- **People**: everyone involved incl. vendor contacts, with duties; any training needed first.
- **Schedule milestones**: spec freeze; prep start/end; code freeze; new-feature testing start/end; regression start/end. **End regression 2–3 days before release.** If estimates were wrong, that is the slack; if right, use it for the final «тест сдачи» check with areas split among testers.
- **Risks and mitigations**: Savin's example mitigation is late nights and weekends; don't use that. Modern risk sections cover product risk as well as schedule risk, and mitigate with scope cuts, priority cut-offs and date moves.
- **Sign-offs**: at minimum the tester's manager, the tester, the PM and the dev. The more people signed, the more the plan is shared responsibility. Today: approvals on the Confluence/Notion page or PR.

**Compact template**
```
Test plan: <project/release>   Author: <name>   Version: <n>
1. Context: what the project is and why (2–3 lines); links to specs/epics with priorities
2. In scope: feature — priority — test types
3. Out of scope: item — reason (resource / risk accepted / covered elsewhere)
4. Approach: types of testing, environments, test data, tools to build
5. Entry/exit: prep start/end; execution start/end (exit: no open P1/P2)
6. Suspend at: <N> open P1   Resume at: <M> open P1
7. Assumptions / Dependencies
8. Schedule: spec freeze · prep · code freeze · new features · regression · buffer 2–3 d · release
9. Risks → mitigation (no "overtime" as the plan)
10. People and responsibilities (incl. vendor contacts)
11. Sign-off: QA manager · tester · PM · dev lead
```

---

## 5. Regression (execution stage 2)

Savin's "regression stage" = checking that the release didn't break old features. (He also uses «регрессивное тестирование» for re-checking a bug fix; don't mix them up.) Two problems: which old suites to run within the time box, and what to do as the pool grows every release (the snowball).

### 5.1 Three-group selection
1. **Group 1 — the changed component.** All suites of the component where the new features live. If the dev broke something, it's most likely here.
2. **Group 2 — old features that depend directly on it.** Key insight: these catch not only accidental breakage but **forgotten required changes**: the dependent code also needed updating and neither PM nor dev noticed. Savin's case: per spec, one component changed its cookie format; the consumer of that cookie wasn't updated, couldn't read it, and the main purchase flow died.
3. **Group 3 — everything else, by rotation.** Anything can be hit, but you can't run everything each time. Run a slice per release so every suite runs at least once per period. Arithmetic: suites in group 3 ÷ releases per period = suites per release. Savin: 45 suites, monthly releases, 15 per release → full coverage every 3 months. With 60 suites and releases every 2 weeks, 10 per release cover everything in a quarter. If time is short, run only the top-priority cases of this release's slice.

Group 1 normally ranks before group 2. Modern aids (test-impact analysis, code-owner maps, dependency graphs) feed groups 1–2; the reasoning stays the same. Savin calls the final list a management prerogative; today QA usually owns it, with the lead accountable.

### 5.2 Prerequisite: per-suite duration data
You cannot plan a time box without knowing how long each suite takes. Record run time per suite (or per case) after every run, in the TMS or the suite header. It's approximate and depends on who runs it (someone who has run a complex suite before is faster). If the user has no such data, ask for estimates or start recording now, before promising a regression plan.

### 5.3 Fitting the time box
1. **Capacity** in person-hours: testers × days × productive hours. No overtime in the base plan.
2. **List candidate suites** with group, hours, priority (1 = highest).
3. **Include whole priority levels** cumulatively while the sum fits.
4. **Fill the leftover** with the highest-priority *cases* from the next level's suites, spread across them, rather than one whole suite. Savin calls this the best option.
5. Whatever didn't fit goes to the front of the next release's rotation and is stated in the report.

**Worked example** (fresh numbers): change in the search component; 3 testers × 4 days × 7 h = **84 h**.

| Group | Suite | Hours | Priority |
|---|---|---|---|
| 1 | Search core | 12 | 1 |
| 1 | Search filters | 16 | 2 |
| 1 | Sorting and result UI | 10 | 3 |
| 2 | Saved searches | 9 | 1 |
| 2 | Recommendations | 18 | 2 |
| 2 | Analytics export | 20 | 3 |
| 3 (rotation) | Notifications | 14 | 2 |
| 3 (rotation) | Profile settings | 11 | 4 |

Total 110 h vs 84 h.
- P1: 12 + 9 = 21 h. P1+P2: 21 + 16 + 18 + 14 = 69 h. Adding P3 → 99 h, over budget.
- Run P1+P2 (69 h). Leftover 15 h → the top cases of both P3 suites (e.g. 6 h from Sorting, 9 h from Analytics export), not one whole suite.
- Profile settings (P4) moves to the next release's rotation; the report says so.
- Wrong answers to avoid: "everyone stays 2 extra hours a day" (Savin shows it as the startup habit and rejects it for a humane company), or silently dropping groups 2–3.

### 5.4 The «проблема противоречия» and its four remedies
Regression time is fixed; the suite pool grows every release because new-feature suites join it. Savin's remedies, in his order of preference:
1. **Prioritise suites and cases.** Run only what really matters. Many companies prefer exhausting people to prioritising; don't.
2. **Optimise suites.** Retire obsolete cases, merge duplicates, simplify execution with tools written since, and revisit *how* old suites test.
3. **Automate regression**, with the cautions in section 6.
4. **Hire more testers**: the simplest and most short-sighted option. It only postpones the problem, since you can't hire forever. Savin prefers a small group of well-paid professionals who solve it with 1–3.

Overtime is not a remedy. If the user's plan relies on it, say so and offer the priority cut-off instead.

---

## 6. Automation: Savin's cautions only

Savin is a sceptic about regression automation done badly. Pass on his cautions briefly:
- **Cost per bug found.** Over the same period, compare (automation build + maintenance + tooling) ÷ bugs found by the scripts against manual regression cost ÷ bugs found manually. Also compare build-plus-maintenance time with manual run time. He claims automation usually loses; present this as a check against vanity automation. It ignores feedback speed and run frequency in CI, so it is not the only measure.
- **Automate only stable, "adult" features.** A feature is like a person: a child changes radically between 6 and 12, an adult barely between 42 and 48. Automating UI regression for a feature that just shipped is the most senseless idea. (Unit/API tests written alongside development are a different matter.)
- **Distrust "N suites / M cases automated" metrics.** They push the automator to skip infrastructure and produce scripts that die at the next UI change. His story: a newly hired automator promises to automate everything; the demo dazzles; next release the scripts break on small UI changes, fixing takes the whole regression window, and he is eventually replaced by a manual tester.
- **More bugs are found while automating** (by studying the feature closely) than by running the scripts later.
- **100% automation of regression is a myth.** Find a real professional, let them build maintainable infrastructure without pressure for counts. Savin says a good black-box tester beats one or more bad automators.

For **whether, what and how** to automate (ROI with real numbers, pyramid level, frameworks, CI), **defer to the `test-automation-coach` skill if it is installed**. Otherwise apply ROI thinking: creation + maintenance + run cost of automated vs manual over the expected number of runs; maintenance cost dominates for UI tests; prefer stable features and lower pyramid levels; count the value of fast feedback too.

---

## 7. Measuring testing

**The goal of testing is to find bugs before users do**, so effectiveness is measured by **escaped bugs** (found after release), not by bugs found before release. Users don't care how many bugs you found, the same way a shoe buyer doesn't care what the shop's renovation cost. Pre-release counts are popular because managers want a simple number to report upward.

Illustration (fresh numbers): release A: 40 bugs found before release, 3 escaped. Release B: 160 found, 15 escaped. Testing of A was more effective, despite B's impressive count. Pre-release counts mostly reflect the object under test (code quality, complexity, testability), so they aren't comparable across features or people.

**Post-release matrix**, built after every release: rows = priority (P1–P4), columns = functional area; cells = escaped bugs. Optionally slice by tester, dev, spec.

| | Onboarding | Search | Messaging | Billing |
|---|---|---|---|---|
| P1 | 0 | 1 | 0 | 5 |
| P2 | 1 | 2 | 1 | 3 |
| P3 | 2 | 4 | 3 | 1 |

Compare releases. One bad release is noise; the same area leading for several releases is a **pattern** worth investigating. Turning these statistics into process diagnosis is QA work, not just testing.

**Humane root-cause analysis.** The first instinct is to blame the tester of the hot area. Savin's case: an area kept leaking P1/P2s; the real causes were a PM without domain background or documentation, and a personal conflict between a strong dev and a strong tester, both at odds with that PM. Look at spec quality, domain knowledge, team relations and process before people's diligence, and investigate with tact and leadership, not a hunt for the guilty. Testers receive software already flawed; quality is the result of everyone's work and of the process.

**Why bug counts are a bad KPI**
- **For testers**: rewards volume over importance (splitting one bug into five, filing trivia, avoiding hard areas), turns a bug record into a currency (a record is someone's opinion of a problem, not proof of one), and isn't comparable across features of different quality. It also says nothing about what users hit.
- **For programmers**: the number of bugs depends on objective factors (deadlines, bad specs, complexity) as much as on skill, and counting them makes devs fight bug filing, which ruins the rule that every found bug gets filed. Savin's alternative: judge devs by **how fast they fix**, not how many bugs they made.

**Alternatives a lead can propose**
- Escaped bugs per release by priority, especially P1/P2, trended over releases; or Defect Detection Percentage = pre-release ÷ (pre-release + escaped) for the same scope.
- The post-release matrix and the patterns it shows, each followed by a root-cause action item.
- Time from escape to detection and to fix (MTTR) for production bugs; fix turnaround for devs, by priority.
- Regression health: share of priority flows covered; share of group-3 suites run within the rotation period.
- Reopen rate and "Can't reproduce"/"Not a bug" rate as **team signals of report and fix quality**, reviewed together, never as a personal score.
- For individual testers: qualitative review (test-case reviews, quality of reports, escaped bugs in their area discussed with context), not a numeric target. Remind the user that any number made a target gets gamed.
