# AI Skills

A collection of skills for Claude and Codex. Each skill can be installed separately.

## Skills

<!-- skills:start -->
| Skill | Description | Download |
|---|---|---|
| [harvard-negotiation-coach](plugins/harvard-negotiation-coach/skills/harvard-negotiation-coach/SKILL.md) | Helps you get through a tough conversation: asking for a raise, agreeing on a price or contract, a disagreement with a colleague, a family dispute. Analyzes the situation, helps you prepare, suggests what to say when you're under pressure, and lets you rehearse. Based on the book Getting to Yes. | [zip](https://github.com/sidelnikovmike/ai-skills/raw/main/dist/harvard-negotiation-coach.zip) |
| [test-automation-coach](plugins/test-automation-coach/skills/test-automation-coach/SKILL.md) | Explains test automation in plain words, helps decide whether automating is worth it, plans automation and QA processes from scratch, and reviews or writes autotests without over-engineering. Based on the book Let's automate it! | [zip](https://github.com/sidelnikovmike/ai-skills/raw/main/dist/test-automation-coach.zip) |
| [testing-coach](plugins/testing-coach/skills/testing-coach/SKILL.md) | Coaches testers in the craft: designs tests from requirements, finds gaps in specs, writes and reviews bug reports, plans testing and regression under time pressure, and prepares newcomers for QA interviews. Based on Roman Savin's book Testing Dot Com. | [zip](https://github.com/sidelnikovmike/ai-skills/raw/main/dist/testing-coach.zip) |
<!-- skills:end -->

### Better together: testing-coach + test-automation-coach

The two testing skills complement each other:
- **testing-coach** covers *what and how to test*: requirements, test design, bug reports, regression planning, interviews.
- **test-automation-coach** covers *automation*: whether it pays off, how to build it, and how to review autotests.

If both are installed, testing-coach passes automation questions to test-automation-coach. Install both if you work on quality as a whole. Each one also works on its own.

## Installation

### Claude (web, desktop, mobile)

1. Download the skill's zip from the table above.
2. Open **Settings → Capabilities → Skills**.
3. Click **Upload skill** and select the zip file.

### Claude Code: via the plugin marketplace

Add the collection (once):

```
/plugin marketplace add sidelnikovmike/ai-skills
```

Install a skill, for example:

```
/plugin install harvard-negotiation-coach@ai-skills
```

To get newly added skills and updates: `/plugin marketplace update ai-skills`.

### Claude Code: manually

Unzip the skill's archive into `~/.claude/skills/`.

### Codex in the ChatGPT desktop app

Open this repository as a Codex project. The repository already contains links
to every skill in `.agents/skills/`. If a link is missing (for example, on a system
where Git does not create symlinks), run the following from the repository root
on macOS or Linux, replacing `<skill>` with the skill name from the table above:

```sh
mkdir -p .agents/skills
ln -s ../../plugins/<skill>/skills/<skill> .agents/skills/<skill>
```

Codex scans `.agents/skills/` for project skills. Each link points to the entire
skill folder, so Codex can read both `SKILL.md` and the files in `references/`.
In the project's Codex chat, type `$` and select the skill, for example
`harvard-negotiation-coach`. If a skill does not appear, restart the app and
reopen the project.

Example prompts:

> $harvard-negotiation-coach Help me prepare for a conversation with my manager about a raise. Then let's rehearse it.

> $testing-coach Here are the requirements for a signup form. What should I test, and what is missing from the spec?

> $test-automation-coach We have 300 manual regression cases. Is it worth automating them, and where do we start?

See [Build skills](https://learn.chatgpt.com/docs/build-skills#where-codex-loads-local-skills)
for details on project skill discovery.

### ChatGPT on the web and mobile

To use the skill in ChatGPT on the web or mobile, package the skill folder
as an OpenAI plugin and make it available in your ChatGPT workspace.
The ZIP in the table above is the Claude distribution. See
[Package your plugin](https://developers.openai.com/plugins/build/plugins)
for the required manifest and folder layout.

## Sources

Skills based on books contain an original summary of the ideas, not the text of the book. The books themselves are not included in this repository.

- **harvard-negotiation-coach**: Roger Fisher, William Ury, Bruce Patton. *Getting to Yes: Negotiating Agreement Without Giving In*. Russian edition: «Гарвардский метод переговоров» (Mann, Ivanov & Ferber).
- **test-automation-coach**: Mikhail Sidelnikov. *Let's automate it!* (in Russian).
- **testing-coach**: Roman Savin. «Тестирование Дот Ком, или Пособие по жестокому обращению с багами в интернет-стартапах» (Delo, 2007; in Russian).

---

Want to add or change a skill? See [CONTRIBUTING.md](CONTRIBUTING.md).
