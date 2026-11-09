# Fundamentals: bug, goal, mindset, cycles, testing types, terminology

Use as background for every mode and when the user asks "what is…", "why do we…", or uses Savin's vocabulary. Section 11 is the **terminology map**: the other reference files point to it. Recognise Savin's terms, answer in common modern terms, and give the mapping when the user writes in Savin's terms.

Savin's context: commercial web products where risk is tolerable, not life-critical software. Nothing in his processes is "set in stone"; formats and names vary between companies, and a startup needs less formality than a corporation. The only thing that counts is the result: the end user's happiness. Testers are paid above all for honesty, because they are the people trusted to say "go" for a release.

---

## 1. What a bug is

- **Definition:** a deviation of the actual result («ФР») from the expected result («ОР»). Everything else is detail.
- **«Три условия жизни бага».** A bug exists only when all three hold at once:
  1. the actual result is known;
  2. the expected result is known;
  3. it is known that they differ.
- Consequence: getting the actual result is the easy part (make the system act and watch). **The hard part is the expected result.** No known expected result means no bug can be claimed yet; the tester's job is to establish one first.
- **Two meanings of «баг»** (tell them apart by context):
  - the **deviation** in the product (what really happens);
  - the **record in the tracker**, a card that holds *someone's subjective opinion* that there is a problem. A record can exist with no real bug behind it (e.g. the reporter misread the spec), which is why "not a bug" resolutions exist. Bug *counts* are counts of records, not facts about the product.
- **Bug classes worth naming:**
  - functional bug: code deviates from the spec;
  - **spec bug** («баг в спецификации»): the spec itself is wrong, contradictory or silent where it must not be (e.g. the error text is not specified, so any "Error" is formally correct). Filing spec bugs against the author (the PM) is «не право, а обязанность» of the tester, because spec bugs spread into code and test cases;
  - logical bug: the code compiles but gives the wrong output; syntax bugs are caught by compilers, linters and type checkers before testers see them;
  - "rationalisation bug": the product does what the spec says, but common sense says users will suffer (one photo upload at a time for someone with 200 photos). Report it as an improvement / feature request.
- Training drill Savin recommends: mentally stamp "bug" on every expected/actual mismatch in daily life until noticing them becomes a reflex.

## 2. Sources of the expected result (in the book's order)

1–4. **The spec**, listed four times on purpose: it is the tester's main friend, and a shield («прикрыть спину»). Today: user stories with acceptance criteria, PRDs, design mockups, API contracts.
5. Then, when the spec is silent or absent:
   - **life experience**: testing software is the same checking we do every day;
   - **common sense**: valid even when a spec exists;
   - **communication**: ask the PM, developer or colleagues;
   - **established standards**: e.g. a confirmation email after sign-up is expected even if the spec omits it, and the tester may ask for the spec to be amended;
   - **statistics**: e.g. users' patience with page load (Savin's 5 s figure is dated; use current web-performance data such as Core Web Vitals);
   - **authoritative opinion**: e.g. the manager's;
   - other.
- Rule: don't fill spec gaps silently with your own assumptions. Raise the gap in writing and get the decision recorded in the ticket or spec.
- When Claude helps judge "is this a bug?", first ask: *what is the expected result, and which source does it come from?*

## 3. The goal of testing

- **«Цель тестирования — нахождение багов до того, как их найдут пользователи.»** The "before users" clause is the whole point.
- Myth 1: the goal is 100% verification. Impossible: every input has valid and invalid paths, loops multiply the expected results, and real systems would need several lifetimes. Escaped bugs are an unavoidable evil; keeping them few is the job.
- Myth 2: **the number of bugs found before release measures testing effectiveness.** Wrong. Users don't care how many bugs you found, just as a shoe-shop customer doesn't care what the renovation cost. Managers like the number because it is easy to report upward.
- **The measure is escaped bugs** (post-release bugs, defect escape rate). The release where users found fewer bugs had better testing, even if fewer bugs were found before it. Pre-release counts across features are not comparable anyway: each feature differs in complexity, code quality and testability.
- **Post-release analysis** (this is already QA work, not testing):
  1. after each release, classify escaped bugs by priority, functional area, and so on (a priority × area matrix);
  2. compare releases to find a stable pattern (one area keeps producing P1s);
  3. find the root cause humanely: weak spec or missing domain knowledge, conflicts between people, a PM without background. Don't default to blaming the tester.
- **Testing vs QA.** QA = care for quality through *prevention*, by improving the process. Testing = care for quality by *detecting* bugs before users do. Savin's analogy: raise the son well from day one (QA) vs lock the grown-up troublemaker away with tutors and hope to fix him in three years (testing).
  - A QA/process engineer without strong management backing achieves nothing.
  - Quality is the result of everyone's actions and of how well the process is tuned. Testers receive code that is already "crooked"; they cannot own quality alone.
- **Cost of a bug** («стоимость бага»), two parts:
  - **internal**: what the company spends to find and fix the bug before users see it. It can be estimated: the hours spent writing the wrong part of the spec + coding it + testing it + fixing it and its consequences;
  - **external**: losses once users meet it (support time, compensation, legal risk, users who leave and the people they warn off). Usually unmeasurable.
  - The earlier, the cheaper: a bug caught at the idea stage costs least, a post-release bug costs most. QA and testing serve the company's *self-preservation* as well as user happiness. Use this decomposition when the user needs an argument for management.

## 4. The tester's mindset: «нигилистический настрой»

- Different people look at a forest and see boards, a landscape or dissertation material. For the tester, **code is a refuge for bugs** («убежище багов»). Postulate: software has bugs.
- **Testing is a SEARCH for bugs.** «Тестировщик не занимается поиском доказательств того, что ПО работает.» The tester is not the Soviet quality-control stamp "checked, no bugs"; testers certify nothing.
- The stance is destructive thinking toward the **code, never toward people**: suspicion, distrust, no virtue assumed in advance. A good tester is a cynic, a nihilist and a doubting Thomas, all about the code. «Был бы код, а баги найдутся.»
- No contradiction with running positive tests first: running positive first is a *technical* ordering, destructiveness is a *mental* setting. Even while writing a positive test, look for how it could break.
- Tuned this way, intuition switches on and test ideas swarm as soon as you read the spec.
- A failed test is the desired outcome: a bug was found.
- Balance for Claude: today testing is also described as providing information about risk. Keep Savin's motivating edge for test design, but report results as risk information, not as a scorecard against developers.

## 5. Grey box is the norm: check the real output

- Black box: the tester doesn't know the internals; ideas come from expected user behaviour. Plus: no author bias (the developer believes in their own code the way a parent believes in their child; the author doesn't see their own bugs). Minus: possible redundant tests.
- White box: ideas come from knowledge of the internals; mostly unit tests by developers. Analogy: off-road test drive (black) vs opening the bonnet, seeing a part with a known snow weakness, and going to thrash the car on a snowy field (white). Together they increase coverage.
- **Grey box: user-oriented scenarios plus active use of knowledge of the back end. Savin says most professional web testers work this way.**
  - A success page is not a successful operation. The output to check is the page **and** the real effect: DB record, sent email, queued message, file produced, API state.
  - Choose deliberately which output *actually proves the code works*. If the page alone is the expected result, the DB check was wasted effort; if the DB effect is what matters, the page alone proves nothing.
  - Test-only helpers are legitimate: debug endpoints, stub payment providers, test pages showing internal state, data-seeding tools.
  - Knowledge sources: architecture/design docs, talking to the developer, the code itself.
- Typical multi-check test: "outside and inside", i.e. the value in the UI plus the value in the back end.

## 6. The testing cycle (process-agnostic)

Vacuum-cleaner analogy: read the manual section on wet cleaning → make a quick mental plan → do it. Three stages:

1. **Study and analysis of the subject** («изучение и анализ»).
   - Exit criterion: answers to two questions: *which functionalities must be tested?* and *how do they work?*
   - A functionality (feature) = a means of solving some user task. Functional testing is the axis of most testing work.
   - Three knowledge sources:
     - documentation: specs, mockups, flowcharts, partner/integration docs;
     - people: ask the PM in plain words; a friendly atmosphere prevents misinterpretation, which is ideal breeding ground for bugs;
     - the product itself, via **«эксплоринг»**: learning how the product works by using it as a user. Used when the code exists and there are no docs (typical for the first tester joining a running company) and for onboarding. The oracle here is life and work experience. Note that this is a *learning* activity, narrower than exploratory testing in Bach/Kaner's sense.
   - «Eat your own dog food»: use your product and competitors' products as a real customer.
2. **Planning** («планирование»): answers *how will we test?*
   - The most creative stage; product quality depends on the wisdom of the decisions.
   - Wisdom = short, simple, elegant checks + a compromise between what is testable in theory and in practice.
   - Output: test documentation (core: test cases) plus helper tools the tester writes.
   - Study and planning interleave, so together they are «подготовка к тестированию» (test preparation): writing, changing **and deleting** test cases.
3. **Execution** («исполнение»):
   - **New functionality first**, with the new test cases; they are refined during their first run («обкатка»). **Then regression** with the old cases.
   - Every bug goes into the tracker. After the fix: (a) confirm it is fixed by repeating the input that produced the wrong output; (b) check that nothing nearby broke. Savin calls (a)+(b) together "regressing the bug".
   - Before testing or verifying a fix: is it the right environment, the right version, and the right build that actually contains the fix? Is the environment mid-deploy? (Savin's story: two internal sites looked identical, but one ran the old release.) Today, check the deployed commit/build on the environment.
- **Abstract the testing cycle from the development cycle.** In practice there may be no spec and the code may already be written. Knowing the testing cycle as an independent process lets you plug it into any process: waterfall, Scrum or continuous delivery.

## 7. The development cycle, modernised

Savin's waterfall stages still describe the activities each feature goes through; today they repeat per story or sprint and overlap.

| Stage | Savin's essentials | Modern form |
|---|---|---|
| Idea | Comes from marketing, support, new contracts; "it would be nice to have" (MRD) | Opportunity / epic / PRD in the tracker or wiki |
| Design and spec | Idea = the GOAL, design = the PATH. Spec says WHAT and WHY, never HOW in code. Statuses draft → pending approval → approved. **Approval ≠ the spec is perfect; the spec stays the PM's responsibility.** «Неутвержденный спек не кодируется.» No silent edits; changes are re-approved by the same people and announced to everyone. Testers demand «БМП» (flowcharts, mockups, examples) | Story with acceptance criteria, Figma flows, Specification by Example; Definition of Ready; change history + @-notifications on the ticket |
| Coding | Internal code design before code; code review; coding standards; realistic estimates (cut scope rather than quality); unit tests are the developers' job; continuous integration beats big-bang integration. **Meanwhile the tester prepares tests**, then runs a test-case review with PM + developer | PR review, linters, CI, preview environments; "three amigos"-style review |
| Test and fix | Smoke («тест приемки») of the build → new-feature testing → regression → final acceptance («тест сдачи») → go-ahead | Same order, gated in CI/CD on a release branch or with feature flags |
| Release | Major/minor/patch; keep a known-good version to return to (Savin's "smiling baby photo" kept for the mother-in-law); maintenance notice to users; no releases in peak season except serious fixes; beta to representative users | SemVer, release branches, rollback plans, zero-downtime deploys, change freezes (e.g. Black Friday), canary / staged rollout / feature flags |

- Everyone works on a different version at once: while v1 is coded and the tester prepares for it, the PM designs v2 and marketing dreams up v3.
- Spec approval, test-case review and code inspection are not policing; they improve quality, cover everyone's back and make good people better.
- Every act of planning (spec, design, test case, emergency procedure) is a chance to look into the future and prevent problems.
- Web testers are privileged: a production bug can be fixed in half an hour, whereas a car suspension bug means a recall.

## 8. Hotfix rules and the «НРБ» procedure

**Triage of a production bug:**
- non-critical → fix in the main branch («ствол», trunk), ship with the next planned release;
- critical (users can't complete a core flow) → patch release ASAP under the emergency procedure.
- Urgent non-bug changes (e.g. a legal or court requirement) are logged in the tracker as a feature request and go through the same procedure. They are also patch releases. A *minor* release, by contrast, is planned.

**«Процедура о неотложном ремонте багов» (НРБ)**, Emergency Bug Fix Procedure; modern name: hotfix runbook. Write it down in advance. It defines:
1. which priorities qualify (e.g. P1 only);
2. who may start it;
3. the sequence:
   1. the developer fixes it on the release/hotfix branch;
   2. a colleague reviews the code;
   3. a build is made for regression testing (today: the CI pipeline);
   4. the tester tests the fix and the affected area;
   5. the patch goes to production;
4. communication: whoever owns a step posts when they **start** it (with an ETA) and when they **finish** it, handing over explicitly to the next owner. Savin did this with reply-all in one email thread; today use one incident channel or thread.

**After a release:** Savin's «SWAT-команды» (PM + developer + release engineer + tester on shifts for the first days, ready to come in and stay until the patch is out) = today's on-call rotation and hypercare period.

**Branch states** as a control: open (anyone commits) → conditionally closed after code freeze (only fixes that carry a real bug ID) → closed (emergency procedure only). Today: protected branches, a ticket key required in commits, required reviewers.

## 9. Iron rule: every patched bug gets a P1 test

- **For every bug that required a patch release, write a priority-1 test case and add it to that feature's regression suite.**
- Why: (1) the bug already proved it matters to users; (2) a classic slip is fixing it on the release/hotfix branch and forgetting to merge it into the main branch, so the bug comes back in the next release. Only a regression test catches that reliably.
- Modern form: a regression test per incident, linked to the incident or bug; check forward-port/cherry-pick to main. Whether the test is automated is a separate question; see test-automation-coach.

## 10. Classification of testing types (short)

Savin's 8 axes; one test belongs to several classes at once. Claude knows the textbook definitions; the bullets below mark what Savin adds.

1. **Knowledge of internals:** black / white / **grey box (the norm)**. See §5.
2. **Object:** functional, UI, localisation, performance/load/stress, security, usability, compatibility.
   - «Тестирование интерфейса» (checking the UI itself, e.g. the max length of a field) ≠ «тестирование *с помощью* интерфейса» (using the UI as a tool to drive the back end).
   - Performance: functionally correct code can still bring down the site (one clumsy DB query). Look for the bottleneck in the whole system.
   - Compatibility: build the browser/device matrix from real usage statistics. Don't write browser versions into ordinary test cases; it costs maintenance.
3. **Subject:** alpha testers (anyone inside the company) / beta testers (outside users, who must represent the target audience).
4. **Time:** before release = «альфа-тестирование», i.e. smoke («тест приемки») → new features → regression → final acceptance («тест сдачи»); after release = beta. A "pseudo-beta" (a full release with a Beta badge meaning "your own risk") is not a real beta.
5. **Positivity:** positive / negative.
   - **Negative = a potential user error AND/OR a potential system failure**: an upstream job writes a malformed file name, a dependency is down, a queue is empty. If the spec doesn't say how the system should react, the tester must foresee it and write the test (and raise the spec gap).
   - Negative tests usually find more bugs and need more creativity, because specs describe the happy path.
   - In a suite, run positive tests first: the positive scenario is the feature's purpose.
   - A wrong or misleading error message is a serious bug: the user abandons the flow.
6. **Isolation:** component / integration / system (end-to-end).
   - Test a component in isolation with hand-crafted inputs instead of waiting for a broken upstream component.
   - Typical integration bugs are invisible to component tests: a wrong delimiter between producer and consumer, a field truncated by the consumer, data encrypted on one side and compared raw on the other.
   - Put one simple end-to-end case at the start of a feature's suite as a mini smoke test.
7. **Automation:** manual / automated / semi-automated. Savin's emphasis is on helper tools (create a test account in one click, generate test data and files) and semi-automation (tool for setup, manual for the check). For anything beyond that, point to test-automation-coach.
8. **Preparation:** documented (test cases) / ad hoc («эд хок», «интуитивное»). Ad hoc is used for an informal smoke check, for "peace of mind" on top of documented testing, by a newcomer with no test cases, and for all-hands testing before a release. It often finds big bugs. Modern structured version: session-based exploratory testing.

**Coverage, two kinds:** coverage of possible scenarios (abstract, can't be a percentage; it grows only through *unique* test cases) vs coverage of test-case execution (concrete: executed / total). Don't let the second masquerade as the first.

## 11. Terminology map (Savin → modern / ISTQB)

**Reference for all other files.** Recognise the left column, answer with the middle one, and add the mapping when the user writes in Savin's terms.

| Savin's term | Modern / ISTQB term | Note |
|---|---|---|
| «тест приемки» (smoke, sanity, confidence test) | smoke test, build verification test | **Trap:** in ISTQB-Russian «приемочное тестирование» = acceptance testing. Savin means the quick check of a new build's main functions |
| «тест сдачи» (acceptance / certification test) | release acceptance, final regression / release certification | The last days before the go-ahead. Today often automated release gates |
| «регрессивное тестирование» (of a bug) | confirmation testing (re-testing) + regression testing | Savin's "regress the bug" = check the fix itself AND its side effects |
| «регрессивное тестирование» (of a release) | regression testing | Runs after new-feature testing |
| «тестирование новых функциональностей» | new feature testing | Runs first |
| «альфа-тестирование» | all internal pre-release testing | **Trap:** ISTQB alpha = acceptance testing by users or an independent team at the developer's site. Savin's covers unit, smoke, new-feature, regression and acceptance |
| «бета-тестирование», «бета-релиз» | beta, canary, staged rollout, early access | Beta users must be representative |
| «псевдобета» | "Beta" label on a full release | Savin: not a real beta |
| «эксплоринг» | product exploration for learning | ≠ exploratory testing; it is a way to learn the product |
| «эд хок», «интуитивное тестирование» | ad hoc testing; structured form = exploratory testing (SBTM) | Savin doesn't separate the two |
| «нигилистический настрой» | tester mindset, "testing shows the presence of bugs" | Myers-style "testing is trying to find errors" |
| «серый ящик» | grey-box testing | Savin: the default for web testers |
| «тестирование с помощью интерфейса» | UI-driven back-end testing | vs UI testing proper |
| «функциональность» | feature / functionality | "A means of solving a user task" |
| «флоу» | flow, user journey | One or more scenarios of one usage path |
| «сценарий» | test steps + test data | Savin: actions + concrete values |
| ОР / ФР | expected / actual result | |
| «ИДЕЯ» тест-кейса | test objective / purpose | A separate plain-language field |
| «тест-комплект» | test suite | |
| «обкатка» | first run / dry run of new test cases | Done by the author, who refines the cases |
| «рассмотрение тест-кейсов» | test case review with PM + developer | ≈ three amigos; agreement recorded in writing |
| «продюсер» | product manager / product owner (BA) | Owns the spec and spec bugs |
| «спек», PRD, MRD | requirements, PRD, user story + acceptance criteria | MRD → epic/opportunity brief |
| «БМП» (блок-схемы, макеты, примеры) | flowcharts, mockups/prototypes (Figma), examples (SbE/BDD) | Testers should demand them |
| «заморозка спеков» | requirements baseline, Definition of Ready, change control | No silent edits; re-approval + announcement |
| «заморозка кода» | code freeze, release branch cut | Today often replaced by trunk-based development + feature flags |
| «СТБ» (система трэкинга багов) | bug tracker (Jira, YouTrack, Linear) | Process first, tool second |
| «баг» as a record | issue / ticket | vs the real deviation |
| Feature Request («запрос об улучшении») | improvement / enhancement | Also used for urgent non-bug changes |
| П1–П4 | priority P1–P4 (Jira: Highest…Low) | Separate from severity; see `bugs.md` |
| «НРБ» (процедура о неотложном ремонте багов) | hotfix / emergency fix runbook | See §8 |
| «заплаточный релиз» | patch / hotfix release | |
| «основной» / «дополнительный» релиз | major / minor release | Savin's digit-count numbering (12.46) → SemVer |
| «SWAT-команда» | on-call rotation, incident response, hypercare | |
| «бранч», «ствол» | branch, trunk / main | CVS → Git |
| «билд» | build | "A version of a version": check the build ID before testing |
| «плейграунд» программиста | local / dev environment, preview environment | |
| «www.main…» / «www.old…» | staging (next release) / production mirror | Savin wanted logical environment names and a visible version |
| «ментальный настрой» + «инструментарий» | mindset + test design techniques | What separates a tester from others who can invent tests |
| «черновик–чистовик», «матричная раскладка», «блок-схемы» | brainstorm → filtered list; classification tree / input partition table; flowchart / state-flow testing | Savin's *generation* methods |
| «оценка риска», «эквивалентные классы», «пограничные значения» | risk-based prioritisation, equivalence partitioning, boundary value analysis | Savin's *selection* methods (ISTQB calls EP/BVA design techniques) |
| «покрытие сценариев» / «покрытие исполнения» | test (requirements) coverage / test execution progress | |

## 12. Cultural stances

- **Don't rate programmers by the number of bugs they produced.** Time pressure and bad specs matter as much as skill. Smart programmers aren't offended by bug reports, because their work is judged by how fast they fix bugs, not by how many they made. Equally, don't rate testers by pre-release bug counts (§3).
- **«Если к тебе обратились — помоги».** Don't answer "let's do it tomorrow" twice: the developer will code their own guess. Ask freely, even 200 times.
- **A written trail is business practice, not snitching** («это не стукачество, а деловая практика»):
  1. after an oral answer, confirm your understanding in writing ("Did I understand correctly that…?");
  2. no reply → send it again;
  3. still none → escalate to your manager, who goes to the PM's manager.
  - Keep the trail where the work lives (ticket comments, the spec page, a thread linked from the ticket), not only in private chats. After a meeting, send a summary; silence counts as consent.
  - Purpose: protect everyone's back when deadlines slip and people start blaming each other, and keep decisions findable.
- All bugs go into the tracker, with no exceptions, and are never deleted; the tracker keeps the product's full history.
- Everyone owns their area: the spec is the PM's, and the tester raises the alarm instead of guessing.
- Leadership's attitude to users leaks into the product. Jokes about "stupid users", even in the smoking room, end up in error messages and support replies, to the competitors' delight.
- Practice before theory: testing rests on logic, common sense and life experience. Methods are tools: «не методы должны управлять вами».
