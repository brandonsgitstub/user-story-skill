---
name: user-story-generator
description: "Turn any feature description, PRD, or rough idea into dev-ready user stories with acceptance criteria. Use this skill whenever a PM asks to write user stories, generate acceptance criteria, break a feature into stories, create a story map, or produce tickets ready for engineering handoff. Trigger even if the input is messy — a brain dump, a pasted PRD section, a one-liner feature idea, or a full spec all work. Produces either a single focused story or a full story map (epic + child stories) depending on input complexity. Each story includes: the standard As a / I want / So that format, Given/When/Then acceptance criteria, edge cases and error states, and explicit out-of-scope notes. Designed for PMs at growth-stage SaaS companies who need stories that engineers can act on without a follow-up meeting."
---

# User Story Generator

A skill for turning any feature input — clean or messy — into dev-ready user stories that engineers can act on immediately.

**Output adapts to input size:**
- Small input (single feature, one-liner, rough idea) → **Single story** with full detail
- Medium input (PRD section, multi-part feature) → **Epic + 3–8 child stories**
- Large input (full PRD, complex initiative) → **Full story map** with epics, child stories, and a dependency note

---

## Phase 1: Read and Classify the Input

Before writing anything, read the input carefully and determine:

**1. Input quality — what do you have?**
- **Well-defined:** Clear feature description, known user, stated goal → proceed directly to writing
- **Partially defined:** Feature is clear but persona or goal is vague → make reasonable assumptions and flag them
- **Vague / brain dump:** Unclear what specifically is being built → ask one targeted clarifying question before proceeding

If you need to ask, keep it to a single question. The most valuable clarifying question is almost always: *"Who is the primary user of this feature, and what's the one outcome they're trying to achieve?"*

**2. Input size — single story or story map?**

| Input | Output |
|-------|--------|
| One feature, simple scope | Single story |
| One feature with multiple distinct user interactions | Epic + 2–4 child stories |
| PRD section covering multiple features | Epic per feature group + child stories |
| Full PRD or large initiative | Full story map — see `references/story-map-format.md` |

When in doubt, err toward more stories rather than fewer. A story that tries to cover too much is harder to estimate and harder to test than two focused stories.

**3. Time-sensitive assumption scan**
Before writing, scan the input for any specific numbers, dates, durations, or thresholds that have been inherited from the upstream doc — not decided by the stories themselves. These are assumptions that could silently invalidate multiple stories if they change.

Look for:
- Specific durations (e.g. "90-day transition window", "30-day warning")
- Hard thresholds (e.g. "80% usage warning", "2,500 contact limit")
- Specific dates or deadlines embedded in requirements
- Numeric limits that appear to be product decisions rather than engineering constraints

If any are found, collect them and surface a single callout block **after delivering the stories** (not before — don't interrupt the writing flow):

> ⚠️ **Time-sensitive assumptions inherited from this PRD:**
> - [Assumption 1 — e.g. "90-day grandfather window (EP-05-01 through EP-05-03)"]
> - [Assumption 2 — e.g. "80% threshold triggers usage warning (EP-03-01)"]
>
> These values are baked into the acceptance criteria above. If they change in the PRD, the affected stories will need to be updated before engineering handoff.

The goal is not to question these decisions — just to make the PM aware that the stories are coupled to them.

**4. Persona extraction**
If the input names a specific user type, use it. If not, infer the most likely persona from context and note it as assumed. Growth-stage SaaS defaults, if context is unclear:
- Feature in a core workflow → "a [role] user" (e.g., "a project manager", "a team admin")
- Feature in onboarding → "a new user"
- Feature in settings / admin → "an account admin"
- Feature in reporting → "a reporting user" or "a team lead"

---

## Phase 2: Write the Stories

Write each story using the full four-part format. Read `references/story-format.md` for the exact template and quality standards before writing.

**Core quality rules:**
- The "So that" must state a real outcome — not just restate the action. Bad: *"so that I can see the notifications."* Good: *"so that I can act on time-sensitive tasks without switching to a different tool."*
- Acceptance criteria must be testable. Every Given/When/Then must be something a QA engineer can verify with a specific, observable outcome. Avoid vague criteria like "the system responds quickly" or "the UI looks good."
- Edge cases are not optional. At minimum, include: empty/zero state, permission boundary (what happens if the user lacks access), and the unhappy path (what happens when the action fails).
- Out of scope notes are explicit constraints, not fillers. Each one should name something a reader might reasonably assume is included but isn't.
- Story size: each story should be completable in one sprint (roughly 1–5 days of eng work). If a story feels like it spans multiple sprints, split it.

**Story sizing signals — when to split:**
- The story requires changes to 3+ distinct system areas
- The acceptance criteria list exceeds 6 items
- There are 2+ distinct user types in the same story
- The story contains the word "and" in the "I want" clause

---

## Phase 3: Output Format

**Default output: inline in the conversation** — formatted cleanly with headers and code blocks for AC. This is fastest for the PM to review.

**If the PM asks for a file** (or if the story map has 6+ stories): generate a `.docx` using the docx skill. Read `/mnt/skills/public/docx/SKILL.md` before generating. Save to `/mnt/user-data/outputs/` and present with `present_files`.

**Story ID format:** Use sequential IDs within the session: `US-01`, `US-02`, etc. For epics: `EP-01`, `EP-02`. Child stories under an epic: `EP-01-01`, `EP-01-02`.

---

## Phase 4: Review Loop

After delivering stories:
1. Ask: *"Do these feel right for your team's definition of ready? Anything to split, merge, or reframe?"*
2. If the PM flags a story as too large → split it and re-output the child stories
3. If the PM flags a story as too small → offer to merge with an adjacent story or note it as a sub-task instead
4. If acceptance criteria feel off → ask what the QA test would actually look like and rewrite from that angle

---

## Reference Files

- `references/story-format.md` — Full story template with quality standards and examples
- `references/story-map-format.md` — How to structure a full story map for large initiatives
- `references/ac-patterns.md` — Common Given/When/Then patterns for recurring scenario types (auth, forms, limits, permissions, empty states)
