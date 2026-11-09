# Requirements review: hunting bugs in the spec

Use when the user shares a spec, PRD, user story, acceptance criteria or a Figma flow and asks for a review, test ideas or "what's wrong here". Also use when a bug's root cause is "the spec didn't say".

Savin's stance: **the spec is the tester's first and main source of the expected result**, so a bug in the spec spreads «как зараза» into the code and the test cases. The earlier a bug is caught, the cheaper it is, and a bug caught at the spec stage is the cheapest of all. Reporting spec bugs is «не право, а обязанность» of the tester.

Terminology: Savin's «спек» / «спецификация» = today's PRD, user story with acceptance criteria, API contract or design file. Savin's «продюсер» = product manager / product owner (PO). Recognise his words, but answer in modern terms.

---

## 1. The seven «болезни спеков»

Savin says a good spec is like a good law. Any deviation from these seven qualities is a spec bug. For each finding, give the user a pair: **error in the spec → what the end user will experience**. A finding without a user-facing consequence is weak and easy for the PO to dismiss.

| # | Disease (Savin's name) | What to check |
|---|---|---|
| 1 | «Акцент на деталях и их четкое определение» (vague details) | Formats, limits, units, defaults, error texts, timeouts, what "valid" means |
| 2 | «Неверное толкование» (open to misreading) | Words with two readings; the reader is 100% sure of the wrong one |
| 3 | «Противоречивость» (internal or cross-spec contradiction) | Clause vs clause, text vs mockup, this story vs another team's spec |
| 4 | «Логическая взаимосвязь компонентов» (no logical structure) | Steps out of order, references to undefined screens or terms, gaps between items |
| 5 | «Полнота охвата предмета» (incomplete scope) | Missing flows: errors, recovery, cancel, roles, states, security-critical steps |
| 6 | «Соответствие нормативным актам» (legal and regulatory) | Personal data, consent, age limits, licences, accessibility, tax and financial rules |
| 7 | «Соответствие деловой практике» (business practice) | Promises the real world can't keep: timings, guarantees, industry conventions |

Examples (the book's originals are summarised in one line; use the fresh ones with users):

1. **Details.** Book: e-mail validation checks only for "." before the domain, so double "@" and illegal characters pass. Fresh: "the phone field must be valid" with no format or country codes → dev accepts only 10-digit Russian numbers → users with Kazakh or Armenian numbers can't get the SMS code and leave for a competitor.
2. **Misreading.** Book analogy: a song lyric everyone mishears and is sure they heard right. Fresh: "show orders for the last month". Is that the calendar month or the last 30 days? → the dashboard and the accounting export show different totals → support tickets and a loss of trust in the numbers.
3. **Contradiction.** Book: one clause says delivery only to the card's billing address, another says any address, two devs implement both, and the order is accepted but never shipped. Fresh: section 3 says the trial converts to paid automatically, section 5 says the user must confirm the card first → backend and frontend each implement one rule → users are charged without warning, or silently lose access on day 8.
4. **Logic.** Book: Sharik's letter from «Простоквашино», where the items jump around incoherently. Fresh: the onboarding story sends the welcome e-mail before the e-mail is confirmed and refers to a "Preferences screen" that isn't defined anywhere → the dev invents the screen → the user gets a welcome mail for an account they can't log in to yet.
5. **Completeness.** Book: the card-binding spec forgot CVV2, which led to fraud. Fresh: the 2FA spec has no "lost phone" / recovery-code path → locked-out users flood support and some never come back. Or card binding without 3-D Secure → chargebacks.
6. **Regulatory.** Book: an online pharmacy selling prescription drugs, and music file sharing (Napster) → lawsuits. Fresh: a sign-up form that stores Russian citizens' personal data outside Russia (152-ФЗ) or sets marketing consent pre-ticked (GDPR); an alcohol delivery flow without age verification → fines, app store removal.
7. **Business practice.** Book: promising an exact transfer time when bank transfers take 3–6 business days → the user plans a big purchase on money that hasn't arrived. Fresh: "refund within 1 hour" when acquirers return card money in 3–10 business days → angry users and chargebacks, and the support script contradicts the UI.

**Bonus check (Savin's polemic): the spec dictates the implementation.** A spec should say WHAT and WHY, not table names or function signatures («каждый должен заниматься своим делом»). Flag this as a finding when it blocks a better design or hides the actual requirement.

---

## 2. Mining a single requirement

Savin's signature move: take any one-line requirement and pull 3–5 bugs out of it before any code exists. Run these checks on every clause that contains a number, range, list or field:

1. **Boundaries: inclusive or exclusive?** Does "from 1 to 12" include 12? Does "up to 500 characters" allow exactly 500? Demand the wording «включительно» or explicit ≤ / <.
2. **Invalid input undefined.** What happens on a value below or above the range, the wrong type, special characters, empty input, spaces only, a value pasted with leading spaces? If the spec doesn't say, the dev decides, and they might allow it silently or crash.
3. **Is an empty value valid?** Savin: null input can be valid or invalid depending on the spec. Ask which.
4. **Error behaviour.** Is the error text specified? Savin's example: if not, a dev can show a bare «Ошибка» and still be "right".
5. **Text vs mockup.** Does the mockup agree with the text? Check required-field markers, field order, labels, limits shown in hints, button states.
6. **Expected result for every input class.** If you can't write the expected result, the spec has a gap.

Fresh worked example: "A review comment must be 10 to 500 characters long."
- Are 10 and 500 allowed? Are emojis counted as 1 or 2 characters? Are leading and trailing spaces counted?
- What happens with 9 characters? Is the button disabled, is there an inline error (with what text?), or does the server reject it?
- What happens on a paste of 5,000 characters: truncated or rejected?
- The mockup shows a counter "0/1000". That contradicts the text, so it's a spec bug.

## 3. Equivalence partitioning as a spec check

Savin uses partitioning first to **review the spec**, and only after that to pick tests. Draw the classes from the spec and look for three defects:

| Defect | Symptom | Question to the PO |
|---|---|---|
| **Overlap** | A boundary value fits two classes ("500–1000" and "1000–5000") | Which class does exactly 1000 belong to? |
| **Gap** | Part of the input range has no class (nothing below the first tier, nothing for negative or zero values) | What happens below 200? Is that "no bonus" or an error? |
| **Ambiguous base quantity** | It's unclear *what* is measured | Is it the item total, or the total with delivery? Before or after refunds? |

Book example (one line): a bookstore's "spend more, save more" table had overlapping edges, no tier below 200, and an unclear base ("spent" on books only, or including delivery). The fix: unfreeze the spec, rewrite it as five non-overlapping classes with explicit kopecks, and notify the dev.

Fresh example: cashback by monthly card spend: 0–10 000 → 1%, 10 000–50 000 → 2%, 50 000+ → 3%.
- Overlap: exactly 10 000 and 50 000.
- Base quantity: do transfers, cash withdrawals and utility payments count? Are refunds subtracted? Which time zone defines "month"?
- Gap: what happens with a negative monthly balance after refunds?

Every class is made of two parts: the input range plus the expected-output rule. If the spec gives only one of them, that's a finding. Boundary-value test design itself is covered in `test-design.md`.

## 4. The spec bug playbook

When the spec is silent or wrong, Savin's rule is **never assume silently** («лучше поднять тревогу», rather than guess). Whoever guesses owns the consequences.

| Situation | Action |
|---|---|
| A detail is missing, and the dev will probably do something sensible | Ask the PO to add it to the spec clause anyway, in writing. Otherwise the next dev, or the next release, may do something different |
| The implementation does something against common sense or a company standard, even though the spec allows it | File a bug. Spec silence doesn't make it correct |
| The spec contradicts itself (text vs text, text vs mockup) | File a bug against the spec, assigned to the PO. The PO fixes the text or the mockup |
| The spec is correct but the behaviour is clumsy for real users (e.g. upload one photo at a time when users have 200) | File an improvement request. Savin's joke: a "rationalisation bug" that is politely called a Feature Request |
| An established convention is missing (confirmation e-mail after sign-up, "forgot password" link) | You may demand the spec is amended. Conventions are a valid source of the expected result |

Rules:
- **Spec bugs go into the tracker** (a Jira issue of type Bug or Spec issue, linked to the story, assigned to the PO). A chat remark is not enough, because it's not traceable.
- The fallback sources of the expected result when the spec is silent, in Savin's order: life experience, common sense, communication, established standards, statistics, authoritative opinion. Name which one you are using in the finding.
- Failure mode to warn about: the tester misses both the spec gap *and* the dev's bad guess. That's why gaps are logged at review time, not at testing time.

## 5. «БМП»: mockups, flowcharts, examples

Savin's coinage «БМП» = «блок-схемы, макеты, примеры». **Testers must insist that specs are illustrated with all three.** The reason: text alone gets misread, and a complex thing needs several views to be understood, like an engineering drawing with front, top and side views.

- **Mockups** (today: Figma screens and prototypes) show WHAT the user sees. Savin's idea of printing the whole flow and hanging it on the wall in user order works today as a Figma flow board.
- **Flowcharts** show the logic: input → processing → output. Savin's maxim: the interface is WHAT the user sees, the algorithm is WHY. Backend-only features have no mockups, so a flowchart or sequence diagram is the only picture. Either the PO or the tester may draw it. If the tester draws it, attach it to the story or the test suite.
- **Examples**: concrete inputs with outputs (today: Specification by Example, Gherkin scenarios, example tables). Savin: if you wrote a rule, invent an example, and you will understand it better yourself.

Why БМП find bugs:
1. Several views give one shared interpretation.
2. Making them forces the author to rethink the text, which surfaces gaps.
3. They are visual, so contradictions can literally be seen.

**A contradiction between text and БМП is a spec bug**, because БМП are part of the spec. Savin's planted example: the text says all fields are mandatory, but the mockup has no asterisk on «Фамилия». File it against the PO, who fixes either the text or the mockup. Modern variants: Figma shows a field the story doesn't mention; the API example in the story uses a field name that differs from the contract; a Gherkin example contradicts a rule in the description.

In a review, if a complex story has no БМП, make that a finding of its own ("needs a flow diagram / examples table") instead of trying to guess the logic.

## 6. Change control

Savin's spec lifecycle: Draft → Approval pending (a review meeting or async review with dev and QA) → Approved. Modern equivalent: a Confluence/Notion page or Jira story with a status, named approvers and page history.

Principles to apply and to defend for the user:
- **Approval ≠ perfection.** Approving means the spec was found fit for further work after an initial read. It does not mean it's bug-free.
- **Responsibility stays with the author.** Even after dev and QA approved it, gaps found later are the PO's. This protects the tester from "but you approved it".
- **No silent edits.** Savin's story: the PO quietly edits the approved spec three times after approval. Each team member works from a different revision, and the release is a disaster. Today the same thing happens with a living wiki page or Figma file that people opened on different days. Every change after approval must be visible.
- **Late changes breed bugs.** An urgent change to a feature that is coded and half-forgotten pulls three people off current work, they rush to recall the old context, and both the old and the new project suffer. Management should weigh each change against that cost.
- **«Неутвержденный спек не кодируется, и точка.»** Modern form: a **Definition of Ready / ready-for-development gate**. A story doesn't enter the sprint until it has acceptance criteria, БМП where needed, and sign-off from PO, dev and QA.

Change procedure after approval (Savin's freeze, translated):
1. The same people who approved the previous version approve the change.
2. The change is announced to everyone involved (PO, devs, QA, release/DevOps): a changelog entry on the page plus @-mentions in the ticket or channel, listing what changed and who approved it.
3. The spec version is traceable: page history, a "v2" label, or the spec in Git with required reviewers.
4. QA updates the affected test cases and links them to the change.

## 7. The question escalation ladder

Answering spec questions is the PO's «святая обязанность». The tester has not just the right but the duty to ask ALL questions, because it's the tester who gets blamed for a missed bug. Savin: no written trail, no proof («нет е-мейла — нет доказательств»).

1. **Ask everything**, even the 200th question. Group the questions per spec (Savin kept a mail folder per spec ID; today, comments on the ticket or a thread linked from it).
2. **Confirm oral answers in writing.** After a call or chat at the desk, post "Just confirming I understood correctly: clause 4.2 means … Right?" in the ticket.
3. **No reply → resend** the same message (quote or re-ping the original), so it's visible it was already ignored once.
4. **Still nothing → escalate via your manager.** Forward the thread with "I'm blocked on this spec". Your manager goes to the PO's manager. Savin: that's not snitching, it's business practice («это не стукачество, а деловая практика»).
5. **Record the decision in the spec itself**, not just in the thread, so the answer reaches the dev and the next reader.

Also teach the reverse culture rule, which applies to the tester too: «Если к тебе обратились — помоги». A PO who answers "let's do it tomorrow" twice gets code built on a guess.

## 8. Three-party test case review

«Рассмотрение тест-кейсов»: a few days before testing starts, the PO who wrote the spec, the dev who implemented it and the tester meet. The tester walks through how they will test, case by case.

- **Gains:** the PO and dev add test ideas and catch misunderstandings in the tester's reading of the spec. It's a second spec review from a different angle.
- **The «политический момент»:** if participants suggested nothing, or what they suggested was added, they have co-approved the test scope. You can't test everything, so if a bug slips through, everyone knows the agreed scope was covered.
- **Written summary afterwards:** send all attendees the agreed changes, as a comment on the ticket or a short page. It serves as your to-do list and as a check that you understood correctly. Savin: no reply means agreement. Today, say so explicitly in the message, e.g. "If there are no objections by Thursday, I'll proceed with this scope".
- Modern format: 30 minutes, async-friendly. Share the test cases from the test management system or the checklist in advance and focus the meeting on disagreements.

## 9. Output format for a spec review

When the user asks to review a spec, output exactly this structure, so they can paste it into the ticket or Confluence. Write it in the user's language. Skip disease sections with no findings, but list them in the summary line as "no issues found".

```
## Spec review: <name / ticket key>, <version or date>

Summary: <N> findings (<blockers> blockers). Checked: details, misreading,
contradictions, logic, completeness, regulatory, business practice, БМП.
Recommendation: <ready for development | ready after blockers are fixed | needs rework>

### 1. Vague details
- [B] <clause ref> — <what's missing>.
  Consequence: <what the user experiences if the dev guesses wrong>.
  Proposal: <concrete wording, e.g. "10–500 characters inclusive; emoji counts as 1">
### 2. Open to misreading
### 3. Contradictions (incl. text vs mockup)
### 4. Logic and structure
### 5. Missing scope
### 6. Regulatory
### 7. Business practice
### БМП
- <which flows need a mockup / flowchart / examples table>

### Questions to the PO
1. <closed question, answerable in one line, with clause ref>
2. ...

### Proposed acceptance criteria / examples (optional)
| Input | Expected result |
```

Rules for the output:
- Severity: **[B]** blocker (the dev can't implement correctly without an answer), **[M]** major (likely user-facing bug), **[m]** minor (wording or clarity).
- Every finding has a clause reference, a consequence and a concrete proposal. "Unclear" alone is not a finding.
- Questions are closed and specific ("Is 500 included?"), not "please clarify the limits".
- Don't invent answers. Where you had to assume something to continue, mark it **Assumption:** and add it to the questions.
- End by reminding the user: findings → tracker issues assigned to the PO; answers → written into the spec; a re-review after changes.
