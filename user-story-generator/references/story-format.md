# Story Format — Template & Quality Standards

## Single Story Template

```
## [US-XX] — [Short title: verb + noun, e.g. "Create task via mobile quick capture"]

**Epic:** [Parent epic name, if applicable — otherwise omit]
**Persona:** [Who this is for — be specific]
**Priority:** [Must-have / Should-have / Nice-to-have — optional, set by PM]
**Size estimate:** [S / M / L — optional, set by eng]

---

### Story
As a [specific persona],
I want to [specific action or capability],
So that [real outcome or value — not a restatement of the action].

---

### Acceptance Criteria

**Scenario 1: [Happy path — primary success case]**
- Given [the user's starting state or context]
- When [the specific action they take]
- Then [the observable, testable outcome]

**Scenario 2: [Secondary success case or variant, if applicable]**
- Given ...
- When ...
- Then ...

**Scenario 3: [Permission / access boundary]**
- Given [user who lacks required access or role]
- When [they attempt the action]
- Then [system response — error, redirect, or hidden affordance]

**Scenario 4: [Unhappy path — action fails or errors]**
- Given ...
- When [action fails — network error, validation failure, system error]
- Then [graceful error handling — what the user sees and what they can do next]

**Scenario 5: [Empty / zero state]**
- Given [user reaches the feature with no existing data]
- When [they view the surface]
- Then [appropriate empty state is shown — not a blank screen]

---

### Edge Cases & Error States

- [Specific edge case 1 — e.g. "User submits form with special characters in name field"]
- [Specific edge case 2 — e.g. "User loses network connection mid-upload"]
- [Specific edge case 3 — e.g. "Two users edit the same record simultaneously"]
- [Error state — e.g. "API returns 500 — user sees a non-technical error message with retry option"]
- [Limit boundary — e.g. "User on Starter plan attempts to exceed 2,500 contact limit"]

---

### Out of Scope (this story)

- We are NOT [thing a reader might reasonably expect to be included]
- We are NOT [related feature being deferred]
- We are NOT [edge case being explicitly punted]

---

### Dependencies & Notes

- [Dependency on another story, system, or team — e.g. "Requires US-03 (auth middleware) to be complete"]
- [Design note — e.g. "Figma mock: [link]"]
- [Open question — e.g. "Eng to confirm: does current event bus support this trigger?"]
```

---

## Quality Standards

### The "So that" test
Read the "So that" clause in isolation. Ask: does this describe something a user actually cares about, or does it just restate what they did?

❌ **Bad:** "As a project manager, I want to receive task notifications, so that I receive task notifications."
✅ **Good:** "As a project manager, I want to receive task notifications inside the app, so that I stay on top of blockers without switching between tools."

The "so that" should answer: *what changes for the user if this feature exists?*

### The testability test
Read each acceptance criterion aloud. Can a QA engineer write a specific test for it with a pass/fail outcome?

❌ **Not testable:** "The system should respond in a reasonable time."
✅ **Testable:** "The notification renders within 200ms of the triggering event."

❌ **Not testable:** "The UI looks clean and professional."
✅ **Testable:** "The notification banner appears at the top of the screen below the nav bar, does not overlap content, and dismisses on tap."

### The scope test
Can one engineer reasonably complete this story in a single sprint (1–5 days of work)?

If a story contains any of these signals, split it:
- "and" in the "I want" clause → probably two stories
- More than 6 acceptance criteria → too much scope
- Multiple distinct user types → separate stories per persona
- Changes to 3+ distinct system areas → split by system boundary

### Edge case minimum bar
Every story must include at minimum:
1. The empty/zero state (what does the user see with no data?)
2. The permission boundary (what happens if the user lacks access?)
3. The failure/error path (what happens when something goes wrong?)

These are the three most commonly skipped cases that cause eng to come back with questions mid-sprint.

---

## Examples

### Example: well-written story

```
## US-04 — Receive in-app notification on task assignment

**Persona:** Project team member (assignee)

### Story
As a project team member,
I want to see an in-app notification when I'm assigned to a task,
So that I can act on new assignments immediately without checking my email or missing the update.

### Acceptance Criteria

**Scenario 1: Task assigned while user is in the app**
- Given I am logged in and actively using the app
- When another user assigns a task to me
- Then a banner notification appears at the top of my screen within 2 seconds
- And the banner shows the task name, assigner name, and due date
- And tapping the banner navigates me to the task detail view

**Scenario 2: Notification dismissed**
- Given a banner notification is visible
- When I tap the dismiss icon
- Then the banner disappears
- And the notification is marked as read in my notification history

**Scenario 3: User has disabled in-app notifications**
- Given I have turned off in-app notifications in my account settings
- When I am assigned a task
- Then no banner appears
- And no notification is added to my notification history

**Scenario 4: Assignment fails (network error)**
- Given another user attempts to assign a task to me
- When the API call fails
- Then no notification is shown to me
- And the assigner sees an error message with a retry option

**Scenario 5: No prior notifications (first-time state)**
- Given I have never received a notification
- When I open the notifications panel
- Then I see an empty state with the message "No notifications yet — assignments and updates will appear here"

### Edge Cases & Error States
- User is assigned to a task they already own — no duplicate notification
- User is assigned to a task in a project they've been removed from — notification shows but task detail shows access error
- 10+ notifications arrive in rapid succession — show most recent 3 in banner queue, batch the rest
- App is in background on mobile — notification deferred until next foreground session

### Out of Scope (this story)
- NOT building push notifications (separate initiative)
- NOT building notification preferences or per-type on/off controls (v2)
- NOT covering @ mentions or comment notifications (separate story)

### Dependencies & Notes
- Requires EP-02-01 (notification service infrastructure) to be complete first
- Design: banner component spec in Figma [link TBD]
```
