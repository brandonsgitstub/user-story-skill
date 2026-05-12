# Story Map Format — Large Initiatives

Use this structure when the input is a full PRD or a complex initiative that spans multiple epics.

---

## Story Map Structure

```
Initiative: [Initiative name]
Total stories: [N]
Epics: [N]

── EP-01: [Epic name — user-facing capability group]
   ├── EP-01-01: [Child story]
   ├── EP-01-02: [Child story]
   └── EP-01-03: [Child story]

── EP-02: [Epic name]
   ├── EP-02-01: [Child story]
   └── EP-02-02: [Child story]

── EP-03: [Epic name]
   └── EP-03-01: [Child story]

Dependencies:
- EP-02-01 must ship before EP-01-02
- EP-03 is independent — can be built in parallel
```

---

## How to group stories into epics

Epics should map to a coherent user capability — something a user could describe as a feature in one sentence. They should not map to system layers (don't create an "API epic" and a "frontend epic" for the same feature — those are engineering concerns, not product epics).

**Good epic groupings:**
- "Notification delivery" — all stories related to receiving and managing notifications
- "Plan upgrade flow" — all stories a user experiences in the upgrade path
- "Admin seat management" — all stories an admin does to manage team members

**Bad epic groupings:**
- "Backend work" — not user-facing
- "Performance improvements" — not a capability
- "Misc features" — not coherent

---

## Epic template

```
## EP-XX — [Epic name]

**User capability:** [One sentence: what can the user do when this epic is complete?]
**Stories:** [N child stories]
**Estimated effort:** [Total person-months across child stories — rough]
**Dependency:** [What must be true before this epic can start?]
**Ships when:** [What's the minimum set of child stories for this epic to be releasable?]

Child stories: EP-XX-01 through EP-XX-0N
```

---

## Story map sizing guide

| Initiative size | Epics | Stories | Output format |
|----------------|-------|---------|---------------|
| Single feature | 1 | 2–4 | Epic + child stories inline |
| Feature group (PRD section) | 2–3 | 4–8 | Story map inline |
| Full initiative (full PRD) | 3–6 | 8–20 | Story map as .docx |
| Large platform initiative | 6+ | 20+ | Suggest splitting into phases first |

For initiatives with 20+ stories: before writing all stories, present the epic structure to the PM for approval first. Writing 20 stories only to restructure the epics wastes both parties' time.

---

## Dependency notation

Use a simple dependency table at the end of the story map:

| Story | Depends on | Reason |
|-------|-----------|--------|
| EP-01-02 | EP-02-01 | Requires notification service to be live |
| EP-03-01 | EP-01-01 | Requires auth middleware |
| EP-02 | — | Independent — can build in parallel |

Keep it short. Only flag hard blockers, not nice-to-have ordering preferences.
