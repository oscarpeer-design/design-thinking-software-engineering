# Interview guide (step 1: Empathise)

Read this before writing the questions for step 1.

## Rules for asking

1. Send all questions in one message, as a numbered list.
2. Ask one question for each decision area where the files do not answer it and a different answer would make you build a different component, interface, or data structure. There is no limit on the number of questions. Also ask the social responsibility and ethics questions below when the software involves people. Do not ask any other question. Write each one concisely.
3. Group the questions under the topic headings below, so the user can answer area by area.
4. Skip any question that the project's files already answer.
5. Write a default answer after each question, for example "(default: just you)". The user can reply "defaults" to accept all of them.
6. For questions the user leaves unanswered, use the default and list those defaults in your next response. Mark them "unconfirmed" in the Assumptions section of the spec. If the user replied "defaults", mark them "confirmed".
7. If no user can reply, use the defaults, mark them "unconfirmed" in the spec, and list them under "Unconfirmed assumptions" at the top of your output.

## Question bank

Go through each topic below. Rewrite the wording to fit the request, and drop any question that fails the test in rule 2, except the social responsibility and ethics questions.

**Problem and end users**
- Who will use this, and what are they trying to get done?
- What do they do today, and what is wrong with that?
- What result would make you say this worked?

**Constraints**
- Which language, framework, platform, or hosting must be used?
- Is there a deadline, an expected data size, or a speed requirement? If a number is needed, ask for it.
- What existing systems must it connect to?

**Data**
- What data does it read or store, and who could be affected if that data leaks or is wrong?

**Social responsibility and ethics**
Ask these whenever the software involves people (their data, decisions, access, safety, or work). They are exempt from the test in rule 2: ask them even if the answer may not change the design, because the answers feed the Main risks section of the spec. Drop one only if the project files or the user have already answered it.
- Who is affected besides the end users: other people, communities, or workers? (default: no one else known)
- Who benefits, and who could be excluded or disadvantaged, for example by disability, language, age, cost, or poor internet access? (default: none known)
- Does it make or influence decisions about people? Could it treat groups differently, and can a person review or override a decision? (default: it makes no decisions about people)
- What personal data does it use, did those people agree to that, how long is it kept, and who can see it? (default: only the user's own data, kept locally)
- How could someone misuse it to harm others, for example for surveillance, harassment, fraud, or discrimination? (default: no misuse identified)
- If it is wrong or fails, who is harmed, how badly, and can the harm be undone? (default: only the user, and it can be undone)
- Do people know the software is being used, and can they find out how it reaches its results? (default: yes, a single user runs it)
- Which laws, regulations, or policies apply, such as privacy, accessibility, or sector rules? (default: none known)
- Where does any data or model it relies on come from, and are you allowed to use it? (default: the user's own data)
- What are the implications of this software for society?
- Does it use notable energy, computing, or hardware? Ask only if it runs at large scale. (default: no)

**Learning**
- Do you want to practise a skill while building this? (learning mode)

If the request names no file, function, or feature in the project, the first question is: "Which part of the project is this about?"

## Example

Request: "build me a tool that tracks my expenses."

Questions:
1. Is it for you only, or for more than one person? (default: you only)
2. Where does the data come from: typed in, a bank export, or receipts? (default: a CSV export from the bank)
3. What output do you want: monthly totals, categories, or alerts? (default: monthly totals by category)
4. Where should it run: command line, web page, or phone? (default: command line)
5. Is sending data to an online service allowed? (default: no, all data stays on your computer)
6. Will other people's financial details, such as a partner's, appear in the files? (default: no)
7. Do you want to practise a skill while building this? (default: no)

How-might-we statement and user story for step 2:
> How might we show a single user where their money goes each month without sending their bank data anywhere?
>
> A single user needs a local tool that turns their bank's CSV export into monthly totals by category so that they can see where their money goes.
