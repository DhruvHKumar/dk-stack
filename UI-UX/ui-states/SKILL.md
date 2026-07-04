---
name: ui-states
description: >
  UI state completeness enforcer that ensures every interactive component
  handles all possible states — not just the happy path. Triggers when
  creating, reviewing, or modifying any interactive UI element: buttons, forms,
  inputs, cards, lists, tables, navigation, modals, notifications, or any
  component the user can interact with.
---

# UI States — Completeness Enforcer

You are a **UI State Enforcer**. The most common failure in AI-generated UI is
building only for the **happy path** — the ideal scenario where everything
works, data exists, and users behave perfectly. Real interfaces must handle
every state a component can be in. An unhandled state is a broken experience.

> A component is not done until every state is designed.

---

## Core Philosophy

Every interactive element exists on a **state spectrum**. Your job is to ensure
no state is left unconsidered. The user should never encounter a blank screen,
a broken layout, or an ambiguous interaction because a state wasn't handled.

---

## The Universal State Model

These states apply to **every** interactive component. Not all states are
relevant to every component — but every component must be evaluated against
this full list. If a state doesn't apply, consciously skip it. Never skip
by accident.

### 1. Default / Rest

The component's **neutral, idle** state before any interaction.

**Apply:**
- This is the baseline. It must clearly communicate what the element is and what it does.
- Visual affordances must be clear: buttons should look clickable, links should look tappable, inputs should look editable.
- Default state sets the expectations for all other states — design it first.

---

### 2. Hover

The cursor is **over** the element but hasn't clicked.

**Apply:**
- Hover must produce a **visible change** — color shift, shadow elevation, underline, cursor change, or subtle scale.
- The change should be **subtle but unmistakable**. Don't redesign the element on hover; hint at interactivity.
- Hover transitions should be fast (100–150ms) — they must feel responsive.
- **Mobile has no hover.** Don't rely on hover to convey critical information. Hover is an enhancement, not a requirement.
- Hover must not shift layout (no size changes that push other elements).

---

### 3. Focus

The element has received **keyboard focus** (Tab key navigation).

**Apply:**
- **Focus states are NOT optional.** They are a hard accessibility requirement (WCAG 2.4.7).
- Focus must be **highly visible** — a clear outline, ring, or border change. Default browser focus rings are acceptable; custom styles must be equally or more visible.
- Focus styles must have sufficient contrast against the background (at least 3:1 ratio for the focus indicator).
- Never use `outline: none` without providing a custom, equally visible focus indicator.
- Focus order must follow a logical reading sequence — don't rely on `tabindex` hacks.
- For focus-within (e.g., a card with a focused link inside), the container should show a subtle focus indication too.
- Consider `:focus-visible` to show focus rings only on keyboard navigation, not mouse clicks.

---

### 4. Active / Pressed

The element is being **clicked or tapped** (mousedown / touchstart).

**Apply:**
- Active state should feel **tactile** — like pressing a real button.
- Common patterns: slight scale reduction (0.97–0.98), darkened background color, inset shadow, or removing elevation.
- The change should be immediate (no transition delay on press).
- Active state duration is naturally brief — the user's finger releases quickly. Don't over-design it.
- For touch devices, consider a brief active-state hold (100ms) so users can see the feedback before transition.

---

### 5. Disabled

The element **exists but cannot be interacted with** right now.

**Apply:**
- Disabled elements must be **visually muted** — reduced opacity (0.4–0.6), desaturated colors, or grayed out.
- The cursor should change to `not-allowed` on hover.
- **Never use disabled as a mystery.** If something is disabled, users must understand *why*. Use:
  - Tooltips explaining the condition ("Complete all required fields to submit")
  - Inline text near the disabled element
  - Visual cues showing what needs to happen first
- Disabled elements should NOT receive focus (use `disabled` attribute, not `aria-disabled`, unless you need to provide context to screen readers).
- Don't disable navigation elements — hide them if they're not available, or show them with explanation.

---

### 6. Loading / Pending

The element has been **activated** and is **waiting for a response**.

**Apply:**
- The user must know their action was received. Replace or augment the element with a loading indicator:
  - **Buttons:** Replace label text with a spinner + "Saving..." or disable with spinner. Never leave the button looking clickable.
  - **Forms:** Disable all inputs during submission. Show inline progress.
  - **Data regions:** Show skeleton placeholders matching the expected layout shape.
  - **Actions:** Show optimistic UI where safe (e.g., like button fills immediately, reverts on failure).
- **Prevent double-submission.** Once an action is triggered, the trigger element must be locked.
- Loading states must appear within 400ms (Doherty Threshold).
- If loading exceeds 3–5 seconds, provide contextual messaging about what's happening.

---

### 7. Empty / Zero State

The component has **no data to display**.

**Apply:**
- Empty states are **not blank screens.** They are onboarding opportunities.
- Every empty state must include:
  1. **An explanation** of what will appear here ("No projects yet")
  2. **A clear action** to populate it ("Create your first project" with a CTA button)
  3. **Optionally, an illustration** or icon to soften the emptiness
- Empty states should feel inviting, not broken.
- Different empty states have different causes — distinguish between:
  - **First-time empty:** User hasn't created content yet → encourage action
  - **Search/filter empty:** No results match → suggest broadening criteria
  - **Error-caused empty:** Data failed to load → show error state, not empty state
  - **Cleared empty:** User deleted everything → confirm the action, offer undo
- Tables, lists, cards, dashboards, feeds — ALL need empty state handling.

---

### 8. Error State

Something **went wrong** — validation failed, network request failed, or an
unexpected condition occurred.

**Apply:**
- **Error states must be specific.** "Something went wrong" is a failure of the error state.
- **Form validation errors:**
  - Show errors inline, next to the offending field — not just at the top of the form.
  - Use color (red/danger) + icon + text. Never rely on color alone (color blindness).
  - Explain what's wrong *and* how to fix it ("Password must be at least 8 characters" not "Invalid password").
  - Validate on blur where possible, not only on submit.
  - Don't clear the user's input on error — let them correct it.
- **Network/system errors:**
  - Provide a retry action.
  - Distinguish between recoverable ("No internet — retry?") and non-recoverable ("This page doesn't exist") errors.
  - Show contextual errors in-place, not full-page redirects when possible.
- **Error states must never be dead ends.** Always offer a path forward: retry, go home, contact support, try again later.
- Preserve user input across error recovery whenever possible.

---

### 9. Success / Confirmation

The action **completed successfully**.

**Apply:**
- Success should feel rewarding — this is the **peak moment** (Peak-End Rule).
- Common patterns:
  - Green checkmark animation
  - Brief success banner/toast that auto-dismisses (5–8 seconds, or user-dismissable)
  - Confetti or subtle celebration for milestone achievements (use sparingly)
  - Inline confirmation ("Saved ✓") that fades after a few seconds
- **Don't over-celebrate routine actions.** Saving a form doesn't need confetti. Creating a first project might.
- Success states should naturally transition back to the default state or next logical step.
- For destructive actions completed successfully (delete, archive), include an **undo option** with a timer.

---

### 10. Selected / Active (Toggled)

The element is in an **"on" or selected state** — distinct from hover/focus.

**Apply:**
- Selected state must be visually distinct from default, hover, AND focus.
- Common patterns: filled vs outlined icons, background color change, checkmark overlay, bold text, active indicator (underline, sidebar marker).
- For multi-select scenarios, the selected count should be visible ("3 selected").
- Selected items should remain clearly identifiable even when the user's attention is elsewhere.
- Radio/toggle behavior: selecting one item should deselect others with a smooth transition.

---

### 11. Skeleton / Placeholder

The component's **structure before content has loaded**.

**Apply:**
- Skeleton screens should **match the shape and layout** of the actual content that will replace them.
- Use neutral, pulsing/shimmering placeholder blocks — not spinners.
- Text skeletons should approximate the expected text length and line count.
- Image skeletons should maintain the correct aspect ratio.
- Don't skeleton-ize the entire page — the layout shell (header, nav, sidebar) should render immediately. Only skeleton the dynamic content regions.
- Skeleton → content transition should be a smooth crossfade, not a jarring swap.

---

### 12. Overflow / Truncation

Content **exceeds the available space**.

**Apply:**
- Text overflow must be handled explicitly: truncate with ellipsis (`...`), or wrap with clear boundaries.
- Truncated text should reveal full content on hover (tooltip) or click (expand).
- Lists with too many items: show a count + "Show more" link, not an infinitely scrolling container by default.
- Tables with too many columns: horizontal scroll with a sticky first column, or responsive collapse on mobile.
- Never let content overflow and break layout silently. Define explicit overflow behavior for every container.

---

### 13. Read-Only vs Editable

When an element **looks like an input but can't be modified**.

**Apply:**
- Read-only fields must look visually distinct from editable inputs — remove borders, change background, or use plain text display.
- Don't use `disabled` styling for read-only content. Disabled implies "you can't use this yet"; read-only means "this is informational."
- Read-only values should still be selectable/copyable where useful (e.g., API keys, generated URLs).
- When a form switches from read-only to editable (edit mode toggle), the transition should be clear and animated.

---

### 14. Drag State

When an element is being **dragged** (if applicable to the component).

**Apply:**
- The dragged element should have elevated visual treatment (shadow, opacity change, slight scale increase).
- The **drag origin** should show a ghost or placeholder indicating where the element came from.
- **Drop targets** must be clearly highlighted when a drag is active.
- Invalid drop zones should have a visual "not allowed" indicator.
- Provide a clear cancel mechanism (Escape key, releasing outside valid zones).
- On mobile, long-press to initiate drag with haptic feedback if available.

---

## Component State Matrix

Use this as a quick-reference. For each component you build, verify every
applicable state:

| State | Button | Input | Card | List Item | Modal | Nav Item | Toast |
|---|---|---|---|---|---|---|---|
| Default | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Hover | ✅ | ✅ | ✅ | ✅ | — | ✅ | ✅ |
| Focus | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Active | ✅ | — | ✅ | ✅ | — | ✅ | — |
| Disabled | ✅ | ✅ | — | — | — | ✅ | — |
| Loading | ✅ | ✅ | ✅ | ✅ | ✅ | — | — |
| Empty | — | ✅* | ✅ | ✅ | — | — | — |
| Error | ✅ | ✅ | ✅ | ✅ | ✅ | — | ✅ |
| Success | ✅ | ✅ | — | — | ✅ | — | ✅ |
| Selected | ✅** | — | ✅ | ✅ | — | ✅ | — |
| Skeleton | — | — | ✅ | ✅ | ✅ | — | — |
| Overflow | — | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| Read-only | — | ✅ | ✅ | ✅ | — | — | — |

*\* Input empty = placeholder text.  \*\* Toggle buttons.*

---

## Review Checklist

After building any interactive component, verify:

1. **Default** — Is it clear what this element is and does?
2. **Hover** — Does it respond to cursor presence? (Desktop only)
3. **Focus** — Can it be reached and is it visible via keyboard?
4. **Active** — Does it feel tactile when pressed?
5. **Disabled** — Is the reason for disabling communicated?
6. **Loading** — Does the user know their action was received?
7. **Empty** — What does the user see when there's no data?
8. **Error** — Is the error specific, helpful, and recoverable?
9. **Success** — Does completion feel rewarding and clear?
10. **Selected** — Is the active/toggled state distinct from all others?
11. **Skeleton** — Does the placeholder match the real content shape?
12. **Overflow** — What happens when content exceeds its bounds?
13. **Read-only** — Can the user tell the difference from editable?
14. **Drag** — (If applicable) Is the drag interaction fully handled?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| No focus styles (`outline: none` with no replacement) | Keyboard users can't navigate | Always provide visible focus indicators |
| Empty container with no messaging | User thinks the page is broken | Add explanation + CTA for empty states |
| Generic "Something went wrong" errors | User can't diagnose or recover | Specific message + recovery action |
| Disabled button with no explanation | User doesn't know what to do | Tooltip or inline text explaining the condition |
| Button still clickable during loading | Causes double submissions | Disable + show spinner immediately on click |
| Same visual style for disabled and read-only | Confused semantics | Distinct styles: disabled = muted; read-only = informational |
| Content silently overflowing containers | Broken layouts, hidden information | Explicit truncation, scroll, or expand patterns |
| Success state that disappears too fast | User misses confirmation | Toast for 5–8s with manual dismiss option |
| Error clears user's input | Infuriating — forces re-entry | Preserve all input, highlight only the error |
| Hover-only information with no mobile fallback | Invisible on touch devices | Long-press, tap-to-reveal, or always-visible alternatives |
