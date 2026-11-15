# Design Thinking for Software Engineering

A Claude skill that applies a design-thinking process to software work. Before any code is written, Claude interviews you, defines the problem, proposes approaches, prototypes the riskiest assumption, and writes a short spec. You make the architecture, data, interface, and hard-to-reverse decisions, and you write the components you choose to write.

**Status:** experimental. It has not been evaluated against a test set yet.

## Why

Code written from an unclear request often carries assumptions nobody approved, and those are slow to find in review. This skill makes the assumptions visible early, keeps the important decisions with the engineer, and makes social and ethical risks part of the design instead of an afterthought.

## What it does

| Step | What happens |
|---|---|
| 1. Empathise | Claude reads the project, then asks the questions whose answers would change the design, including social responsibility and ethics questions. |
| 2. Define | A how-might-we statement, user stories, and up to 3 yes/no success criteria. |
| 3. Ideate | 2-3 approaches that differ in kind, each with a trade-off and a risk. You pick. |
| 4. Prototype | A disposable test of the riskiest assumption. |
| 5. Test | You review the result. The process can loop back to steps 2 or 3. |
| 6. Specify | A spec of about 1,000 words with components, owners, and suggested main risks. You approve it before any implementation. |
| 7-10. Build | Components are built one at a time, reviewed against checks drawn from your own spec, tested, and the spec is kept in step with the code. |

Claude stops and waits for your reply at each gate. Small changes use a lighter version, and trivial edits skip the process.

Optional **learning mode**: say you want to practise, and Claude scaffolds the core component for you to write, then reviews your attempt.

## Install

**Claude app (web or desktop):** download `design-thinking-software-engineering.zip` from this repo, then go to Customize > Skills > Create skill > Upload a skill and upload it. The zip has the skill folder at its top level, as the upload requires.

**Claude Code:** clone or copy this folder to `~/.claude/skills/design-thinking-software-engineering/` for all projects, or to `.claude/skills/design-thinking-software-engineering/` in one project.

Start a new conversation after installing.

## Try it

> Help me build an AI tool that does market research on ETFs and gives users evidence-based valuation views.

You should see a status line and a set of interview questions before any code.

## Contents

```
SKILL.md                      the process and rules
references/interview.md       question bank, including social responsibility and ethics
references/spec-template.md   spec template and size limit
references/learning-mode.md   scaffolding and feedback for practice
```

## Customising

The numbers in `SKILL.md` (reading limits, spec length, finding caps) are defaults. The list of decisions reserved for the user is under "Decisions that belong to the user". Edit it to match how you work.

## Rebuilding the zip

After changing any skill file, commit the change, then rebuild the zip from the committed files so it never goes stale:

```
git archive --format=zip --prefix=design-thinking-software-engineering/ -o design-thinking-software-engineering.zip HEAD SKILL.md references
```

Then commit the new zip.

## Limitations

- It adds several stops before any code is written. That is intended, but it is slower than asking for code directly.
- It guides Claude. It cannot enforce anything, so you still review.
- The interview and risk sections prompt for social and ethical considerations. They do not replace legal or compliance advice.
