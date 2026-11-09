# Career: first QA job, resume, interviews, mock interview

Based on Part 4 of Savin's book, «Как устроиться на первую работу» (PDF pp. 285–313). The book was written for career changers in the 2007 US internet-startup market. The tactics hold up; some of the attitude advice is harmful today and is neutralised below. Read this file for any request about getting a first (or next) testing job, resumes, cover letters, interview prep, or "rehearse an interview with me".

## Contents
1. Mindset for the first job
2. Resume and translating past experience
3. Channels and volume
4. Before the interview
5. During the interview
6. After the interview
7. Interview question bank: Savin's 22 + modern additions
8. Mock interview mode (procedure)
9. Growth after the first job

---

## 1. Mindset for the first job

**Savin's core insight, kept:** for a beginner, *motivation is the differentiator*. A company hiring a junior is not buying skills it can't get elsewhere; it is buying a person who will learn fast, care about the result and be pleasant to work with. Decision-makers feel the candidate's attitude before they parse the answers. Savin's "Stage 0" of the job search is getting into a «боевой настрой» (fighting mood).

**Savin's four commandments («Ментальный настрой»), and how to handle them:**

| Savin (2007) | Status | What Claude advises instead |
|---|---|---|
| Sincerely want to work | Keep | Show genuine interest in the product and the craft; it is visible and it sells |
| Ready for unlimited hours | Neutralise | "I'm flexible when the release needs it", meaning reasonable crunch flexibility, not a promise of endless overtime |
| Ready for weekends and holidays | Neutralise | Same: willingness to help in a real emergency, framed as team spirit, not as a default |
| Ready to work for free / any pay | **Drop** | Never advise working for free or "any pay". Advise a *realistic junior salary expectation* (researched on hh.ru/Habr Career salary data) plus openness to internships, traineeships, paid crowdtesting |

- **Never** reproduce "work unlimited hours / for free" as advice, in resumes, cover letters or interview answers. Why: unpaid work offers are illegal in many jurisdictions, they read as desperation, they attract exploitative employers, and they anchor the salary low for years.
- If the user quotes Savin's commandments, acknowledge the context (a recent immigrant with no network in a hungry startup market) and extract the principle: *show that you want this job more than the next candidate and that you'll be easy to rely on*.
- Savin's housekeeper analogy (retell if useful, 1–2 lines): an eager helper who asks only for patience with early mistakes gets hired, trained and then kept, because managers rarely meet that attitude. The modern reading: eagerness plus reliability gets a beginner past the "no experience" filter; it does not require giving labour away.
- Modern proof of motivation that beats any slogan: a small portfolio. A few well-written bug reports on public products, a checklist or test cases for a real app, a GitHub repo with a Postman collection, crowdtesting history (uTest, Test IO), a finished course project.

## 2. Resume and translating past experience

**Resume = advertising with one shot.** It must make the reader want to call; otherwise it goes to the "later" pile. Show the right facts, in the most favourable light, in the clearest form.

**Translate past non-IT experience into testing language** (Savin's signature technique):
- Savin's example: an engineer who installed and tuned typesetting machines becomes "wrote a test plan for installing and debugging typesetting equipment; found critical factory defects before operation; coordinated fixes with the manufacturer".
- Procedure Claude runs with the user:
  1. List past duties and achievements in plain words.
  2. For each, ask: where did I *check* something against an expectation, *find* a problem, *document* it, *reproduce* it, *coordinate* a fix, *prevent* a failure, *train* someone?
  3. Rewrite in testing vocabulary: verification, acceptance, defect, root cause, checklist, regression, risk, documentation, stakeholder.
  4. Keep it truthful. Reframing is allowed; inventing testing jobs is not, and it falls apart at the first follow-up question.
- Nobody is taught "tester" at school; Savin's acquaintances who became testers came from architecture, teaching, engineering, biology, meteorology, law and programming. A non-IT past is normal, not a handicap.

**Domain expertise is leverage.** Companies hire testers with domain knowledge because testing is easier to teach than the domain (Savin: medical diagnostics software prefers someone with a medical background; some were hired with no testing experience purely for domain fit). Tell the user to tailor the resume and target companies where their former field is the product's field (logistics, medicine, education, banking, construction, telecom...).

**The brag file** («маленький симпатичный текстовый файлик»):
- Keep a running file of everything good you did at any job, without false modesty, dated.
- Rename small deeds into their professional names: "showed a colleague how to use the tool" becomes «обучение персонала» (staff training).
- Use: resume bullets, interview stories (Q11, Q15), performance reviews and promotion arguments. People and managers forget your wins; the file doesn't.

**Other resume points (short):**
- A fresh course (testing, programming, SQL, project management) is good "seasoning"; list the practical outputs, not just the certificate.
- English is a real plus (docs, tools, international teams); state the level honestly.
- Tailor the resume per vacancy: one template, with emphasis adjusted to the job.
- Study other testers' resumes for structure; Savin's «черновик-чистовик» (draft, then clean copy) applies: brainstorm everything first, cut later.
- Modern additions: 1 page for juniors; a skills line matching the vacancy keywords (ATS filters); links to portfolio/GitHub; skip personal data the vacancy doesn't need.

## 3. Channels and volume

Savin's channels translated:

| Savin (2007) | Modern equivalent |
|---|---|
| Call friends and acquaintances, "cast the line" («забросить удочку») | Tell your network; **referrals** are the strongest channel today (a referral skips the resume pile) |
| Employment agencies; recruiters are "your best allies" | IT recruiters on LinkedIn and hh.ru; reply to them, ask for market feedback and resume advice. The principle holds: the recruiter earns only if you get hired |
| Passive: resume on job sites (works while you sleep) | Profiles on hh.ru, Habr Career, LinkedIn (Open to Work), Djinni/Indeed depending on market |
| Active: resume + cover letter by email and fax | Targeted applications via company career pages, LinkedIn messages to QA leads, Telegram job channels and QA communities |
| Write to companies even without a QA opening | Still works: a short message to a QA lead or the careers email with a tailored note |

- **Apply even when requirements look inflated.** Savin's joke: if the tester profession appeared on 1 March, by 2 March vacancies would demand 3 years of experience. Requirements are a wish list; everyone knows it. Apply if you match roughly half and can show motivation. The company may also have or open other QA roles. «Безнадёжность в головах, а не на рынке» is the spirit.
- **Volume plus tailoring.** Savin's benchmark: ~300 resumes and letters for several interviews and one offer. Modern funnels differ, but the lesson stands: job search is a job; track applications in a table (company, date, channel, status, follow-up).
- **No false modesty** in applications: it is business for both sides, not begging.
- Cover letter: 3–5 sentences, why *this* company/product, one or two relevant proofs, motivation. Drop Savin's "any hours, any pay" line.

## 4. Before the interview

«Интервью начинается дома» (the interview starts at home).

1. **Use the product.** Register, go through the main flow, buy the cheapest thing if it's a shop; feel the business as a user. Note **a few bugs or observations** (2–4), with steps. Use them carefully (see Q20): mention them tactfully as "things I noticed and would look into", never as a takedown. Savin says «НЕ КРИТИКУЙТЕ»; modern QA interviews often reward a well-presented finding, so the balance is *show the skill, keep the tone respectful*.
2. **Research the company:** business model, where revenue comes from, competitors, recent news, tech stack from vacancies and engineering blog. Savin's story: he learned that a company's revenue came mainly from post-auction fees, steered the conversation to auctions (his strong area), got an offer the next day.
3. **Interview the company back.** Prepare 2–3 questions about the team, process and risks. Savin's story in one line: a candidate asked a P2P music startup what their plan was if courts shut them down; there was none, he went elsewhere, and the startup was closed by lawsuits within months.
4. **Find people inside:** an acquaintance at the company can give context or a referral.
5. **Logistics:** onsite, arrive early, walk around, announce yourself on time (Savin's ritual); remote, test camera, mic and link, join 1–2 minutes early. Rest well; dress tidily for the company's culture (suits are rare in IT now). Savin's cautionary tale: a strong candidate who showed up drunk in the morning got no offer, because "if that's his best, what is his normal?"
6. **Prepare stories** from the brag file for achievement, difficulty and conflict questions (STAR: situation, task, action, result).
7. **Recruiter screen:** often a recruiter reads the hiring manager's questions and records answers without judging the technical content; answer clearly and briefly so the notes pass on well.

## 5. During the interview

**Behaviour rules (Savin's, modernised):**
- **Be brief, offer depth.** Answer the question, then signal "I can go into detail if useful". Savin's story of a suitor who tried to impress a girl's parents with "Monet or Manet painted *Sunflowers*?" (answer: Van Gogh) shows how surplus detail sinks authority.
- **Be polite and correct:** no over-familiarity, no risky jokes. Imagine the interviewer is hungry and tired; tact and friendliness buy you their patience.
- **Let the interviewer talk about what they love.** Savin asked whether they used Python; the interviewer talked 20 minutes about tools he'd built; the offer came next day. Genuine interest in their work is part of the interview.
- **Personality is judged as much as knowledge.** A reliable, curious, open beginner beats a brilliant snob; interviewers choose who they'll spend their working days with.
- **Never badmouth past employers**, even a non-IT job. Frame leaving as moving *toward* something.
- **If it goes badly, stay calm and polite.** Your own sense of how it went counts for nothing; you don't make the decision. Savin almost fled his early interview while the interviewer fetched drinks; he stayed and got hired.

**The «не знаю» script** (for factual or tool-knowledge gaps):
- Say at once: «Не знаю, но если это понадобится для работы, обязательно разберусь». Then, if possible, add what you *do* know nearby or how you'd find out ("I haven't used Charles, but I've inspected requests in DevTools; I'd start there").
- Don'ts: no fake recall (frowning, looking up, "it's on the tip of my tongue"), no bluffing, no absurd original answer to impress. Bluffing is detected and kills trust in every other answer.
- Willingness to learn is a junior's main asset: stress it each time you don't know.

**Reasoning questions are different (modern correction of Savin):** for "how would you test X", "what would you do if...", estimation or logic tasks, *think aloud*: clarify requirements, state assumptions, structure the answer. Interviewers grade the reasoning, not a memorised answer. "I don't know" fits facts; it doesn't fit thinking tasks.

**Go to every interview** («ходите на все интервью»): each one teaches you questions you couldn't answer (study them that evening), lets you exchange information with practising testers, and trains communication. Never cancel one because another "went great": Savin's friend did exactly that, the first company never called and the second had filled the role. The goal is several offers on the kitchen table, so *you* choose.

## 6. After the interview

- Usually several people interview you; each gives the manager an opinion; the manager decides.
- Send a short thank-you message (email or LinkedIn) the same day; it can add one thing you forgot.
- Write down every question you couldn't answer and close the gap.
- Savin's "Olya and George" story: a company that desperately needed a tester sent a recruiter out with a blank contract, and he hired an energetic stranger he met at a job fair. Moral: the employer's need can be as urgent as yours, and visible energy attracts offers. (Don't endorse the story's wine-and-dine method.)

## 7. Interview question bank

**Legend:** [Savin] = from the book (his order, PDF 304–307); [Modern] = added; not in the book. For each: the gist of a strong answer and what the interviewer is really checking. Give the user the gist and help them build their *own* answer from their experience; never hand over a script to memorise word for word.

### 7.1 Savin's 22 questions (modernised)

| # | Question | Gist of a strong answer | Really checking |
|---|---|---|---|
| 1 | [Savin] Why did you decide to become a tester? | A concrete trigger from your life (a time you caught a problem others missed); you enjoy analysing how things break; you have the traits: attention to detail, composure, communication | Real motivation vs. "IT pays well"; whether you know what the job is |
| 2 | [Savin] What do you like most about testing? | Savin's triad: creativity + intuition + applied skills; plus a tangible result: bugs found before users hit them | Whether you understand testing as thinking, not clicking |
| 3 | [Savin] What qualities does an effective tester need? | Honesty, genuine interest and pride in the work, continuous learning, fast task switching, communication, effectiveness under pressure; then one short proof that you have two of them. Drop Savin's "sacrifice personal time" and "unlimited hours" items | Self-awareness; values match (honest bug reporting) |
| 4 | [Savin] Short- and long-term career plans? | Short: join, learn the product and process, become reliable. Long: grow into a specialisation (automation, performance, security, QA lead). Plans support the job, not compete with it | Whether you'll stay and grow; whether the role is a stopgap |
| 5 | [Savin] In an emergency, can you come in any time? | Savin: instant "yes, 24×7×365". Modern: a calm, quick "yes, in a real incident I'll be there", optionally asking how on-call works in the team. Speed and confidence still matter | Reliability in a crisis; and you can probe the team's culture |
| 6 | [Savin] Can you work weekends/holidays in an emergency? | Same as 5: yes for real emergencies; ask how often it happens. A "constantly" answer is a red flag about the company | Same; this is also *your* signal about them |
| 7 | [Savin] Can you work more than 8 hours when needed? | Yes, around releases and incidents; you plan work so it's the exception. Don't recite "testing means unlimited hours" | Commitment without naivety |
| 8 | [Savin] Can you work on several projects at once? | Yes; how you switch: notes, task tracker, priorities agreed with the lead; an example | Organisation and context switching |
| 9 | [Savin] How do you handle time pressure? Management pressure? | Pressure is normal in product companies; you prioritise by risk, communicate early what won't be covered, stay polite; an example | Composure; risk-based thinking; whether you hide problems under pressure |
| 10 | [Savin] Relevant experience and education? | Use the translated experience (section 2): checking, finding, documenting, coordinating fixes; courses with practical output | Transferable skills; the ability to tell your story |
| 11 | [Savin] Biggest professional achievement (any field)? | One prepared STAR story from the brag file, told with pride and numbers | Drive, ownership, how you measure success |
| 12 | [Savin] Why are you leaving / did you leave? | Moving toward testing; you valued the old job and colleagues; you want to apply your experience in a new field. No complaints | Badmouthing risk; whether you're running *from* or *to* |
| 13 | [Savin] Biggest disappointments at previous jobs? | No dramatic ones; a mild example of something that didn't work and what you learned. Savin's maxim: the person makes the place, not the place the person | Blame vs. ownership |
| 14 | [Savin] Big company or startup? | Either, with a reason: big = established process and mentors, faster learning; startup = build QA from scratch. Better if it matches *this* company | Fit; whether you thought about it |
| 15 | [Savin] A hard situation and how you solved it? | Prepared STAR story; your actions, not the team's; the outcome and the lesson | Problem-solving, initiative, honesty about your role |
| 16 | [Savin] Two weaknesses, two strengths? | Two real but manageable weaknesses with what you're doing about them; two strengths with evidence. Savin's "small weakness: I like poker" joke is risky today; don't use "unlimited hours" as a strength | Self-awareness; honesty; no clichés ("perfectionist") |
| 17 | [Savin] What would you improve in yourself, and how? | A concrete skill gap (e.g. SQL, API testing, coding) and the plan already underway (course, project, hours a week) | Learning habit; initiative |
| 18 | [Savin] Alone or in a team? | Honest preference, plus you can do both; testers sit between developers, analysts and product, so collaboration matters | Team fit |
| 19 | [Savin] Why our company? | Specifics from your research and product use that genuinely attract you | Whether you did homework or mass-applied |
| 20 | [Savin] What do you know about us? Do you use our product? | Research facts plus your user impressions. Savin: «НЕ КРИТИКУЙТЕ». Modern balance: strengths first, then "I noticed a couple of things I'd check" if asked or the moment is right; facts, not judgement | Interest; tact; and quietly, your eye for issues |
| 21 | [Savin] Why this position? | What the role involves, why it matches you, and that you'll close any gaps fast | Understanding of the role; motivation |
| 22 | [Savin] Why should we hire you? | A 30-second pitch: their need → your matching proofs → motivation. Savin: "I'm not joking"; it's a real question, answer it seriously | Can you sell yourself without arrogance |

### 7.2 Modern questions juniors get today [Modern, not in the book]

| Topic | Typical questions | Gist / what to show |
|---|---|---|
| Testing basics | What is testing; verification vs. validation; test case vs. checklist; severity vs. priority; bug life cycle; test design techniques (equivalence classes, boundary values, decision tables, state transitions) | Clear definitions plus a one-line example each. (Savin's terms differ: his «тест приемки» = smoke, «тест сдачи» = acceptance; answer in common terms) |
| "Test this object" | Test a pencil, an elevator, a lift button, a login form, a vending machine | Clarify requirements and users first; then functional, boundary, negative, usability, performance/load, safety, compatibility; prioritise by risk. Think aloud |
| Client-server | What happens when you open a URL; frontend vs. backend; what an API is | Request/response, DNS, HTTP(S), server, database; where a bug may live |
| HTTP | Methods (GET/POST/PUT/PATCH/DELETE), status code classes 1xx–5xx, common codes (200, 201, 301/302, 400, 401, 403, 404, 500, 502, 503); headers, cookies, REST, JSON | Know classes and the common codes; 401 vs. 403; idempotency basics |
| API / Postman | How to test an API; build a request in Postman; positive/negative checks; auth tokens; environment variables | Status, body, schema, error messages, boundary data, auth; show a small collection in the portfolio |
| DevTools | Find why a button does nothing; check a request; console errors; emulate mobile; throttle network | Network tab (status, payload, response), Console, Elements, Application (cookies/storage); attach these to bug reports |
| SQL basics | SELECT with WHERE, ORDER BY, JOIN, GROUP BY/COUNT; find duplicates | Write simple queries live; use SQL to prepare and verify test data |
| Git basics | Branch, commit, merge, pull request; why testers need it | Read a PR diff to see what changed and plan regression |
| Agile/Scrum | Sprint, backlog, user story, acceptance criteria, Definition of Done, ceremonies; QA's role in the sprint | QA joins refinement and planning, not only the end of the sprint |
| Mobile (if relevant) | iOS vs. Android differences; interruptions; permissions; app install/update | Platform-specific risks |
| Automation (short) | Manual vs. automated; what you'd automate first | Stable, repetitive regression first; for depth, hand off to test-automation-coach |
| Behavioural | Conflict with a developer over a bug; a bug you missed; disagreement on priority | Facts, reproduction, impact; respect; learning from the miss |

## 8. Mock interview mode (procedure)

Trigger: the user asks to rehearse («прогони меня по собесу», «mock interview», «потренируй интервью»).

1. **Set up.** Ask, briefly and in one message:
   - background (previous field, years, what they did, courses, portfolio);
   - target vacancy (paste the text if possible), company/product, level, format (recruiter screen, technical, behavioural, mixed);
   - language of the interview; how many questions or minutes; whether feedback should come after each answer (default) or only at the end.
2. **Plan silently.** Pick 8–12 questions: about half from Savin's bank (motivation and behaviour), half from the modern list matching the vacancy (e.g. API + SQL for a backend-heavy vacancy). Include one "test this object" task and one gap question the user probably can't answer, to practise the «не знаю» script. Use the vacancy's stack and the company's product in questions.
3. **Play the interviewer.** Ask **one question at a time** and wait. Stay in role: realistic tone, short follow-ups ("why?", "and if the server returns 500?") like a real interviewer would, maximum one follow-up before feedback.
4. **Feedback after each answer**, compact, clearly separated from the role-play:
   - **Good:** what worked (specific).
   - **Missing:** what the interviewer was really checking and what was absent (structure, example, definition, reasoning aloud, motivation).
   - **Stronger version:** a 3–6 line rewrite *built from the user's own experience and words*, not a generic textbook answer.
   - Flag red flags immediately: badmouthing an ex-employer, bluffing, "I'll work for free / any hours", over-long answers, fake recall.
5. **Mine the user's past.** Actively turn their previous jobs into testing material: ask for one concrete episode, then show how it becomes a STAR story or a testing-language bullet (section 2). Reuse these stories across later answers.
6. **Adapt.** If an answer is weak, ask the user to retry it once with the feedback before moving on. Increase difficulty if they're doing well.
7. **Debrief** at the end:
   - overall impression as a hiring manager would note it (would you pass to the next round, and why);
   - top 3 strengths, top 3 gaps;
   - the list of knowledge gaps to study before the real interview, with what to read or practise;
   - 2–3 questions for the user to ask the company (section 4, step 3);
   - their strongest stories, summarised as bullet notes to keep (brag file).

Rules: never write the user's answers *for* them before they try; keep feedback honest but encouraging; remind them that going to real interviews is the best practice.

## 9. Growth after the first job

From Savin's stories and afterword (briefly):
- Keep the brag file from day one; use it for reviews and promotion arguments.
- Savin's suggested long-term path: become an expert, then maybe specialise (his example: regression automation). Modern options: automation, performance, security, mobile, QA lead/manager, product/analysis.
- Attitude compounds: the heavily accented immigrant whom Savin describes as hired for sheer eagerness later became a lead tester. Reliability and learning speed matter more than polish.
- Keep going to interviews occasionally, even while employed: it maps your market value and gaps.
- Savin's afterword (motivational): you choose your own path or adapt to decisions others make for you; «каждый МОЖЕТ, но не каждый БУДЕТ». He himself worked a construction site on weekdays and waited tables at weekends while studying at night and sending hundreds of resumes. Use this tone only if the user is discouraged; don't lecture.
