# Module 1 — The Constitution & The First Spec

**⏱ 25 minutes** · [← Module 0](00-setup.md) · [Next: Module 2 →](02-plan-tasks-implement.md)

---

## Goal

Establish the project's non-negotiable principles, then write a specification that describes *what* the product does without leaking *how* it is built, using short prompts and letting the agent do the interrogating.

---

## 1.1 — Why the constitution comes first

The constitution (`.specify/memory/constitution.md`) holds the rules that every later phase is evaluated against: testing discipline, architectural constraints, security posture, dependency policy.

It exists because of a failure mode you will otherwise hit repeatedly: **restating the same constraint in every spec.** "Test every route." "No frameworks." "Auth required." Ten specs later, one of them omits a line and the agent quietly does something else.

| Constitution | Spec |
|---|---|
| True for the lifetime of the project | True for this feature |
| "All state-changing routes require auth" | "The dashboard shows a course filter" |
| "No frontend frameworks" | "Ratings are 1–5 stars" |
| Changing it is a team decision | Changing it is a normal iteration |

This is the piece most teams skip, and it is the piece that determines whether SDD holds up past feature three.

---

## 1.2 — Setting the principles

The constitution is the one artifact where there is nothing to discover: the principles are decisions the team has already made. So there is no interview here. You hand the agent the principles in a single command and let it turn them into a structured, versioned document. The interrogation comes in 1.4, where the spec has real unknowns.

### 👉 Prompt 1 — generate the constitution

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

**Open `.specify/memory/constitution.md` and read it.** Check three things:

- Every one of the five priorities became a principle, and the "broken rules are recorded in the spec first" part became a governance clause.
- Nothing was dropped or softened.
- Nothing was added that you didn't ask for. This is the file everything else is judged against, so if the agent invented a principle, challenge it or delete it.

---

## 1.3 — Writing a specification

One rule dominates this step:

> **The spec describes behaviour, not technology.** No Express. No SQLite. No Jest. Those go in `/speckit-plan`.

This separation is not bureaucratic. It exists so requirements survive a change of stack, and so that reviewing *what we're building* isn't tangled up with *how*. If your spec mentions a library name, you've written a plan.

### 👉 Prompt 2 — seed the spec (short, deliberately)

```text
/speckit-specify Students leave anonymous feedback on a course: a rating from 1 to 5 and a written comment. Professors read all of it on a private dashboard that shows nothing at all to anyone without credentials. That's the whole first feature, no multiple courses, no export, no charts, no student accounts.
```

Three sentences. Notice what they contain: the two audiences, the shape of the data, the one hard security property, and an explicit scope boundary. Notice what they *don't* contain: validation rules, ordering, empty states, error handling. Those are coming, from the agent, not from you.

**Open `spec.md` and read it.** Look for:

- **User stories, prioritised** (P1, P2, P3) — your three sentences decomposed into independently testable slices
- **Numbered functional requirements** — what tasks will trace back to
- **`[NEEDS CLARIFICATION]` markers** — genuine ambiguities the agent refused to invent answers for. That refusal is a feature.

---

## 1.4 — Let the tool interrogate you

`/speckit-clarify` asks up to five targeted questions about underspecified areas and writes your answers back into `spec.md`. It's the cheapest quality gate in the process: answering here costs 20 seconds, discovering the same gap after implementation costs 20 minutes.

### 👉 Prompt 3 — broad pass, no arguments

```text
/speckit-clarify
```

**Answer key** (if the agent asks something not on this list, decide for yourself and say so out loud):

| If it asks about… | We're going with |
|---|---|
| Invalid submissions | Reject with a clear message saying what to fix. Store nothing. Missing rating, rating outside 1–5, empty comment, or comment over 1000 chars. |
| Where errors appear | Inline in the form, with the typed comment preserved. |
| After a successful submit | Confirmation message, form resets, ready for another. |
| Ordering on the dashboard | Newest first. |
| Pagination | None this feature. Show everything. |
| Empty dashboard | An explanatory empty state, not a blank page. |
| Timestamps | Stored ISO 8601, displayed human-readably. |
| Credentials | One shared professor username and password, from environment variables. |
| What an unauthenticated visitor sees | A challenge, and zero feedback content. Not a redirect to a page that leaks counts. |

### 👉 Prompt 4 — mop up

```text
/speckit-clarify Focus on anything still marked NEEDS CLARIFICATION.
```

Repeat until clean.

**Re-open `spec.md`.** Your answers are now requirements, numbered and version-controlled. You've just done requirements engineering in five minutes, from three sentences, and there's a written record of every decision.

---

## ✅ Checkpoint

- [ ] `.specify/memory/constitution.md` has numbered principles and a governance clause, and reflects the six principles you pasted (testing, dependencies, auth, privacy, input handling, increments)
- [ ] `specs/001-*/spec.md` exists with prioritised user stories and numbered requirements
- [ ] No tech stack names appear anywhere in `spec.md`
- [ ] No `[NEEDS CLARIFICATION]` markers remain

---

## 💬 Discussion (2 min)

> Compare your `spec.md` with the person next to you. Same three-sentence seed, different conversations, how far apart did you end up? That gap is the honest measure of how much the dialogue is doing, and it's the thing to be careful about when two people on your team specify adjacent features.

---

[Next: Module 2 — Plan → Tasks → Implement →](02-plan-tasks-implement.md)
