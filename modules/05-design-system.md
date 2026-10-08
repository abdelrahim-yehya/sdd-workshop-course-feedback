# Module 5 — Design System Constraints (Amending the Constitution)

**⏱ 20 minutes** · [← Module 4](04-csv-export.md) · [Next: Module 6 →](06-capstone.md)

---

## Goal

Make the UI genuinely usable while preventing the agent from reaching for a framework and learn where a durable constraint belongs.

---

## 5.1 — The teaching point: constitution vs. spec

Left alone, an agent asked to "make this look modern" will often pull in Tailwind, Bootstrap, or a component library. Not out of malice, those are statistically what "modern UI" looks like in its training data.

The naive fix is to add "no frameworks" to the feature spec. **Wrong place.** Ask which is true:

- *"This feature's UI uses no CSS framework"* → a property of one feature
- *"This project uses no CSS framework, ever"* → a property of the project

The second is **constitutional**. Put it in a spec and you must restate it in every future spec, forever; the first time you forget, the constraint evaporates. Put it in the constitution and it's checked at every plan, for every feature, without you remembering anything.

---

## 5.2 — Amend the constitution

Interview first, again because "modern and polished" is exactly the kind of phrase that means nothing until someone makes you define it.

### 👉 Prompt 1

```text
/speckit-constitution Add a new design principle and keep every existing principle intact, bumping the document version. The UI must look modern and polished using only hand-written CSS.
Rules: (1) all colours and spacing are defined once as CSS custom properties on :root, with an 8px spacing base and no literal colour or spacing value anywhere else; (2) cards, inputs and buttons have an 8px radius, with layered subtle shadows instead of heavy borders; (3) every interactive element defines hover, focus-visible, active and disabled states, and focus indicators are always clearly visible and never removed; (4) text contrast meets WCAG AA; (5) layouts use Grid and Flexbox and work down to 360px wide with no horizontal scroll; (6) forbidden: CSS frameworks, resets, component libraries, icon fonts or packages, third-party web fonts, preprocessors and any build step; allowed: system font stacks and inline SVG.
```
---

## 5.3 — Specify the visual work

### 👉 Prompt 2

```text
/speckit-specify Make the app look and feel modern. No behaviour changes at all, same routes, same validation, same auth. Star rating instead of a number input. Feedback shown as cards with the course as a badge. Summary figures at the top of the dashboard. And every state properly designed: loading, empty, and error.
```

### 👉 Prompt 3

```text
/speckit-clarify
```

**Answer key:**

| If it asks about… | We're going with |
|---|---|
| Star rating interaction | Hover previews, click selects, selection visually obvious. Must work by keyboard. |
| Comment field | Live character count that changes appearance as it nears and exceeds 1000. |
| Submit button | Disabled until the form is valid. |
| After submitting | Confirmation that appears then fades; form resets. |
| Timestamps on cards | Human-readable and relative ("2 hours ago"). |
| Summary figures | Total count and average rating, for the current filter. |
| Loading state | Shown while feedback is being fetched. |
| Error state | Visible message region, not console-only. |

---

## 5.4 — Cascade

### 👉 Prompt 4

```text
/speckit-plan Restyle the existing frontend only, server code and API contracts unchanged. Rebuild the stylesheet around a :root custom property block. Build the star rating from accessible radio inputs styled with CSS, so keyboard operation and screen-reader semantics come for free instead of being rebuilt in JavaScript. Existing tests must keep passing unmodified.
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

---

## 5.5 — Verify the constraint held

This is the real assessment of the module.

```bash
# Should list only your original dependencies
cat package.json

# Should return nothing
grep -rn "cdn\|unpkg\|jsdelivr\|googleapis\|tailwind\|bootstrap" public/ src/

# Should show a :root custom property block
head -40 public/*.css
```

Then in the browser: tab through the whole student form using only the keyboard. Can you set a rating? Is focus always visible? Resize to phone width, anything overflow?

**If the agent smuggled in a framework anyway,** that's a valuable moment. Don't rip it out by hand:

```text
This violates our constitution — you added [name it]. Remove it and rebuild that styling with hand-written CSS using the custom properties. Then tell me which artifact let this through: was the constitution ambiguous, or did the plan skip its constitution check?
```

The follow-up question is the important half. Violated constraints usually reveal a wording problem upstream, and the fix belongs there.

---

## 5.6 — Commit

### 👉 Prompt 5

```text
/speckit-git-commit
```

---

## ✅ Checkpoint

- [ ] `package.json` has no new dependencies
- [ ] No external CDN references in `public/` or `src/`
- [ ] Stylesheet driven by `:root` custom properties
- [ ] Star rating fully keyboard-operable with visible focus
- [ ] All previous tests still pass
- [ ] Fourth semantic commit exists

---

[Next: Module 6 — Capstone →](06-capstone.md)
