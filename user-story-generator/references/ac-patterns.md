# Acceptance Criteria Patterns

Common Given/When/Then patterns for scenario types that appear repeatedly across SaaS products. Use these as starting points — always adapt to the specific feature context.

---

## Authentication & Access

**Login success**
- Given I am a registered user on the login page
- When I enter valid credentials and submit
- Then I am redirected to my dashboard and my session is authenticated

**Login failure**
- Given I am on the login page
- When I enter an incorrect password
- Then I see the message "Incorrect email or password" — no indication of which field is wrong
- And my account is not locked after fewer than 5 attempts

**Insufficient permissions**
- Given I am logged in as a [role] without [permission]
- When I attempt to access [restricted feature or page]
- Then I see a [permission denied message / redirect to a permitted page]
- And no sensitive data is exposed in the URL or response

**Session expiry**
- Given my session has expired
- When I attempt any authenticated action
- Then I am redirected to the login page with a "Session expired — please log in again" message
- And my intended destination is preserved so I return there after login

---

## Forms & Input Validation

**Valid submission**
- Given I have filled all required fields with valid data
- When I submit the form
- Then the form submits successfully and I see a [confirmation message / redirect]

**Required field missing**
- Given I have left one or more required fields empty
- When I attempt to submit
- Then the form does not submit
- And each empty required field is highlighted with an inline error message
- And focus moves to the first invalid field

**Field format validation**
- Given I enter [invalid format — e.g. "not an email", "text in a number field"]
- When I move focus away from the field or submit
- Then I see an inline error: "[field] must be [format description]"

**Character limit**
- Given I type beyond the [N]-character limit for a field
- When I reach the limit
- Then additional characters are not accepted
- And a character counter shows "[N]/[limit]" near the field

**Duplicate entry**
- Given [an item with this name / email / identifier] already exists
- When I submit a form creating a new item with the same value
- Then the form does not submit
- And I see an inline error: "[Item] already exists — please use a different [field name]"

---

## Usage Limits & Plan Gates

**Approaching limit (warning state)**
- Given I am on the [plan] and have used [80%] of my [resource] allowance
- When I view [the relevant surface]
- Then I see a warning banner: "You've used [N] of [limit] [resources]. Upgrade to Pro for unlimited access."

**At limit (hard block)**
- Given I have reached my [resource] limit
- When I attempt to create a new [resource]
- Then the creation is blocked
- And I see a modal explaining the limit and a primary CTA to upgrade

**Gated feature (Starter user)**
- Given I am on the Starter plan
- When I attempt to access [Pro-only feature]
- Then I see a contextual upgrade prompt naming the feature and the plan required
- And I am NOT shown a generic error or blank state

**Post-upgrade access**
- Given I have just upgraded from Starter to Pro
- When I attempt to access a previously gated feature
- Then access is granted immediately without requiring a logout/login

---

## Empty & Zero States

**First-time empty state**
- Given I am a new user who has not yet created any [items]
- When I navigate to the [items list / dashboard]
- Then I see an empty state with a clear headline, a brief explanation of what [items] are, and a primary CTA to create the first one
- And I do NOT see a blank page or an error

**Post-deletion empty state**
- Given I have deleted all [items]
- When the last item is removed
- Then the empty state appears immediately
- And no ghost rows or layout artifacts remain

**No search results**
- Given I have typed a query that matches no results
- When the search completes
- Then I see "No results for '[query]'" with a suggestion to try different terms
- And the previous results are cleared

---

## Async Actions & Loading States

**Loading state**
- Given I have triggered an action that requires a server request
- When the request is in flight
- Then I see a loading indicator appropriate to the context (spinner, skeleton screen, or progress bar)
- And interactive elements that depend on the result are disabled

**Success after async action**
- Given I have triggered an async action (e.g. file upload, data export)
- When the action completes successfully
- Then I see a success message or state change
- And the loading indicator is removed

**Failure after async action**
- Given I have triggered an async action
- When the action fails (network error, timeout, server error)
- Then I see a non-technical error message
- And I am offered a clear recovery path (retry, contact support, or return to previous state)
- And my data is not lost or corrupted

---

## Destructive Actions

**Confirmation before delete**
- Given I attempt to delete a [item]
- When I click the delete action
- Then I see a confirmation dialog naming the item and warning that this cannot be undone
- And the action requires an explicit confirm click — it does NOT trigger on the first click

**Successful delete**
- Given I have confirmed deletion of a [item]
- When the deletion completes
- Then the item is removed from the list immediately
- And I see a brief success toast: "[Item] deleted"
- And an undo option is available for [N] seconds if technically feasible

**Delete blocked by dependency**
- Given the [item] I am trying to delete has [dependent items / active references]
- When I attempt to delete
- Then the deletion is blocked
- And I see a specific message explaining what depends on it and what I need to do first

---

## Notifications & Real-time Updates

**In-app notification delivery**
- Given a triggering event occurs (e.g. task assigned, comment added)
- When the event fires
- Then a notification appears within [N] seconds for the recipient
- And the notification includes the relevant context (who, what, when)

**Notification dismissed**
- Given a notification is visible
- When I dismiss it
- Then it disappears immediately
- And it is marked as read and does not reappear in the current session

**Notification cap**
- Given [N] notifications arrive in quick succession
- When they are delivered
- Then no more than [cap] are shown simultaneously
- And remaining notifications are accessible via the notification panel
