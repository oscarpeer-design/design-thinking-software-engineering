# Learning mode

Use this when the user says they want to practise, learn, or keep their skills sharp. It applies in Full and Light, not in Skip. Writing the code for them defeats that purpose. Your job is to give them a defined problem to solve and specific feedback on their answer.

## Choosing the core component

The core component is the one component in the spec that contains the main algorithm or business rule. If two components qualify, choose the one that needs the most decisions about how to handle different cases. Write "user" as its Owner in the spec draft. The user's reply at the step 6 GATE confirms or changes it. In Light, where there is no spec, name the function in your assumptions list and GATE: "Will you write this one? Reply yes, or name another." You write all other components.

## Scaffold formats

Use format 1 unless the situation below says otherwise.

1. **Skeleton**: the file with function signatures, docstrings, and TODO comments where the logic goes, plus the failing tests that step 9 requires for that component. Do not add tests to reach a number. Default.
2. **Fill in the gap**: everything is written except one function, marked with a TODO. Use when the user says they are new to the language or topic.
3. **Outline first**: before any code, ask the user to describe their approach in plain words or pseudocode, then comment on it. Use when the user says "let me think it through" or "let me try first".
4. **Review my attempt**: the user writes the code and you review it using step 8. Use when the user pastes or points to their own attempt.

## Giving help

Use these steps in order. Move to the next step only when the user says they are still stuck or asks for more help.

1. Ask one question that points at the problem, for example "what does the function return when the list is empty?"
2. Give a hint about the approach, without code.
3. Show a similar example using different input.
4. Give the full solution. Do this only when the user asks for it in words such as "show me the answer".

## Reviewing the user's code

- Choose the review dimensions for this software as step 8 describes.
- Start with one thing that works.
- Then list all issues, most important first. Mention style only if the user asks.
- For each issue give the location and why it matters.
- Do not edit the user's file. The user makes the fix.

## Leaving learning mode

- "Just finish it", "do it for me", "I'm stuck, can you do this one?", and similar words apply to the component being discussed only. Write that component, say nothing further about learning mode, and stay in learning mode for the next component.
- "Leave learning mode" or "stop teaching" ends learning mode for the rest of the task.

## Rules

- A scaffold asks the user to write one function, or at most 40 lines.
- Do not give a full solution unless the user asks for it.
- When the user finishes a component, ask one question about their code, such as "why does this handle an empty input the way it does?" Ask at most one question per component.
- Run the adversarial check (see Definitions in `SKILL.md`) on each scaffold and each component you write before you show it.
- Write concisely.
