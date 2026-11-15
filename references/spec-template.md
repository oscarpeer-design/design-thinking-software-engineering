# Spec template (step 6: Specify)

Save as `docs/spec.md`. If that file exists, do not overwrite it: save as `docs/spec-<short-name>.md` and tell the user. Write bullet points, not paragraphs.

## Size limit

- Aim for about 1,000 words, counting headings and tables. A project with little to specify may need fewer; do not pad. If the count is over 1,100, cut.
- Check with `wc -w <file>`. If you have no shell, estimate and write "length estimated" under the title.
- A section with nothing to say gets "none".
- If the content does not fit in about 1,000 words, write one spec per group of components, each about 1,000 words, and make `docs/spec.md` a list of those files with one line each.
- Write concisely. State each fact once; do not repeat it in another section.

## Template

```markdown
# <Project name> spec

## Problem
<The how-might-we statement from step 2, then the user stories.>

## Success criteria
- <At most 3. Each is answerable with yes or no by running or inspecting the software.>

## Non-goals
- <Things this deliberately does not do.>

## Components
| Component | Responsibility | Owner |
|---|---|---|
| <name> | <one sentence> | user / Claude |

## Interfaces and data
- <Inputs, outputs, data structures used by more than one component, and what each component passes to the next.>

## Decisions
- <Decision>: <option chosen>, because <reason>. Decided by: user / Claude (delegated by user).

## Assumptions
- <Assumption>: confirmed (the user said so in writing) / unconfirmed

## Main risks (suggested by Claude; not yet confirmed)
- <Risk>: <what could go wrong>; affects <who or what>; suggested mitigation: <one action>. Most serious first. Cover technical, security and privacy, and social or ethical risks, drawing on the interview answers.

## Essential tests
- One test per success criterion, one per failure path, one per boundary value.

## Open questions
- <Anything not yet answered, or "none".>
```

## Filling it in

- Owner: write "Claude" for every component, except components the user has said they want to write and, in learning mode, the core component, which is "user". The user's reply at the step 6 GATE sets the final owners.
- Main risks: suggest them yourself; do not wait to be asked. A main risk is one that could harm people, expose or lose data, or force a redesign. Name the specific data item, group of people, or failure. Do not write general statements such as "ensure security" or "protect privacy". Give each a suggested mitigation. Write "none identified" only with one sentence saying why. The user confirms or changes the section at the step 6 GATE; once they do, remove "not yet confirmed" from the heading.
- Decisions: list decisions the user made and decisions the user delegated to you. A decision from the "Decisions you may make yourself" list does not go here.
