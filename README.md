# User Story Generator — Claude Skill

A Claude skill for turning any feature input — a one-liner, a PRD section, or a full spec — into dev-ready user stories with acceptance criteria. Output scales to input size: a single story for simple features, an epic with child stories for multi-part features, or a full story map for large initiatives. Every story includes Given/When/Then AC, edge cases, and explicit out-of-scope notes.

---

## What It Does

Accepts messy input. Produces stories engineers can pick up without a follow-up meeting.

**Output scales to what you give it:**

| Input | Output |
|---|---|
| One feature, simple scope | Single story with full AC |
| Feature with multiple distinct interactions | Epic + 2–4 child stories |
| PRD section covering multiple features | Epic per feature group + child stories |
| Full PRD or large initiative | Full story map with dependency notes |

---

## How to Use It

### 1. Add the skill to Claude

Upload `SKILL.md` to your Claude project.

### 2. What to tell Claude

You don't need a clean spec. The skill is designed to work with whatever you have.

**One-liner:**
```
Write a user story for adding CSV export to our reporting dashboard.
```

**PRD section:**
```
Here's the requirements section from our notifications PRD — 
can you turn this into stories? [paste PRD section]
```

**Brain dump:**
```
We need to let admins set per-user permission levels. 
Right now it's all-or-nothing. There are three roles: viewer, editor, admin. 
Some features should only be accessible by admins.
```

If your input is genuinely ambiguous, Claude asks one targeted clarifying question before writing — usually: *"Who is the primary user, and what's the one outcome they're trying to achieve?"*

---

## What You Get

Each story includes four parts:

**1. Story statement**
```
As a [persona], I want to [action], so that [real outcome].
```

The "So that" states a genuine outcome — not a restatement of the action.

**2. Acceptance criteria (Given/When/Then)**
```
Given [context]
When [action]
Then [observable, testable outcome]
```

Every AC is testable. QA can verify it without interpretation.

**3. Edge cases**
At minimum: empty/zero state, permission boundary (what if the user lacks access?), and the unhappy path (what if the action fails?). Additional edge cases based on the feature.

**4. Out of scope**
Explicit named constraints — things a reader might reasonably assume are included but aren't. Not fillers.

---

## Example Output

**Input:**
```
Add CSV export to the reporting dashboard. Users should be able to export 
any report they can view. Admins can export all reports.
```

**Output:**

---

**US-01 — Export report as CSV**

*As a reporting user, I want to export any report I have view access to as a CSV file, so that I can analyze the data in my own tools without manually copying it.*

**Acceptance Criteria**

```
Given I am viewing a report I have permission to access
When I click the "Export CSV" button
Then a CSV file downloads containing all rows currently visible in the report,
  with column headers matching the report column names

Given I am viewing a filtered report
When I export as CSV
Then the CSV contains only the filtered rows, not the full dataset

Given I do not have view access to a report
When I navigate to that report's URL directly
Then the export button is not visible and the report returns a 403

Given the report contains no data
When I click "Export CSV"
Then a CSV downloads with only the header row and a visible empty-state message appears before export
```

**Edge Cases**
- Report with 10,000+ rows: export should complete without timeout; consider async generation with download link for large datasets
- Report with special characters in column names or cell values: CSV must be properly escaped
- Admin exporting a report from another user's private workspace: confirm whether this is intended behavior `[open question]`

**Out of Scope**
- PDF export (separate initiative)
- Scheduling or automating exports
- Exporting dashboards (multi-report views)

---

**Story ID format:** `US-01`, `US-02` for standalone stories. `EP-01` for epics, `EP-01-01`, `EP-01-02` for child stories.

---

## ⚠️ Assumption Callouts

After delivering stories, the skill surfaces any time-sensitive values inherited from an upstream PRD that are now baked into acceptance criteria — things like "90-day transition window" or "2,500 contact limit." If those values change in the PRD, the affected stories need to be updated before handoff.

---

## File Structure

```
user-story-generator/
├── SKILL.md                        # The Claude skill — upload this to your project
└── references/
    ├── story-format.md             # Full story template with quality standards and examples
    ├── story-map-format.md         # How to structure a story map for large initiatives
    └── ac-patterns.md              # Given/When/Then patterns for auth, forms, limits, permissions, empty states
```

---

## Design Notes

The skill enforces a hard quality bar on two things most AI-generated stories get wrong:

1. **The "So that" clause.** It must state a real outcome, not restate the action. *"So that I can see the notifications"* is not an outcome. *"So that I can act on time-sensitive tasks without switching to a different tool"* is.

2. **Acceptance criteria must be testable.** Every Given/When/Then must be something a QA engineer can verify with a specific, observable result. Vague criteria like "the system responds quickly" or "the UI looks good" get rewritten.

Story sizing is also enforced. If a story spans multiple sprints, the skill splits it. Signals: changes to 3+ system areas, 6+ AC items, 2+ user types in one story, or the word "and" in the "I want" clause.
