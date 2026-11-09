# Test design: generating, selecting and writing test cases

Use when the user asks to design tests for a feature or requirement, to write or review test cases, to organise suites, or to explain equivalence classes / boundary values. Savin splits the work into **form** (how a test case and a suite look) and **content** (which tests to write). Content comes first in practice, so this file starts there.

Mindset to carry through: code is «убежище багов» and testing is the *search* for bugs, not proof that it works. A FAIL is the outcome we hope for. Even a positive test case should be designed to catch a bug.

---

## 1. The 3 + 3 toolbox: generate first, then select

| Stage | Method (Savin's term) | What it does |
|---|---|---|
| Generate | «Черновик-чистовик» (dirty list → white list) | Uncritical brainstorm, then a critical cleanup |
| Generate | «Матричная раскладка» (matrix layout) | Break inputs into elements, then combine them into scenarios |
| Generate | «Блок-схемы» (flowcharts) | Draw the process; the drawing exposes branches and questions |
| Select | «Оценка риска» (risk estimate) | Decide what to test first and most deeply, from data |
| Select | «Эквивалентные классы» (equivalence classes) | Collapse inputs that hit the same logic |
| Select | «Пограничные значения» (boundary values) | Pick the few values per class that catch coding errors |

- Note for Claude: ISTQB calls EP/BVA "design techniques". Savin treats them as **selection filters**, applied during or after generation to cut an infinite input space. Don't jump straight to EP/BVA; generate broadly first.
- Generation methods are mainly **thinking tools for analysing the spec**. Drawing a matrix or flowchart raises questions a plain reading misses.
- Combine freely: a dirty list whose white list becomes Table 1; a dirty/white list to choose which Table 2 combinations to keep; a flowchart to visualise a table's process. «В тестировании нет догм»: the methods serve you, not the other way round.
- Pick the level of abstraction by what you test. If you test registration itself, go detailed. If registration is just one step of a purchase flow, three boxes are enough.

### 1.1 «Черновик-чистовик» procedure
1. **Dirty list.** While or after reading the spec or exploring the product, dump *every* testing-related thought without judging it: test ideas, whole scenarios, single steps or data values, expected results, **questions for the PM**. Include ideas that look absurd. Think of it as a one-person brainstorm.
2. **White list.** Analyse the list, get answers to the questions, and move over only the items that deserve to live. Decide using the spec, the PM, the developer, intuition and the selection methods. While moving items, refine them and group them (positive/negative, functional area).
3. **Iterate.** The first white list usually becomes the next dirty list. Repeat until it stops changing.
4. Write the test cases from the final white list.
- Savin's exercise is the cola vending machine: 15 min dirty list, 15 min white list, 30 min full test cases. It is a good workshop format for juniors.
- Claude's default failure: producing a polished list at once. In coaching mode, show the divergent phase explicitly, including the questions for the PM it surfaced.

### 1.2 «Матричная раскладка» procedure
1. **Table 1, «набросок элементов».** List the elements of a field or page in subgroups (one condition per subgroup), with positive and negative elements together. Give each element an ID: `<Area>_el_NNN`. Savin's postal-code field (6-digit Russian index):

| Subgroup | Element | ID |
|---|---|---|
| Entered? | yes / no | Index_el_001 / 002 |
| Exists (a real address has it)? | yes / no | 003 / 004 |
| Format | 6 digits / 5 / 7 / letters / special char / space | 005 … 010 |

2. **Table 2, «комбинация элементов».** Combine elements into scenarios with IDs `<Area>_com_NNN`. **Positive first**, then negative. For the index field: 001 is a valid existing index. The negatives are a non-existent 6-digit value, 5 digits, 7 digits, a Cyrillic «о» instead of zero, a bracket instead of a digit, an embedded space, and empty. Some rows follow directly from one element (empty = el_002); that's fine.
3. **Attach expected results.** Here: every negative → the page reloads with a specific error text and the label is highlighted. Positive → the next page.
4. **Compose upward.** Each page or stage gets its own Table 1/Table 2 pair. Table 2 scenarios then become *elements* of Table 3 (a multi-page flow), and so on. Black-box web testing is manipulating interdependent pages toward an expected result, so the hierarchy is natural. One Table 1 may also span several linked pages.
- IDs are worth it even when Table 2 shows content: they let cells refer to each other and make discussion precise ("el_008 isn't handled").
- Often Table 1 alone is enough, if the scenarios are obvious from it. The matrix exists for testing, not testing for the matrix.
- Modern note: Table 1 ≈ classification tree; Table 2 ≈ combinations. If the combinations explode, suggest pairwise. Savin doesn't use it, but it fits his "no dogmas" rule.

### 1.3 «Блок-схемы» procedure
Four basic blocks: start/end, process step, decision (usually two branches), and connector (continues on another chart). Drawing rules:
1. Name the main process.
2. Sketch the main path first.
3. Give each block a short, informative name.
4. Add references to sources (spec section, ticket).
5. Lay it out top-to-bottom, left-to-right.
6. Avoid crossing arrows.
7. Verify the finished chart against the spec.
- Every decision diamond is at least two test ideas plus a question: "what if neither?" Mermaid works well today.

### 1.4 Spec questions: the written trail
- Answering spec questions is the PM's duty (Savin's «продюсер»). Asking *all* of them is the tester's duty, because the tester answers for escaped bugs.
- Confirm oral answers in writing ("Did I understand correctly that…?"). Today, do it in the Jira ticket, the Confluence spec or a Slack thread, and record decisions in the ticket or spec. If a question is ignored, ping in the same thread; if it is still ignored, raise it with your lead. Stay polite, but don't let it go.

---

## 2. Selection methods

### 2.1 «Оценка риска» from data, not from «абсолютно очевидно»
- Hotel parable: a new hotel owner has one snowplow and three roads. He makes a confident "obvious" plan. The experienced manager uses what he knows about the guests (who comes from where, when, and why) and reaches a different plan, or simply hires a second plow. Lesson: smug intuition wastes scarce resources. Good decisions need information plus the skill to use it. Sometimes the right answer is "get more resources".
- Procedure:
  1. Get data: usage stats, transaction shares, analytics, support tickets, incident history.
  2. Get the opinion of the people who own the subject: PM, business analyst, finance, support.
  3. Estimate the risk for each feature or flow, and write it into the test case's priority.
- Savin's worked case: four buyer/seller flows («флоу»). The obvious ranking turned out *exactly inverted* once the BA pulled two years of deal data and the PM explained business priorities. The matrix also revealed that one "flow" was really two directions.
- Use: higher priority means tested **first** and **more thoroughly**. If two items have equal priority, start with the more complex, labour-intensive one.
- Bonus: the data is your defence. When asked "why did you test this and not that?", you can show the numbers.
- Savin's "risk" ≈ business usage × importance, drawn from data and owners. If the user asks for likelihood × impact, map onto it, but keep the insistence on real data.

### 2.2 «Эквивалентные классы»
- A class = one or more input values to which the software applies the same logic, **plus the expected output rule** for those values. Any value in a class stands for all of them.
- **Partition first as a spec review.** Before designing tests, check the partition for:
  - **overlaps:** does a boundary value fall into two classes ("200–500" and "500–1000")?
  - **gaps:** is there a missing class, e.g. below the first tier?
  - **an ambiguous base:** what exactly is measured (with or without tax/delivery; inclusive or exclusive)?
  Any hit is a spec bug. Go to the PM, tell the developer, «размораживаем» the spec, fix it, and only then design tests.
- Write classes with explicit inclusive limits at the data's precision (`200.00–499.99`), not `200–500`.

### 2.3 «Пограничные значения»: Savin's 5-point rule
For each class, test in this order:

| # | Value | Sign | Coding mistake it catches |
|---|---|---|---|
| а | lower limit of this class (if any) | + | wrong comparison (`>` instead of `>=`) or a wrong lower-limit value |
| б | upper limit of this class (if any) | + | wrong comparison or a wrong upper-limit value (e.g. `499.00` instead of `499.99`) |
| в | any value inside the class | + | reversed `<` / `>` signs |
| г | upper limit of the **preceding** class (if any) | − | (1) lower limit typed too low, e.g. `100.00` instead of `200.00`, which а/б/в miss; (2) `OR` instead of `AND`, which makes every value match |
| д | lower limit of the **following** class (if any) | − | (1) upper limit typed too high, e.g. `599.99` instead of `499.99`, which а–г miss; (2) `AND`→`OR` again |

- а, б, в are **positive**; г, д are **negative**. Their expected result is "the rule of *this* class does **not** apply" (e.g. "discount ≠ 2%").
- In the book's example (class 200.00–499.99), the three positive values stand in for about 30,000 possible kopeck values.
- **Neighbour de-duplication.** When you test all classes of one rule together, a class's г/д points are the neighbours' а/б points. So you need about 3 values per class, not 5. Savin's five-tier table needs 14 tests in total (the last, open-ended class needs only its lower limit and an inside value).
- A class with a single value (exactly 6 digits): а = б = в = 6, г = 5, д = 7. That gives one positive and two negatives.
- Difference from ISTQB 3-value BVA: Savin doesn't test limit±1-step inside the class. If the user follows ISTQB, mention that their set is a superset. Savin's rule plus the fault model explains *why* each point exists, which is the coaching value.
- Remember the invalid classes too (negative, non-numeric, empty). The book's example omits them; a full design includes them.

### 2.4 Positive first; negatives include system failures
- Run **positive tests first**: the purpose of a feature is its positive scenario. Running positives first is the *technical* order; the destructive attitude is the *mental* one. They don't conflict.
- Negative scenario = a potential **user error and/or system failure**. Examples are an upstream job writing a malformed file name, a dependency timing out, or the mail service being down. If the spec doesn't say what happens then, the tester must foresee it, write the test, and ask the PM. Negative tests usually find more bugs because specs describe the happy path.
- A wrong or misleading **error message** is a serious bug. Savin's story: a site kept saying "check the card number" when the real problem was an unticked consent checkbox, and it lost the customer.

---

## 3. Savin's test case structure

Minimum content: idea + scenario (steps + data) + **expected result (ER)**. The ER is the one part a test case cannot do without. Outcomes: PASS (actual = expected), FAIL (a bug), BLOCKED (not all steps can be passed; file the blocker and postpone).

| Attribute | Rule |
|---|---|
| **TC ID** | Unique across the whole QA department, not just the suite. Assigned once and **never changes**, even if the case moves or is edited (like a tax number). Savin's convention: 4 letters + 4 digits (`PWRS0001`), with the letters chosen by area. Today a TMS generates IDs; keep them stable. |
| **Priority** | 1–4. 1 = vital (the "Buy" button); 4 = cosmetic (a link's font colour). Set by the author from how critical the checked thing is, ideally backed by the risk estimate. Under time pressure, run higher priorities first. |
| **IDEA** | A plain-language statement of what is checked. It is separate from the title and the ER, and it **explains** the check. An ER like "10" means nothing without an IDEA that decodes it. |
| **SETUP and ADDITIONAL INFO** | Like the ingredient list before a recipe: accounts, test data, queries, hints for the executor. |
| **Revision history** | Who, what, why, when. In a TMS or Git, this is the version history plus a change note. |
| **Procedure / ER** | Numbered steps. Each ER sits **next to the step where it is checked**, not only at the end. |

Ready template:

```markdown
**TC ID:** PWRS0003  **Priority:** 1
**IDEA:** <what we check, in plain words; why the ER proves it>
**SETUP and ADDITIONAL INFO:**
- Account: <how to get one; data in a table if data-driven>
- Data: <values; highlight what differs from sibling cases>
- Helpers: <SQL / API call / mailbox access>
**Revision history:**
- Created <date> by <name>
- Modified <date> by <name>: <what and why>

| # | Procedure | Expected result |
|---|---|---|
| 1 | <step> | |
| 2 | <step where a check happens> | <precise ER for this step> |
| 3 | <step> | <ER 2, may be a formula, e.g. "value from step 1 − amount from step 2"> |
```

### 3.1 Data-driven cases and the four maintainability rules
- **Data-driven:** keep data in SETUP (or a data table), separate from the steps. A new variant is then a copy with only the data and the ID changed. The alternative is one table of rows (ID, priority, data…, ER) plus one generic procedure "for each row"; this is a parameterised test.
- Split the steps into logical modules (log in, find, act, verify), then apply:
  1. **Make it data-driven.**
  2. **Don't spell out obviously known flows** that aren't under test ("log in as a user from SETUP"). Login is tested elsewhere; here we only use it.
  3. **Drop specifics that don't matter to the idea** ("open any product", not a named product that may get deleted). Incidental details cause blocks and maintenance work.
  4. **Extract repeated flows** into a shared description (Savin used an intranet QA knowledge base; today use TMS shared steps or a Confluence page) and reference it in one step.
- **Golden mean («золотая середина»).** Too vague is «пойди туда, не знаю куда» (go there, I don't know where): only the author can run it. Too detailed is click-by-click «для учёной обезьяны» (for a trained monkey): it breaks on every UI change. Aim for steps a colleague new to the feature can follow without asking.

### 3.2 How many ERs per case
- **One idea per case.** Several ERs are legitimate when the idea is true only if *all* of them are true. Savin's example: a card payment counts as successful only if the DB status is right *and* the balance dropped by the amount.
- Splitting into separate cases is cleaner in theory, so do it when it's cheap. In practice, the time saved on writing, running and maintaining cases buys tests for other things. Savin calls multi-ER cases normal practice.
- Typical pattern: check the **outside and the inside**, i.e. the UI value plus the DB/API/queue/e-mail value. A success page is not proof of success.
- Claude's default is the purist "one assertion per test". Follow Savin's rule instead, unless the user's team has its own.

### 3.3 Problem test case checklist (use when reviewing cases)
- [ ] **Independent:** no "see steps 1–2 of TC X"; no reliance on data or traces left by another case. Test: can the cases run in any order? Repeating setup is annoying but cheaper. Referencing a *shared scenario* is fine; referencing *another test case* is not.
- [ ] **Unambiguous steps:** no undeciphered abbreviations, no "obvious" shortcuts that will be unclear in two months.
- [ ] **IDEA and ER self-contained:** no "check clause 21.6 of the spec" in the IDEA or ER. (External refs are acceptable in steps, not in IDEA/ER.)
- [ ] **IDEA explains:** not empty and not a copy of the ER.
- [ ] **Precise ER:** "an error page is shown" lets the wrong error pass; "all works" is not an ER. Name the exact text, value or state.
- [ ] **Executable by another person:** Savin's maxim is that a case only its author can run deserves public burning. Write for the colleague who inherits it when you're sick, on holiday or gone.

---

## 4. Suites («тест-комплекты»)

- A suite = test cases for one functional area and/or one spec (epic). Modern equivalent: a TMS section or folder, linked to the Jira epic.
- **Header:** suite name and ID; author; spec/epic link; priority 1–4 (usually the spec's); PM (Savin's «продюсер»); developer; OVERVIEW (what it covers); GLOBAL SETUP (items reused by more than one case). The header gives traceability and shows who to ask. Highlight the data that differs between sibling cases.
- **Lifecycle:**
  1. A new spec arrives → create a **new suite** for its cases, separate from regression («мухи отдельно, котлеты отдельно»). New-feature cases always run first.
  2. Write the cases **without IDs**.
  3. **«Обкатка»:** the author runs them first. Cases written before the code exists are raw, because specs have bugs and change quietly, so editing them now is expected and nobody is to blame. During the run: add missing cases, fix misunderstandings, delete duplicates, improve maintainability, polish the wording until it is crystal clear.
  4. Assign IDs to the survivors.
  5. If it makes sense, merge the suite into the regression suite for that area **as an unchanged part**: no renumbering and no merged header.
- **States:** New (first revision) → Modified (a spec change or a maintainability fix) → Retired («Более недействителен»: the feature is dead, or the case duplicates another).
- **Archive, never delete.** Your judgement may be wrong, and old cases come back. Savin used a `retired_testcases` folder; today use the TMS archived/deprecated status (Git history for cases as code). Usually whole suites retire together.
- When a case changes heavily, rewriting it is often cheaper than restoring it (Savin: easier to build on an empty lot than restore an old mansion). Practical rule: give the rewrite a new ID and retire the old case.
- No format is sacred: adapt it to the company. These are the practices Savin saw work, not a standard.

---

## 5. Mini-example: password-reset link

Spec (as given): "Forgot password" sends an e-mail with a link; the link is valid 24 h and single-use; the new password must be 8–64 characters.

1. **Dirty list (excerpt):** the link works once; a reused link; an expired link; two requests (does the first link die?); an unregistered e-mail (what message? does it reveal that the account exists?); the mail service down; e-mail typed in upper case; the link opened in another browser or device; a password of 7/8/64/65 characters; a password equal to the old one; existing sessions after the reset (logged out?). **Questions for the PM:** is exactly 24:00:00 valid? Does a new request invalidate the old link? Are sessions on other devices ended?
2. **White list:** grouped into Positive (reset works), Link validity, Password rules, Failures and messages. "Another browser" is merged into the positive case's data.
3. **Matrix, Table 1:** `Reset_el_001/002` link used before yes/no; `003/004` a newer link was requested yes/no; `005–007` age: fresh / just under the limit / at or after the limit. **Table 2:** `Reset_com_001` is fresh + unused + latest (positive); the negatives each break one element.
4. **Risk:** pull the daily reset volume and the "can't log in" support tickets, and ask support and the PM. Suppose the data shows most resets come from mobile mail apps → the link opened on mobile becomes priority 1.
5. **EP + BVA on length:** classes 0–7 (reject), 8–64 (accept), 65+ (reject). The joint set with de-duplication is 0 (empty), 4, 7 | 8, 30, 64 | 65, 200. Spec review question: are spaces or Unicode counted as characters?
6. **One case:**

**TC ID:** PWRS0001  **Priority:** 1
**IDEA:** A fresh reset link sets the new password; the old password stops working.
**SETUP:** a registered user with access to the mailbox; new password `Valid#Pass8` (8 chars, the lower limit).

| # | Procedure | Expected result |
|---|---|---|
| 1 | Request a reset for the SETUP user's e-mail | The message on screen is the same one shown for unregistered e-mails |
| 2 | Open the latest reset e-mail, follow the link | The new-password form opens |
| 3 | Enter the new password twice, submit | Confirmation shown; DB/API: password hash changed, token marked used |
| 4 | Log in with the old password, then with the new one | Old: rejected with the standard error; new: logged in |

---

## 6. Hand-offs
- Automating these cases (whether, which ones, how) → **test-automation-coach**.
