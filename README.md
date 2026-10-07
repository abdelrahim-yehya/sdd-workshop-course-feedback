# Spec-Driven Development with GitHub Spec Kit

**A 4-hour hands-on workshop for software engineering teams**

You will build a working course-feedback application end to end without writing application code by hand. Every line is produced by an AI agent from a specification you control. By the end you will have a repository with six commits, a test suite, and more importantly, a repeatable process you can take back to your own codebase.

---

## What you'll build

A course feedback app:

- Public student form: star rating + free-text comment, scoped to a course
- Private professor dashboard behind Basic Auth, with filtering
- Secure CSV export endpoint
- Persistent SQLite storage and a per-course ratings chart

**Stack:** Node.js · Express · SQLite · Vanilla JS/CSS · Jest

The app is deliberately small. The app is not the point — the **workflow** is the point.

---

## The core idea

> In Spec-Driven Development, the specification is the source code. The codebase is a build artifact.

That reframe is the whole workshop. For an engineering audience it maps cleanly onto things you already do:

| Traditional | Spec-Driven Development |
|---|---|
| Source code is the artifact of record | `spec.md` is the artifact of record |
| Compiler turns source into binaries | Agent turns spec into a codebase |
| You review diffs of code | You review diffs of the spec, then the code it produced |
| Requirements drift from implementation | Implementation is regenerated from requirements |
| Linters and CI enforce standards | The **constitution** enforces standards, before code exists |

When a requirement changes, you do not open the editor and patch the code. You change the spec and re-run the cascade. Code becomes cheap and disposable; the spec becomes the thing you protect.

**The realistic version:** you will still read the code, still review the diffs, still own what ships. SDD does not remove engineering judgement, it moves it earlier, from "reviewing 800 lines of generated code" to "reviewing 40 lines of requirements." That is a much better place to spend your attention.

---

## 🗣 How we prompt in this workshop

There is no separate interview step. Spec Kit already has a command that interrogates you, `/speckit-clarify`, so we use it instead of improvising our own. Three kinds of prompt:

**Seed, then clarify.** For specifications. A short `/speckit-specify` (a few sentences of what and why), then `/speckit-clarify` repeatedly until nothing is ambiguous.

**Decide, then command.** For the constitution, the plan and constitution amendments. These record decisions the team has already made, so the command carries them directly, in one short paragraph.

**Cascade.** `/speckit-tasks`, `/speckit-analyze` and `/speckit-implement`, with no arguments or one narrow scope.

*Don't append "ask me questions" to a slash command. The command template already instructs the agent to produce an artifact, so asked to do both, it generates.*

### The one catch

Short prompts move the work into your **answers**. If you reply "whatever you think" to every question, you get a specification full of the agent's assumptions with your name on it. The questions are where you're supposed to make decisions.

Every module includes an **answer key**, the choices we're making and why, so nobody stalls, and so the whole room ends up roughly aligned.

---

## The workflow you'll learn

```
specify init                 →  scaffold the toolkit
  /speckit-constitution      →  non-negotiable project principles      (once, then amended)
    /speckit-specify         →  WHAT and WHY                           (no tech stack here)
    /speckit-clarify         →  agent interrogates your ambiguity
    /speckit-plan            →  HOW: stack, schema, routes, contracts
    /speckit-tasks           →  dependency-ordered, executable task list
    /speckit-analyze         →  read-only consistency gate across artifacts
    /speckit-implement       →  agent writes tests and code
    /speckit-converge        →  (capstone only) verify the codebase satisfies the spec
```

Steps 1–2 and 4 are where humans do real work. The rest is where the agent earns its keep.

---

## Modules

| # | Module | Time | Focus |
|---|---|---|---|
| 0 | [Setup & Orientation](modules/00-setup.md) | 15 min | Install, init, verify, tour the scaffold |
| 1 | [The Constitution & First Spec](modules/01-constitution-and-spec.md) | 25 min | Interview-first prompting; `/specify` + `/clarify` |
| 2 | [Plan → Tasks → Implement](modules/02-plan-tasks-implement.md) | 35 min | Full quality-gate cascade to a working MVP |
| — | **Break** | 10 min | |
| 3 | [Multi-Course Support](modules/03-multi-course.md) | 25 min | The mid-flight requirement change |
| 4 | [Secure CSV Export](modules/04-csv-export.md) | 25 min | Letting the agent sharpen a vague requirement |
| 5 | [Design System Constraints](modules/05-design-system.md) | 20 min | Constraining the agent via the constitution |
| 6 | [Capstone: Persistence & Analytics](modules/06-capstone.md) | 20 min | Authorizing a dependency; `/speckit-converge` |

**Supporting files**

- [`PROMPTS.md`](PROMPTS.md) — every prompt in the workshop, copy-paste ready, in order
---

## Prerequisites

Have these working **before** the workshop starts. Budget 10 minutes the day before.

- **Node.js 22+** and npm — `node --version`
- **Git** — `git --version`
- **[uv](https://docs.astral.sh/uv/)** — the Python package manager that ships the Spec Kit CLI
- **An AI coding agent CLI** — Claude Code, GitHub Copilot CLI, Gemini CLI, Cursor, Codex, or another supported integration, authenticated and working
- A terminal, an editor, and a browser

Full install commands are in [Module 0](modules/00-setup.md).

---

## A note on command syntax

Spec Kit commands are namespaced. Throughout this workshop they are written as `/speckit-specify`, `/speckit-plan`, and so on — the form used by Claude Code, Copilot, Cursor, and most integrations.

Some skills-based agents expose them differently:

| Agent | Form |
|---|---|
| Claude Code, Copilot, Cursor, Gemini | `/speckit-specify` |
| Codex, ZCode | `$speckit-specify` |
| Kimi | `/skill:speckit-specify` |

The steps are otherwise identical. Substitute the form your agent uses.

---

## Ground rules for the session

1. **Do not hand-edit application code.** If the output is wrong, the spec was wrong. Go fix the spec. This will feel unnatural for the first hour. Sit with it.
2. **Answer the agent's questions properly.** Short prompts only work if the dialogue does the work. "You decide" is a decision — make it deliberately, not by default.
3. **Read the generated artifacts.** `spec.md`, `plan.md`, `tasks.md` — actually open them. Skimming them is the single biggest predictor of whether this works for you.
4. **Your output will differ from your neighbour's.** Same seed prompt, different conversation, different code. That is expected and is one of the things worth discussing at the end.
5. **Commit after every module.** The git history is a teaching artifact in itself.
