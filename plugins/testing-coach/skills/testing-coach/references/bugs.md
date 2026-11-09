# Bugs: report, attributes, severity/priority, lifecycle, verification

Use when the user writes, reviews or argues about a bug report, sets severity or priority, gets a bug bounced back (Not a bug, Can't reproduce...), verifies a fix, or handles a production bug.

Savin's frame: the bug tracker («СТБ», система трэкинга багов) is **both a bug repository and a communication tool** for everyone in the process. And **the process defines the tool's settings, not the other way round** (paper cards would do if the process is right).

**Two meanings of «баг»:** (1) a deviation of actual from expected; (2) a record in the tracker, which is "a problem in someone's opinion". A record can exist without a real defect (the filer misread the spec). That is why resolutions like Not a bug exist, and why counting records is not counting product defects.

**Iron rules (Savin):**
1. **Every bug found gets filed. No exceptions.** A filed bug is the tester's real output. Good developers aren't offended: they are judged by how fast they fix, not by how many bugs they made.
2. **Nothing is ever deleted** from the tracker: not bugs, not user accounts (deactivate only). Turn off delete permissions in Jira.
3. Bugs are found in **specs too**, and spec bugs go into the tracker against the PM, like code bugs.

---

## 1. The bug report

### 1.1 Summary «Краткое описание»
- People triage from a **list of summaries**, so the summary must carry the essence alone.
- **Budget: ~100 characters**, spaces included (Savin's Bugzilla limit; Jira allows 255, keep the discipline anyway).
- Pattern: **what is wrong → where → under what condition**. E.g. "Profile: address saved truncated to 7 chars when longer than 7".
- **Spec/feature prefix:** `<spec or feature ID>: <summary>`, e.g. "1211: wrong status written for orders from mobile app". Sorting by summary clusters a feature's bugs; anyone can search by the ID. Modern equivalent: epic link, label or component; keep the prefix when linking is weak or bugs are viewed in flat lists.
- **Never «Ничего не работает» / "nothing works"** (Savin: people used to get hit with a candelabrum for that). Same for "error on page", "bug in search", "doesn't work properly".
- Savin: how a tester words the summary shows their professionalism.

### 1.2 Description and steps «Описание и шаги для воспроизведения»
Savin's shape, in this order:
1. **Description / useful info:** context, nuances, why this priority (see §3.4).
2. **Test data block, all up front:** account login/password, item names, IDs, input values, environment. If the oracle is in the DB or logs, define a **named check** once: `SQL1: select address1 from user_info where login = '<login>'`. Steps then just say "Run SQL1".
3. **Steps to reproduce:** numbered, **one atomic action per step**, imperative («Открой», «Введи», «Нажми»). Name UI elements precisely; if a button's name is unknown, write "submit the form". Include steps that capture data you'll need later ("write down the order number").
4. **Bug:** actual result. **Expected:** expected result (with source: spec section, AC, previous behavior).

Notes:
- The oracle can sit below the UI (grey-box: DB column, API response, log line). Savin's own example ends in a DB value, not a screen.
- **Write for the PM, not for a tester.** A test case may use QA shorthand, abbreviations and links, because colleagues understand them. A bug report is read by developers, testers **and the PM/product owner** (Savin's «продюсер»), so anything obvious only to QA must be spelled out. Aim: someone from another department reproduces it without asking you.

### 1.3 Attachments «Приложение»
- Visuals beat text: screenshot with the problem area circled, screen recording, HAR file, console/server logs, request/response, test data file.
- When a problem is hard to put in words (especially in a foreign language), attach and say so.
- **Always write "See attachment" in the description**, or nobody opens it.

### 1.4 Template
```
Summary: <FEATURE-ID>: <what is wrong> in <where> when <condition>   (≤ ~100 chars)

Description:
<context; anything non-obvious to a non-tester; why this severity/priority>
See attachment: <file names>

Test data:
- Environment: <stage URL>, build/version <x.y.z / commit / image tag>
- Account: <login> / <password>
- Data: <IDs, input values>
- SQL1: <query that shows the result>   (optional named checks)

Steps to reproduce:
1. Open <URL>.
2. Log in as <login>.
3. ...
N. Run SQL1.

Bug: <actual>
Expected: <expected> (per <spec §/AC/previous behavior>)
```

### 1.5 Checklist for reviewing someone's report
- [ ] Summary ≤ ~100 chars, states what/where/condition, has the feature prefix or link; not "doesn't work".
- [ ] All test data and environment + build are given before the steps.
- [ ] Steps are numbered, atomic, imperative; a newcomer or PM can follow them.
- [ ] Ends with explicit **Bug** and **Expected**, and the expected result has a source.
- [ ] It was reproduced a second time before filing (ask!).
- [ ] Attachments exist where words are weak, and are mentioned in the text.
- [ ] Severity matches the category table (§3.2), not a feeling.
- [ ] Priority is justified in the text if it isn't obvious; Found on / version / component / assignee are set.
- [ ] One bug per report; not a duplicate (searched by feature ID and keywords).

---

## 2. Useful attributes (Savin's Bugzilla fields → Jira)

| Savin's field | Jira today | Rule worth keeping |
|---|---|---|
| Bug Number | Issue key | People refer to bugs by key in conversation. |
| Assigned to «держатель бага» | Assignee | **Owner = whoever does the next step**, not "the person who fixes". Every open bug always has exactly one. Don't know whom? Assign to yourself. |
| — | CODEOWNERS / service catalog / component lead | Savin's «Кто за что ответствен» page: component → developer(s) → PM. Without it, filing turns into guessing. |
| Verifier | custom "QA assignee" field | Defaults to the reporter; becomes owner once the fix is deployed. Non-testers who file should name a verifier. |
| Component | Component | The area you were testing when you found it. |
| Found on | custom field / environment | Values: test envs, **Production**, **Spec**, Other. "Spec" makes spec bugs first-class; "Production" tracks escaped defects. |
| Version/Build Found | Affects Version + build/commit in env field | Every build must identify itself (/version endpoint, footer, About, header). |
| Version/Build Fixed | Fix Version + linked PR/commit/pipeline | The developer sets it. Savin's "N/A" is valid for a spec fix made by the PM. Savin's "next build number" rule → today the commit SHA or image tag that contains the fix. |
| Comments | Comments | **Make a comment mandatory on some transitions**, e.g. when returning a bug as Not a bug. |
| Severity, Priority | custom Severity field + Priority | See §3. Both mandatory at filing. |
| Notify list «Список для оповещения» | Watchers / Slack integration | **Always add the PM (spec owner)**, so they see the testing state of their feature. |
| Change history «История изменений» | History tab | Savin calls it the most important attribute: every change is public and signed. "Anonymity is excluded." |
| Type | Issue type | Bug vs Feature request (Story/Improvement). A feature request is also the gate for an **urgent non-bug production change** (see §8). |
| Status + Resolution | Status + Resolution | See §4. |

Minimum fields at filing (Savin's checklist): summary, description & steps, attachment if needed, assignee, component, found on, version/build found, severity, priority, watchers (at least the PM), type, resolution/status = Assigned.

---

## 3. Severity vs priority

### 3.1 The core distinction
| | Severity «серьёзность» | Priority «приоритет» |
|---|---|---|
| Nature | **Technical**: impact on the software | **Business**: importance to the company |
| Scale | **Absolute**: follows from the category | **Relative**: depends on context, release, users |
| How assigned | Mechanically, by the table | Creatively, by assessing the threat to the business |
| Meaning | What kind of failure it is | **A deadline for the fix** |
| Negotiable? | **Never** (a crash is C1, full stop) | Yes, legitimately, with arguments |

### 3.2 Severity C1–C4 (Savin's categories)
| Level | Categories | Examples |
|---|---|---|
| **C1 Critical** | crash; **data loss**; **security hole** | 500 error on search in production; email not stored at registration; editing an address deletes the old one with no new one saved; password visible in the URL |
| **C2 Major** | hang; **blocker** (blocks coding, testing or use, **with no workaround**) | page loads forever after "Buy"; user can't add any payment method so can't buy |
| **C3 Minor** | functional bug not in C1/C2: actual ≠ expected **but the flow can still be completed** | wrong value in a field, wrong filter result |
| **C4 Cosmetic** | content, spelling, UI | typo, misaligned button, wrong font |

- Note Savin's order: data loss and security sit at C1 **with** crashes, above hangs and blockers. Many textbooks put "blocker" on top; map if the team's scale differs.
- **"Blocker" means no workaround.** If testing can continue another way, it is not a blocker and not C2. Example: an internal tool for creating test accounts fails, but accounts can be created by hand → not C2.
- **A bug in a test tool is a fully legitimate bug.** File it.

### 3.3 Priority P1–P4 as deadlines
Every company needs written priority definitions, and **the mandatory part is the fix deadline**. Savin's sample:

| Level | Deadline |
|---|---|
| **P1** | Drop everything and fix now |
| **P2** | Within 72 hours |
| **P3** | Before testing of this release ends |
| **P4** | If there's time |

- Factors that raise priority: share of affected users, money loss, legal risk, reputation damage.
- **Showstopper «пробка»**: a P1 that can derail a planned release. It can be a test-infrastructure bug, e.g. the simulator of an external system is broken, so a whole area can't be tested.
- Release rule: normally all P1–P3 are fixed and closed before release. A release with a P3 may be approved with a commitment to patch within N days. Savin is candid that priorities sometimes get lowered for business/political reasons; that is management's call, and it touches priority only.
- The tool can hard-link top values ("C1 ⇒ P1"); for the bulk (C3) there is no fixed link.

### 3.4 How to set priority
- **«Казнить нельзя помиловать»:** the missing comma is C4 (cosmetic), but if it can get the wrong person punished, it is P1. Low severity, top priority.
- **Ask "what happens in production?"** Savin's worked example: an ordinary functional bug, a wrong code written to a DB column, is C3. The tester asked the developer what that value drives. The answer: in production the user is charged but the order never goes through, on the most popular path. So it became **P1**. Investigate consequences (ask the developer, the PM, look at traffic) before setting priority.
- **Justify the priority in the description** when it isn't obvious: why this one, not higher or lower.
- **In doubt between two levels, take the higher one** "for the sake of our users", and say why in the text.
- **Don't lower it yourself under pressure.** If a developer says it's inflated and you still believe it's right, ask **your manager** to decide.
- Self-interest check: if you're blocked and set P3 instead of P1 to be nice to a developer friend, the missed schedule is **your** problem.
- Maxim: priority is the strongest lever on a developer's schedule. Don't abuse it.
- **Severity never changes for political reasons.** If someone wants a crash "downgraded to Minor", offer to discuss **priority** instead.

---

## 4. Lifecycle

### 4.1 Status vs resolution
Savin's two-level model: a **minimal status** (Open, Closed, Re-Open) plus a **detailed resolution** for the stage. Analogy: status is "alive / dead / reborn"; resolution is the details of the life (entered university, got married, bought a car). In Jira, statuses are usually richer and Resolution is set on close; map as below.

| Resolution (Savin) | Set by | Next owner | Meaning |
|---|---|---|---|
| Not Assigned | reporter | reporter | reporter doesn't yet know who fixes |
| Assigned | reporter / tester | developer | owner set for the next step |
| Fix in Progress | developer | developer | working on it |
| Fixed | developer | release engineer / CI | fix merged into the right branch(es) |
| Build in Progress | release engineer / CI | release engineer / CI | build with the fix being deployed to test env |
| Verify | release engineer / CI | **verifier** | fix deployed; ready to check |
| Fix is Verified | verifier | — (close) | fix confirmed; can be closed |
| Verification Failed | verifier | developer | fix didn't work |
| Can't Reproduce | developer | reporter | see §6, §7 |
| Duplicate | developer | reporter | close after checking |
| Not a bug | developer | reporter | see §7 |
| 3rd party bug | developer | reporter → PM | see §7 |
| No longer applicable | developer | reporter | see §7 |

Rules:
- **The owner is whoever does the next step.** At every moment, exactly one.
- **Only testers close bugs** (Jira: restrict the Close transition to the QA group). Developers resolve; testers close.
- **Reopen** when: a later change broke the fix ("reintroduced", a regression); or a hotfix went to the release/production branch but **wasn't merged back to main**, so it returns in the next release.
- **Watch for ping-pong:** a bug bouncing between developers ("yours" / "no, yours") or between dev and QA. After two bounces, stop commenting and get the owner decided by a lead or the CODEOWNERS/component map.
- Conceptual flow: file → developer understands → fixes → merges → build deployed → tester verifies → close or return. Fork: developer says "not a bug" → tester re-examines own reasoning → close, or return with arguments.

---

## 5. Fix verification
Savin calls the whole thing «регрессивное тестирование бага» ("regress the bug"); in modern terms it is **confirmation testing (part 1) + regression around the fix (part 2)**. Recognise his term, answer in the modern ones.

0. **Check the build first.** Confirm the environment runs the version/commit/image that contains the fix (Fix Version, linked PR, deployed tag). Otherwise you are verifying nothing.
1. **Part 1, confirm the fix:** replay the report's steps exactly, with the same data and the same oracle (same SQL check). Works as expected → the fix is confirmed. You **may set Verified and close right after part 1.**
2. **Part 2, look for side effects:** code is a tightly woven fabric. **Ask the developer what else the fix can affect** (ideally they write it in a comment or the PR). If they didn't say, choose by situation:
   - walk the **main (priority) flow** of the repaired feature, and/or
   - run an **end-to-end path through the repaired component**. E.g. fix in search filters → search → apply filter → open item → add to wishlist.
   - In Savin's example, after fixing one card type he re-ran the test case for the other main card type.
3. **If part 1 fails, skip part 2:** set Verification Failed, assign back to the developer, and comment with the build checked, what you did and what you saw.
4. If part 2 finds something new, **file a new bug** linked to the original; don't reopen the fixed one unless it is the same defect.

---

## 6. Reproducibility

- Core belief: **"if a bug exists, it can be reproduced."**
- **Reproduce it again before filing** («Перед тем как занести новый баг, воспроизведи его ещё раз»). Anti-pattern: file in a hurry, move on; the developer drops their work, fails several times, returns Can't Reproduce. Worst with long repro procedures. Goal: developers never return your bugs as Can't Reproduce.
- **Condition vs cause:**
  - *condition* «условие появления бага»: the situation that, recreated, recreates the bug (e.g. a card with a past expiry date can be added);
  - *cause* «причина»: the developer's or PM's actual mistake (a logic error).
  - **The tester's job is the condition.** Leave the cause to the developer.
- **Once vs twice:** seen once and can't reproduce → you may let it go (note it). **Seen a second time → you are obliged to find the conditions.** They always exist.
- **The lab story:** only one chemist could make a new compound, even with colleagues copying him step by step; filming showed he carried the test tube in his coat pocket across a frosty yard while lighting a cigarette, so it never cooled. The condition was "the liquid must not cool". The tiniest nuance decides.

Techniques for hunting conditions (modern, in Savin's spirit):
- Write down exactly what you did and saw the moment it happens: time, URL, account, data, build. Keep a screen recording running during exploratory sessions.
- Diff your attempt against the original: user/role, data state (empty vs filled, old vs new records), locale/timezone, browser/device, feature flags, cache/cookies, session age, entry path (deep link vs navigation), speed (double-click, slow network), order of actions, concurrency (two tabs, two users), time (midnight, month end), environment and build.
- Use the server's memory: logs, traces, error monitoring (Sentry etc.) at the timestamp; request IDs; DB state before and after.
- Change one variable at a time; once it reproduces, cut the steps down to the minimum.
- Ask the developer what the code depends on (that narrows the variables, not a request for them to debug).
- If still intermittent, file it anyway after the second occurrence with frequency ("3 of 20 runs"), everything you ruled out, logs and timestamps.

---

## 7. Disputed resolutions: playbooks

**Not a bug** («это не баг, это фича»). Usually the tester tested from the spec and the developer coded from "something else": a vague spec (developer did the PM's job), a part of the spec ignored or skipped, or someone misread it.
1. Read the developer's comment (it must be there; make it a required field on that transition).
2. If they're right, close. If not, return it (Assigned, to the developer) with your arguments and the spec reference.
3. Talk directly if needed.
4. **No consensus → assign to the PM** with a comment asking them to decide. Not to your manager, not round 3 with the developer.
5. If the spec was at fault, the PM fixes the spec and marks it for verification. **Close only after seeing with your own eyes** that the spec changed, the change is correct, and it's published where everyone reads specs.
- Note: some companies count a tester's Not-a-bug share against them. Another reason to reproduce and read the spec before filing.

**3rd party bug.** Root cause in someone else's software (a library, runtime, browser, a partner's API).
- **Can't influence the vendor:** the only path is a workaround. **Close this bug as 3rd party and file a new Feature request for the workaround**, so the developer's work is planned and justified. Optionally report upstream and link it.
- **Can influence (a partner):** assign to the project manager with comments; they coordinate both sides. Outcome: our misunderstanding of their system → Not a bug, close; or they log it in their tracker → 3rd party, close with the link. Priority depends on where it lives (their test env vs production) and its impact.
- **Our developer may be blaming the third party wrongly.** If evidence points at our code, return it (Assigned, to the developer) with that evidence.

**No longer applicable** («поезд ушёл»). It was a bug, but requirements changed (e.g. a feature request removed the field you expected).
- Check the change is real (linked story/spec), then close.
- Usually caused by **missing information**. Savin's habit: if actual ≠ expected in **your own** test case, reproduce and file. If you notice something **outside your expected results or outside an area you know fully**, first ask the tester who owns that area.

**Duplicate.**
- Open the original: is it really the same condition and the same result? If yes, close and add anything new (data, environment) to the original. If not, return with the difference spelled out.
- Prevention: search by feature ID/keywords before filing; set saved filters/subscriptions for your areas (Savin: notification rules on keywords in the summary).

**Can't reproduce.**
1. Re-run your own steps on the **current** build and the environment you named.
2. Compare with the developer's environment, data, role, build; often the difference is the missing condition.
3. Reproduces for you → add the missing condition or data to the report, attach a recording, return it. Offer to show it live.
4. Doesn't reproduce anywhere → check whether it was fixed in passing (then close, noting the build) or whether it was never a bug (close, learn).
5. Intermittent → §6: after the second occurrence you owe the conditions.

---

## 8. Production bugs

- Even the most urgent fix is **tested on a test environment before the hotfix/patch release**.
- Triage:
  - **non-critical** (e.g. a missing validation) → fix in main, ships with the **next release**;
  - **critical** (users can't complete a key action, data loss, security) → **hotfix ASAP** through a written emergency procedure (Savin's «НРБ», процедура неотложного ремонта багов): which priorities qualify (e.g. only P1), who may start it, steps (fix → code review → build for testing → test → release), and a status message at the start and end of each step in one thread or incident channel.
- **Iron rule: every bug that got a hotfix gets a high-priority (P1) regression test case** in that feature's regression suite. It catches the fix being lost when the hotfix branch isn't merged back to main. (Whether to automate it: see test-automation-coach.)
- Set Found on = Production; the escaped-defect record feeds later analysis of where testing missed.
- An urgent **non-bug** production change (legal demand, urgent business need) goes through the tracker as a **Feature request** with P1 (hotfix) or P3 (can wait for the next release). The tracker stays the single gate for any production change.
- Savin's «SWAT-команды» after a release → today on-call rotation / hypercare: PM, developer, release engineer and tester ready to patch.
