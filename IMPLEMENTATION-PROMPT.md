# Credits MVP V1.1 — Implementation Prompt

Paste this into a new Claude Code session to implement the post-testing design changes.

---

You are implementing a series of design changes to a set of HTML prototype files. These are standalone, self-contained HTML files — no framework, no build step. All styling is inline `<style>` blocks using CSS custom properties from the GDS-E design system. Read each file before editing it.

## Working directory

`/Users/mayank.tyagi/Ticketmaster/Credits MVP — V1.1/`

Files in scope:
- `Credits — Landing.html` — the credit overview / landing screen
- `Credit Overview — Mastercard.html` — the credit record page (balance breakdown, history table, activity log)
- `Credits — Set up Credit.html` — the set-up and adjust credit drawer/form

If you need to understand the original structure or CSS patterns, read the V1 equivalents at:
`/Users/mayank.tyagi/Ticketmaster/Credits MVP/`
These are **read-only** — do not edit anything in that folder.

---

## Changes to implement

Work through these in order. The first three are blockers for the next testing round — prioritise them.

---

### 1 · Adjust drawer — clarify the delta field (BLOCKER)

**File**: `Credits — Set up Credit.html`

The "Change amount" input field in the Adjust Credit drawer is misread by users as "set a new total" rather than "enter a change value". Fix this:

- Rename the field label from "Change amount" to "Adjust by"
- Add a subtext hint directly below the field: "Enter a positive number to increase the balance, or a negative number to decrease it (e.g. +500 or −250)"
- If the drawer shows a current balance value above the field, add a "New balance" preview line below the input that updates as the user types. If the drawer is static HTML with no JS, add it as a static display showing a placeholder value and label it clearly as "New balance after adjustment"
- Do not change the field's visual size or the drawer's overall layout

---

### 2 · Reason field — required indicator and blocking validation (BLOCKER)

**File**: `Credits — Set up Credit.html`

The Reason field in both the Set up Credit and Adjust Credit forms appears optional but is mandatory. Fix this:

- Add an asterisk (*) to the Reason field label — e.g. "Reason *" — styled in `var(--color-critical)` (`#D93A3A`)
- If the drawer has a submit/save button, add a `required` attribute to the Reason `<textarea>` or `<input>`
- Add an inline error message element below the Reason field (initially hidden, or shown as a static example state): "A reason is required before saving." Style it in `var(--color-critical)`, `font-size: 12px`
- If there is a form submit handler in JS, add a guard that prevents submission and shows the error when Reason is empty

---

### 3 · RELEASED / REFUNDED vocabulary — inline explanations (BLOCKER)

**File**: `Credit Overview — Mastercard.html`

The status labels RELEASED and REFUNDED in the Credit history table are not understood by novice users. Fix this:

- Add a `title` tooltip attribute to each instance of RELEASED and REFUNDED status chips/badges:
  - RELEASED: `title="Funds released — available for the client to spend against tickets"`
  - REFUNDED: `title="Balance returned — credit refunded back to the account"`
- Additionally, if there is a legend, key, or any explanatory section near the Credit history table, add one-line definitions for both terms there
- Do not rename the status labels themselves — only add the supporting explanation

---

### 4 · Activity log — move out of the kebab menu

**File**: `Credit Overview — Mastercard.html`

The Activity log is currently only accessible via a kebab (⋯) overflow menu. It needs a primary entry point:

- Add an "Activity log" tab or section heading directly on the credit record page, below the Credit history table
- If the page uses a tab pattern, add "Activity log" as a tab alongside any existing tabs
- If the page is single-column with no tabs, add a section heading "Activity log" with the same visual treatment as other section headings, and render the log entries below it
- The kebab menu can retain a link to the activity log as a secondary shortcut, but should not be the only route
- If the activity log data is currently rendered somewhere on the page (even inside a hidden element), move it to the new visible section. If it is absent entirely, add a placeholder with 2–3 realistic seeded log entries in the same format as any existing log entries

---

### 5 · Notes field — update label and hint text

**File**: `Credits — Set up Credit.html`

The Notes field is being skipped by users who assume it is a system field. Fix this:

- Change the field label from "Notes" (or equivalent) to "Notes (optional)"
- Add a hint line below the field: "Visible to all operators in the activity log"
- Do not make the field larger or move it — only update the label and add the hint

---

### 6 · Credit history — add a running balance column

**File**: `Credit Overview — Mastercard.html`

Users expect to see the balance after each transaction, not just the transaction delta:

- Add a "Balance after" column to the Credit history table, to the right of the Amount/Delta column
- Populate it with plausible seeded values that are consistent with the running total (i.e. each row's "Balance after" = previous row's "Balance after" + this row's delta)
- Use the same column header style as the existing columns
- If the table is already at full width and adding a column would cause overflow, remove the least important existing column or reduce column widths — confirm with a comment in the HTML what was removed and why

---

### 7 · Reason field — remove minimum character length constraint

**File**: `Credits — Set up Credit.html`

If the Reason field has a `minlength` attribute or any JS validation enforcing a minimum character count, remove it. The field must remain required (see change 2) but should not restrict length beyond "not empty".

---

### 8 · Not-configured credit rows — add actionable treatment

**File**: `Credits — Landing.html`

Rows or cards where credit has not yet been configured should be visually distinct and clearly actionable:

- Identify any rows/cards in the landing list that represent clients with no credit configured
- Add a subtle "Set up credit →" text link or a dashed/muted border treatment to these rows to signal they are actionable starting points, not read-only empty states
- Do not change the layout of configured rows

---

### 9 · Seeded data — add a Held transaction

**File**: `Credit Overview — Mastercard.html`

No persona has seen the Held→Released state transition in the prototype. Add it to the seeded data:

- Add at least one transaction row to the Credit history table with a status of HELD (or equivalent)
- Style it consistently with the existing status chip/badge pattern — use `var(--color-fill-warning-light)` background and `var(--color-warning-text)` colour if no HELD style exists yet
- Add a "Release" action to this row (a button or link) so the transition can be triggered during a test session
- The released state should update the row's status chip to RELEASED

---

## What not to change

- The delta model for adjustments — this is confirmed correct; only the labelling needs improving (change 1)
- The balance breakdown hero card — confirmed correct by testing; do not move or restructure it
- The Credit history column set — confirmed correct; only add the running balance column (change 6)
- The overall visual design, colour tokens, typography, or component patterns — stay within the existing GDS-E system

---

## When done

Confirm which files were changed and summarise what was implemented vs skipped (with reason). If any change required a judgement call about placement or wording, note it so it can be reviewed.
