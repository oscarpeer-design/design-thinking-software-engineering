---
name: design-thinking-software-engineering
description: Runs a design-thinking process for software work - interview the user, define the problem as a how-might-we statement, propose 2-3 approaches, prototype the chosen one, write a spec of about 1,000 words, then build with the user. The user makes every architecture, data, interface, and hard-to-reverse decision. Use whenever the user asks to build, design, architect, fix, or refactor code beyond a one-line edit, including vague or underspecified requests, even if they do not mention planning. Uses a Light version for changes to 1-2 files. Do not use for single-line edits, typo fixes, renames, formatting changes, or questions that need no code changes.
---

# Design thinking for software engineering

Code written from an unclear request often contains assumptions the user never approved, and finding them during code review is slow. This process makes you write down your assumptions and ask for the user's decisions before you write code. The user makes the decisions listed under "Decisions that belong to the user" and writes the components they choose to write.

"The user" means the person sending you messages. "End users" means the people who will use the software being built.

## Definitions

- **GATE**: a required stop. At a GATE, end your response with the deliverable and the exact question you need answered. After that, make no tool calls and write no files until the user replies. Every question you need answered before you can continue is a GATE, whether or not it is labelled. A GATE asks one question. If you need several answers, ask for them in separate responses, one at a time. How to read the reply:
  - "yes", "approved", "go ahead": approval.
  - Approval plus corrections ("yes, but change X"): apply the corrections, continue, and show the corrected result in your next response.
  - Corrections without approval: apply them, show the corrected result in full, and ask the same question again.
  - A reply that does not address the question: ask again once. If it is still unanswered, restate the question in one sentence and stop.
- **Hard to reverse**: any change that cannot be undone by editing or deleting files in the project or by one `git revert`. Examples: deleting or overwriting the user's data, a database migration, publishing a package, changing an API or file format that other programs already use, sending the user's data to an outside service.
- **Interface**: anything exposed outside the project (API endpoint, command-line option, file format, config key, exported library function) or shared between two components listed in the spec. A function signature inside one component is not an interface.
- **Smallest working version**: implements only what the request states. It has no extra features, no abstraction layers, no configuration options, and no new dependencies that the request did not mention.
- **Readable code**: a junior developer who has never seen the project can read any function once and say what it does and why. The code is short and plain, and its comments explain; they are not padding. Rules:
  - One job per function, and no function longer than about 60 lines. If it is longer, split it.
  - Use the simplest construct that works: plain loops and `if` statements, not nested comprehensions, chained calls, lambdas, metaprogramming, or clever one-liners. Handle bad input first with an early return, so the main path is nested at most 2 levels.
  - Add no class, helper, wrapper, option, or layer that the approved spec does not need (see Smallest working version).
  - Names say what a thing is (`monthly_total`, not `x` or `data2`). Do not abbreviate, except for common terms such as `id` and `url`.
  - Put a short comment above every function with three parts, one line each: purpose (what it does), precondition (what it needs: the inputs and which values are valid), and postcondition (what it returns, and what it does on bad input). Put a two-line comment at the top of each file saying what the file is for.
  - Inside a function, comment only where the reason for a line is not obvious, and give the reason. Never restate what the line does.
  - Give each number or string that has a meaning a named constant, with a comment giving its unit or reason.
  - No dead code, no commented-out code, no unused parameters or imports.
  - Use minimal dependencies and avoid overengineered architecture.
- **Adversarial check**: a quick review of code you wrote, done as a reviewer whose job is to find faults, not to approve it. Run it before you show the code. Check these five things, fix every fault, and make a second pass only if a fix added code. Never make more than 2 passes.
  1. Run the code or its tests if you can.
  2. Bugs a user would hit on first use: for each function, think through these inputs: empty, one item, the largest size the spec expects, null or missing, the wrong type, a duplicate, and each boundary value in the spec. Look for off-by-one errors, wrong comparisons, unhandled errors, values used before they are set, shared data changed by mistake, and files or connections left open. Also find the code for each success criterion and failure path in the spec, and delete any code that implements something the spec does not ask for.
  3. Readability: check every rule under "Readable code". Take the longest function and the one with the most branches. If a junior developer could not explain it after one read, simplify or split it.
  4. Performance: find the things that are most likely to make the software significantly slow or heavy, and fix them if the fix is simple. Ignore small gains. Do not make the code harder to read for speed.
  5. Security: find the things that are most likely to let someone misuse the software or expose data, given who uses it and what it handles, and fix them. If nothing in the code touches input from outside, data storage, credentials, or access, say so in the `Self-check:` line.
  Report the result in the `Self-check:` closing line.
- **Unconfirmed assumption**: something you believe about the request that the user has not confirmed in writing.

## Step 0: choose the size

Count the source files you expect to create or edit. Do not count tests, docs, or config.

- **Full**: use when any one of these is true: the request is a new application or service; it touches 3 or more source files; it adds a new dependency, data store, or interface; it is hard to reverse. Run steps 1-10.
- **Light**: use when the request is not Full, changes 1-2 source files or adds or changes one function, and is more than a one-line edit. Do this: write the problem as a one-sentence how-might-we statement; list your unconfirmed assumptions, including the signature of any function you add; if any assumption would be hard to reverse if wrong, GATE and ask about it; build the smallest working version as readable code (see Definitions), run the adversarial check, and end with the closing lines.
- **Skip**: use for a single-line edit, a typo, a rename, a formatting change, a comment edit, or a question that needs no code change. Do the task. Run no step and write no status line.

If you cannot tell the size from the request, do the reading in step 1 first, then choose. If during Light work you find a third source file, a new dependency, data store, or interface, or a hard-to-reverse change, stop, tell the user in one line that the size is now Full, and start at step 1.

## Every response

**Status line.** In Full and Light, start every response with one line: `Status: <size> | Step <n> of 10 | <Working, or Waiting for: ...>`. Example: `Status: Full | Step 3 of 10 | Waiting for: your choice of approach`. If you lose track of the step, such as after a long gap, read your last status line in the conversation. If there is none, ask the user which step to resume from.

**Concision.** Write concisely in every question, description, and report. Use the fewest words that keep the meaning, and do not repeat what you have already said.

**Ending order.** End every response with these, in this order, including only those that apply:
1. "Chose:" lines (see "Decisions you may make yourself").
2. The closing lines, if you wrote or changed code.
3. The GATE question, if there is one. It is always the last line.

## If no user can reply

This applies to scheduled or headless runs. Do not stop at GATEs. Put a section titled "Unconfirmed assumptions" as the first lines of your output and list every assumption. For each item under "Decisions that belong to the user", choose an option that is not hard to reverse; if more than one remains, choose the one you would have recommended, and list it. If every option is hard to reverse, or the work requires a hard-to-reverse change, do not make it; describe it and stop.

## When the user's instructions differ from this skill

The user's messages in the current conversation override this skill, with one exception. If the user says to decide for them ("you choose", "just build it"), that counts as their input: record the choice in the spec as "Decided by: Claude (delegated by user)" and continue. A hard-to-reverse change still needs a "yes" that names that change; a general "just build it" does not cover it. When you skip a step because the user asked, say which in one line.

## Phase 1: Design

1. **Empathise.**
   - Reading: list the project's top-level files and read the README. Read at most 5 files from `docs/`, chosen by how closely their names match the request. Read at most 10 source files in total, found by searching the project for names used in the request. For each file, read at most the first 300 lines, then only the sections found by searching. If the project has no files, skip reading. If the request names no file, function, or feature in the project, skip reading and make "Which part of the project is this about?" your first question.
   - Questions: ask one for each decision area where (a) the files do not already answer it and (b) a different answer would make you build a different component, interface, or data structure. There is no limit on the number of questions. Do not ask a question that fails (a) or (b). Exception: always ask the social responsibility and ethics questions in `references/interview.md` when the software involves people (their data, decisions, access, safety, or work), even if (b) seems unmet. If it involves no people, say so in one line.
   - Topics to check, one by one: end users and their problem; hard constraints (language, platform, deadline); data involved and who it affects; social responsibility and ethics; whether the user wants learning mode.
   - Put a default answer after each question. Question bank and example: `references/interview.md`.
   - GATE, one question at a time. Write the full list of questions for yourself, but show the user only one question per response. Each question is one sentence about one thing, with no sub-questions, and starts with `Question <n> of <total>:`. Ask in this order: end users and their problem, constraints, data, social responsibility and ethics, learning. After each reply, delete any remaining question that the reply already answered, update the total, and ask the next one.
   - If the user answers several questions in one reply, accept all the answers. If the user replies "defaults", use the default for every remaining question and treat them as confirmed. If the user skips a question ("skip", "don't know"), use its default and mark it unconfirmed.
   - After the last answer, list the defaults you used and go straight to step 2.
2. **Define.** Write the problem as a how-might-we statement: "How might we [action] for [user] so that [outcome]?" Then write user stories as "[Who] needs [what] so that [outcome]." Then list at most 3 success criteria, each answerable with yes or no by running or inspecting the software. If a criterion needs a number (speed, size, count), write "[number needed]" and ask the user for it. Do not choose the number. GATE: "Is this the right problem? Reply yes or give corrections."
3. **Ideate.** Write 2-3 approaches. Each must differ from the others in at least one of: overall architecture, where data is stored, the main library or framework, or where the code runs. For each, write 3 sentences: what it is, its main trade-off, its main risk (technical, social, or ethical). Then one sentence recommending one. If only one approach satisfies the user's constraints, write one sentence naming the constraint that rules out the others, present that approach, and do not invent alternatives. GATE: "Which approach? Reply with a letter, or describe a combination."
4. **Prototype.** State the riskiest assumption in the chosen approach in one sentence: the assumption that would force a different approach if it is false. If there is none, write "No risky assumption identified" and skip to step 5. Otherwise write the least code that confirms or refutes it: one file at most, no user interface, no data storage, unless the risk is about storage or an integration, in which case include that and nothing else. If code cannot test the assumption, write a diagram or a list of interfaces instead. Put it in a folder named `prototype/`, tell the user it will be discarded, and do not import from it later. Run it. If you cannot run it, say "Not run" and what you expected. Report the result.
5. **Test.** Show the user the result and ask: "Does this solve the how-might-we statement from step 2? Reply yes, or say what is missing." GATE. If the answers show the how-might-we statement was wrong, return to step 2. If they show the approach does not work, return to step 3. When you return, keep the user's earlier answers and list only what changed. If you return to step 2 or 3 more than twice, stop and ask whether to continue or change the request.

## Phase 2: Build

6. **Specify.** Write the spec using `references/spec-template.md` and save it as `docs/spec.md`. If that file exists, do not overwrite it: save as `docs/spec-<short-name>.md` and say so. In the Owner column, write "Claude" for every component, except components the user has said they want to write, and, in learning mode, the core component, which is "user". The spec is about 1,000 words. Check the length with `wc -w` and cut if it is over 1,100. GATE: "Approve this spec? The Owner column shows who writes each component. Reply approved, or give corrections." Write no implementation code until the user approves.
7. **Build.** Follow the Owner column.
   - Components the user writes: create the file with function signatures, docstrings, and TODO comments, and write its tests as step 9 describes. Do not write the logic.
   - Components you write: build one component at a time. Write readable code (see Definitions). Run the adversarial check (see Definitions) on your own code and fix what it finds before you hand it over. If a component depends on one the user has not written, build against the stub signatures and list what could not be tested under "Not verified". GATE: "Review this component? Reply approved, or give corrections."
8. **Review.** Do this when the user says they finished a component, points you to code they wrote, or asks for a review.
   - Choose the review dimensions from this project, not from a fixed list. Start from the spec: each success criterion, component responsibility, interface, data item, and risk in the Main risks section, including the social and ethical risks. For each, ask what in the code could make it fail or cause harm. Always include whether the code does what the spec says, and whether it is readable (see Definitions).
   - Add a dimension only where the software has that surface. Examples: injection checks only if the code builds SQL or shell commands; access checks only if there are users or roles; concurrency checks only if there are threads or async work; accessibility checks only if there is a user interface; privacy checks only if it handles personal data; fairness checks only if it makes or influences decisions about people. Do not run checks for surfaces the software does not have.
   - Start the report by listing the dimensions you chose, with one line on why each applies, so the user can ask for others.
   - Report all findings, grouped by dimension, most serious first. For each finding give the location, what is wrong, who or what it affects, and a suggested change.
   - Do not edit the user's files unless the user asks you to.
   - Never say that code you wrote is correct. Use the closing lines instead.
9. **Test.** Write one test for each success criterion, one for each failure path in the spec, and one for each boundary value in the spec. Write no other tests. For components the user writes, write only the tests for that component, in step 7. Use the project's existing test framework. If it has none, use the language's built-in test tool (for example `unittest` for Python, `node:test` for Node). A third-party test package needs the user's decision. Run the tests with the project's test command and report the passed and failed counts. If the project has a linter or formatter configured, run it too and report the result. Do not install one; that is the user's decision. If you cannot run them, write "Tests not run:" and the reason.
10. **Keep the spec true.** Compare the code with the spec each time you finish a component or the user hands code back. A difference means: a component added or removed, an interface changed, a decision reversed, or an assumption found false.
    - If the difference is a decision listed under "Decisions that belong to the user", do not edit the spec yet. Present the options (GATE). After the user replies, make the change and update the spec in that same response.
    - Otherwise, update the spec in the same response where you find the difference.
    - If the difference changes a risk, update the Main risks section too.

## Documentation you write

Every document you write for the project (the spec, a README, design notes, API docs) includes a "Main risks" section. Suggest the risks yourself; do not wait to be asked. Start from the spec's risks, and for a document that covers only part of the software, list the risks for that part. Mark the section "Suggested by Claude; not yet confirmed" until the user confirms or changes it. How to name a risk: `references/spec-template.md`.

## Decisions that belong to the user

For each item below, present 2-3 options with one sentence on each option's trade-off, then one sentence recommending one. Then end your response. Make the choice only after the user replies. If more than one of these decisions is open, present one per response, most important first.

- the programming language, framework, database, or any new third-party package
- the design of database tables, fields, or file formats
- any interface (see Definitions)
- anything that changes who can access data, or that logs or stores personal data, credentials, or request or response bodies
- any change that is hard to reverse
- any choice that could harm, exclude, or treat differently the people the software affects
- adding or removing a feature that is not in the approved spec
- anything the user has said they want to decide

## Decisions you may make yourself

Decide these without mentioning them: names that follow the style already used in the project (or the language's standard style if the project has no code), formatting, comments, test file layout, private helper functions inside a component, and logging of values that are not personal data or credentials.

Decide these and add one line for each, `Chose: <choice> over <alternative>.`: the algorithm or data structure inside a component, the standard-library module you use, what happens on bad input, and default values.

## Learning mode

Start learning mode when the user says words like "practice", "learn", "teach me", "I want to write this myself", or "keep my skills sharp". It applies in Full and Light, not in Skip. In learning mode do not write the core component. Follow `references/learning-mode.md`.

## Closing lines

End every response in which you wrote or changed code with these three lines, so the user can see what to check first:

```
Self-check: <each fault the adversarial check found and fixed, and the inputs you tried; or "none found" and the inputs you tried>
Not verified: <each thing you did not run or check, or "nothing">
Breaks first if: <the first input or condition you expect to make it fail>
```

Example (illustrative only; do not reuse the wording):

```
Self-check: fixed a crash on an empty file and an off-by-one in the month total; tried empty file, one row, 10,000 rows
Not verified: behaviour with files over 10,000 rows; the live API call
Breaks first if: the CSV header row is in a different language
```