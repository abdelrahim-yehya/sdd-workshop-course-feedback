# Module 2 — Plan → Tasks → Analyze → Implement

**⏱ 35 minutes** · [← Module 1](01-constitution-and-spec.md) · [Next: Module 3 →](03-multi-course.md)

---

## Goal

Take the spec through the full quality-gated cascade to a running, tested MVP. This is the longest module and the one where the process either earns trust or loses it.

---

## 2.1 — `/speckit-plan` — the technical design

Now, and only now, the tech stack. Same technique as the constitution: interview first, generate second.

### 👉 Prompt 1 — Plan

```text
/speckit-plan A single Node.js service with Express, no build step, no bundler or transpiler. SQLite through the built-in node:sqlite module, in-memory for this feature. API: POST /api/feedback (public) and GET /api/feedback (protected), plus static file serving. Basic Auth as Express middleware reading PROFESSOR_USER and PROFESSOR_PASS from the environment, with a documented .env.example. Frontend: two static pages, index.html and dashboard.html, with vanilla JS using fetch and one plain CSS file. Jest plus supertest for tests; npm test runs the suite and npm start runs the server. Layout: src/ for code, public/ for static assets, tests/ mirroring src/. Validation in its own module so it is unit-testable independently of the routes. Schema: id, rating, comment, created_at. Keep route handlers thin with logic in modules.
```

**Read `plan.md`, `data-model.md`, and the contracts directory.** Two things to check as a group:

1. **Did the plan honour the constitution?** Most plan templates include a constitution-check gate — find it. If the agent had proposed React here, this gate is where it should have caught itself.
2. **Are there decisions you disagree with?** This is the cheapest possible moment to change them. Editing `plan.md` now costs nothing; changing it after `/implement` costs a regeneration.

---

## 2.2 — `/speckit-tasks` — the executable breakdown

### 👉 Prompt 2

```text
/speckit-tasks
```

No arguments — everything it needs is in the artifacts.

**Open `tasks.md`.** This is the most persuasive file in the toolkit. Point out:

- **Phases**: Setup → Foundational (blocking prerequisites) → one phase per user story in priority order → Polish
- **Dependency ordering** — tasks are sequenced, not listed
- **`[P]` markers** — tasks that can safely run in parallel
- **Test tasks interleaved within each user story**, not bolted on at the end. That's the constitution's testing principle showing up as structure.
- Each task references the requirement it satisfies

Count them. Typically 25–40. Ask: *how long would writing this breakdown by hand have taken?*

---

## 2.3 — `/speckit-analyze` — the consistency gate

A read-only pass across `spec.md`, `plan.md`, and `tasks.md` looking for conflicts, gaps, and orphans: a task with no matching requirement, a plan choice that contradicts the spec, a constitutional violation.

### 👉 Prompt 3

```text
/speckit-analyze
```

It writes nothing. It produces a report.

**When it flags something, fix it at the source, not in the report:**

| Problem type | Go back to |
|---|---|
| A requirement is missing, vague, or contradictory | `/speckit-clarify` |
| A design or stack decision is wrong | `/speckit-plan` |
| The breakdown doesn't match the design | `/speckit-tasks` (regenerate) |

Re-run `/speckit-analyze` until clean. This discipline — *fix it where it's owned* — is the single most transferable habit in the workshop. Patching downstream artifacts is how spec-driven projects rot back into ordinary ones.

---

## 2.4 — `/speckit-implement` — build it

### 👉 Prompt 4

```text
/speckit-implement
```

The agent now works through `tasks.md` in dependency order: implement, write its tests, verify, next.

### Verify it actually works

```bash
npm install
npm test
npm start
```

Then in a browser:

- `http://localhost:3000` — submit feedback
- `http://localhost:3000/dashboard.html` — expect a Basic Auth challenge, then your submission

**If tests fail, do not open the editor.** Hand the failure back as evidence:

```text
The test suite is failing. Diagnose the root cause. If it's a defect in the implementation, fix it. If it's because the spec or plan was ambiguous or wrong, tell me which artifact is at fault and what it should say instead. Don't silently work around it.

[paste the output]
```

That second sentence preserves the invariant that the spec is the source of truth, and surfaces the case where the real bug is upstream.

---

## 2.5 — Commit the MVP

### 👉 Prompt 5

```text
/speckit-git-commit
```

Check with `git log -1` and `git show --stat`. If the message is vague, ask for a better one — the agent has full context and commit messages are documentation.

---

## ✅ Checkpoint

- [ ] `npm test` passes
- [ ] Student form stores feedback; dashboard requires auth and displays it
- [ ] `/speckit-analyze` reported clean
- [ ] One semantic commit exists
---

[Next: Module 3 — Multi-Course Support →](03-multi-course.md)
