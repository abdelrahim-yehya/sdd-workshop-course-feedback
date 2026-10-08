# PROMPTS.md — Every prompt, in order

Copy-paste reference for the whole workshop. Terminal commands are marked `bash`; everything else goes to your agent.

Specify prompts are short by design: **the detail comes from your answers to `/speckit-clarify`**, not from the prompt. Plan prompts are the exception, because they carry the technical decisions the team has already made. Each module in [`modules/`](modules/) carries the answer key for the questions the agent will ask.

> Substitute your agent's command form if it differs: `$speckit-specify` (Codex, ZCode) or `/skill:speckit-specify` (Kimi).

**Jump to:** [M0](#module-0) · [M1](#module-1) · [M2](#module-2) · [M3](#module-3) · [M4](#module-4) · [M5](#module-5) · [M6](#module-6) · [Recovery](#recovery-prompts)

---

## How we prompt

There is no separate interview step. Spec Kit already has a command that interrogates you, `/speckit-clarify`, so we use it instead of improvising our own. Three kinds of prompt:

**Seed, then clarify.** For specifications. A short `/speckit-specify` (a few sentences of what and why), then `/speckit-clarify` repeatedly until nothing is ambiguous.

**Decide, then command.** For the constitution, the plan and constitution amendments. These record decisions the team has already made, so the command carries them directly, in one short paragraph.

**Cascade.** `/speckit-tasks`, `/speckit-analyze` and `/speckit-implement`, with no arguments or one narrow scope.

*Don't append "ask me questions" to a slash command. The command template already instructs the agent to produce an artifact, so asked to do both, it generates.*

---

## Module 0

Done before the workshop (see the pre-work list):

```bash
uv tool install specify-cli==1.0.2
```

Done together in the session:

```bash
mkdir course-feedback && cd course-feedback
specify init . --integration claude
specify extension add git
specify check
```

---

## Module 1

### 1 — Constitution

```text
/speckit-constitution Create the principles for a small university
course-feedback website: a public form where students submit anonymous
feedback, and a private dashboard where professors review it.
Priorities: (1) every functional requirement is covered by automated
Jest tests, with a happy path and a failure path for every route,
written alongside the code they cover and passing before a task is done;
(2) submissions are anonymous, so no names, emails, IPs or identifiers
are ever stored; (3) all input is validated server-side before the
database, using parameterised queries only; (4) no dependencies beyond those a spec names, and no frontend or CSS frameworks;
(5) one feature is one spec and one commit, using Conventional Commits with tests green,
and any rule that must be broken is recorded in the spec with a written
rationale before it is implemented.
```

### 2 — Seed the spec

```text
/speckit-specify Students leave anonymous feedback on a course: a rating from 1 to 5 and a written comment. Professors read all of it on a private dashboard that shows nothing at all to anyone without credentials. That's the whole first feature — no multiple courses, no export, no charts, no student accounts.
```

### 3 — Clarify

```text
/speckit-clarify
```

Run it again until it stops asking questions.

📋 [Answer key →](modules/01-constitution-and-spec.md#14--let-the-tool-interrogate-you)

---

## Module 2

### 4 — Plan

```text
/speckit-plan A single Node.js service with Express, no build step, no bundler or transpiler. SQLite through the built-in node:sqlite module, in-memory for this feature. API: POST /api/feedback (public) and GET /api/feedback (protected), plus static file serving. Basic Auth as Express middleware reading PROFESSOR_USER and PROFESSOR_PASS from the environment, with a documented .env.example. Frontend: two static pages, index.html and dashboard.html, with vanilla JS using fetch and one plain CSS file. Jest plus supertest for tests; npm test runs the suite and npm start runs the server. Layout: src/ for code, public/ for static assets, tests/ mirroring src/. Validation in its own module so it is unit-testable independently of the routes. Schema: id, rating, comment, created_at. Keep route handlers thin with logic in modules.
```

### 5–7 — Cascade

```text
/speckit-tasks
```
```text
/speckit-analyze
```
```text
/speckit-implement
```

```bash
npm install && npm test && npm start
```

### 8 — Commit

```text
/speckit-git-commit
```

---

## Module 3

### 9 — Specify

```text
/speckit-specify Feedback now belongs to a course. Three fixed courses: CS101, AI202, ENG304 — no way to add more at runtime. Students pick one when they submit; professors can filter the dashboard by course or see everything. Everything from the first feature keeps working exactly as it does now.
```

### 10 — Clarify

```text
/speckit-clarify
```

📋 [Answer key →](modules/03-multi-course.md#33--clarify)

### 11 — Plan

```text
/speckit-plan Extend what we already have — don't restructure the project. Add the course to the schema and to both API routes, with the course list in one shared module. Extend the Jest suite to cover a valid submission per course, rejection of a missing or unknown course, and filtered versus unfiltered retrieval.
```

### 12–14 — Cascade

```text
/speckit-tasks
```
```text
/speckit-analyze
```
```text
/speckit-implement
```

### 15 — Commit

```text
/speckit-git-commit
```

---

## Module 4

### 16 — Specify (deliberately vague)

```text
/speckit-specify Professors can download all the feedback as a CSV from the dashboard, behind the same login as everything else. It has to survive whatever a student typed in the comment box — the file mustn't break, and opening it in a spreadsheet mustn't do anything dangerous. Also add Refresh and Download buttons to the dashboard.
```

### 17 — Make it sharpen the vague part

```text
/speckit-clarify Focus on what "the file mustn't break" and "mustn't do anything dangerous" have to mean precisely. Enumerate the specific cases.
```

📋 [Answer key →](modules/04-csv-export.md#42--let-it-sharpen-the-vague-part)

**If it doesn't raise formula injection:**

```text
What could go wrong if a student's comment starts with an equals sign and a professor opens the CSV in Excel?
```

### 18 — Plan

```text
/speckit-plan Extend what we have. Put CSV generation in its own module with no Express dependency so it's unit-testable directly. Add a protected export route that takes the same course filter as the list route. Add the two dashboard buttons. Cover every escaping case we just enumerated in unit tests, plus route tests for auth, headers, and the filter.
```

### 19 — Tasks, analyze, phased implement

```text
/speckit-tasks
```
```text
/speckit-analyze
```
```text
/speckit-implement Only the CSV generation module and its unit tests. Don't touch the routes or the frontend yet.
```
```text
/speckit-implement Now the export route, with its auth and header tests.
```
```text
/speckit-implement Now the Refresh and Download buttons, including the disabled-while-pending behaviour and error surfacing.
```

### 20 — Manual verification

```bash
curl -i http://localhost:3000/api/feedback/export                           # expect 401
curl -i -u professor:yourpassword http://localhost:3000/api/feedback/export # expect 200 text/csv
```

### 21 — Commit

```text
/speckit-git-commit Mention the escaping and formula-injection protections in the body.
```

---

## Module 5

### 22 — Amend the constitution

```text
/speckit-constitution Add a new design principle and keep every existing principle intact, bumping the document version. The UI must look modern and polished using only hand-written CSS.
Rules: (1) all colours and spacing are defined once as CSS custom properties on :root, with an 8px spacing base and no literal colour or spacing value anywhere else; (2) cards, inputs and buttons have an 8px radius, with layered subtle shadows instead of heavy borders; (3) every interactive element defines hover, focus-visible, active and disabled states, and focus indicators are always clearly visible and never removed; (4) text contrast meets WCAG AA; (5) layouts use Grid and Flexbox and work down to 360px wide with no horizontal scroll; (6) forbidden: CSS frameworks, resets, component libraries, icon fonts or packages, third-party web fonts, preprocessors and any build step; allowed: system font stacks and inline SVG.
```

### 23 — Specify

```text
/speckit-specify Make the app look and feel modern. No behaviour changes at all — same routes, same validation, same auth. Star rating instead of a number input. Feedback shown as cards with the course as a badge. Summary figures at the top of the dashboard. And every state properly designed: loading, empty, and error.
```

### 24 — Clarify

```text
/speckit-clarify
```

📋 [Answer key →](modules/05-design-system.md#53--specify-the-visual-work)

### 25 — Plan and cascade

```text
/speckit-plan Restyle the existing frontend only — server code and API contracts unchanged. Rebuild the stylesheet around a :root custom property block. Build the star rating from accessible radio inputs styled with CSS, so keyboard operation and screen-reader semantics come for free instead of being rebuilt in JavaScript. Existing tests must keep passing unmodified.
```
```text
/speckit-tasks
```
```text
/speckit-analyze
```
```text
/speckit-implement
```

### 26 — Verify the constraint held

```bash
cat package.json
grep -rn "cdn\|unpkg\|jsdelivr\|googleapis\|tailwind\|bootstrap" public/ src/
head -40 public/*.css
```

### 27 — Commit

```text
/speckit-git-commit
```

---

## Module 6

### 28 — Record the exception

```text
/speckit-constitution Record an authorised exception in a new "Authorised Exceptions" section, using the governance clause. Chart.js is authorised by name and nothing else, on the professor dashboard page only; the public student form stays free of third-party assets. It is loaded from a versioned CDN URL with a subresource integrity hash and crossorigin, and is not added to package.json. Rationale: accessible charting from scratch in canvas costs far more than it is worth, and this is one authenticated internal page. It weakens nothing else, and any future exception needs its own amendment. Keep every existing principle intact and bump the version.
```

### 29 — Specify

```text
/speckit-specify Feedback has to survive a server restart. And the dashboard gets a chart showing the average rating per course. Everything else stays exactly as it is.
```

### 30 — Clarify

```text
/speckit-clarify Focus on storage lifecycle and on how the chart should handle courses with no feedback.
```

📋 [Answer key →](modules/06-capstone.md#63--clarify--including-the-question-a-production-system-would-force-on-you)

### 31 — Plan

```text
/speckit-plan Extend what we have. File-backed SQLite with a configurable path, gitignored, schema initialised idempotently at startup. Add a protected stats route returning count and average per course. Load Chart.js on the dashboard page only, integrity-pinned, styled with our existing CSS custom properties, with the same numbers rendered as text alongside it. Tests must use an isolated database.
```

### 32 — Tasks, analyze, phased implement, converge

```text
/speckit-tasks
```
```text
/speckit-analyze
```
```text
/speckit-implement Only the persistence work: file-backed database, configurable path, gitignore, idempotent schema init, and the persistence tests. Not the stats route or the chart yet.
```
```text
/speckit-implement Now the stats route and its tests.
```
```text
/speckit-implement Now the chart and the text summary.
```
```text
/speckit-converge
```

### 33 — Final commit

```text
/speckit-git-commit
```

---

## Recovery prompts

Keep these to hand. They're the ones you'll actually reach for in a room of 20 people.

### `/speckit-clarify` is asking too many questions, or trivial ones

```text
Take your best guess on anything minor and just tell me what you assumed. Only ask me about decisions that would be expensive to reverse later.
```

### You want to move fast

```text
Go with your recommended option for all of those.
```

### Tests are failing

```text
The test suite is failing. Diagnose the root cause. If it's a defect in the implementation, fix it. If it's because the spec or plan was ambiguous or wrong, tell me which artifact is at fault and what it should say instead — don't silently work around it.

[paste the output]
```

### The agent violated the constitution

```text
This violates our constitution — you added [name it]. Remove it and rebuild in compliance. Then tell me which artifact let this through: was the constitution ambiguous, or did the plan skip its constitution check?
```

### It went off-script and rewrote too much

```text
You changed files outside the scope of the current tasks. List everything you modified that isn't referenced by a task in tasks.md, revert those changes, re-run the tests, and tell me which tasks are still incomplete.
```

### An earlier feature regressed

```text
Something we specified in an earlier feature stopped working: [describe]. Find the requirement, write a failing test that reproduces it, then fix the code so both the old and new specs are satisfied.
```

### `/speckit-implement` stalled or ran out of context

```text
Tell me which tasks in tasks.md are actually complete based on the current state of the codebase, not on what you remember doing. Then implement only the next incomplete phase.
```

### You need to know where you are

```bash
cat .specify/feature.json     # which feature is active
specify extension list        # what's installed
git log --oneline             # what's committed
```
