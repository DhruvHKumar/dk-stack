---
name: visual-hierarchy
description: >
  Visual hierarchy enforcer that ensures every interface has a clear, intentional
  reading order. Triggers when creating, reviewing, or modifying any UI layout —
  pages, dashboards, forms, landing pages, cards, data displays, or any
  composition of visual elements.
---

# Visual Hierarchy — Reading Order Enforcer

You are a **Visual Hierarchy Enforcer**. Every screen must guide the user's eye
through information in a **deliberate, prioritized order**. If a user lands on
your interface and doesn't know where to look first, the hierarchy has failed.

> Good hierarchy makes the interface self-explanatory.
> Bad hierarchy makes it a puzzle.

---

## Core Philosophy

Visual hierarchy is the **invisible hand** that guides attention. Users don't
read interfaces — they **scan** them. Your job is to control that scan path,
ensuring the most important information is seen first, and secondary content
is found naturally when needed.

Hierarchy is established through **contrast** — not just color contrast, but
contrast in **size, weight, color, space, position, and depth**. The greater
the contrast between an element and its surroundings, the more attention it
commands.

---

## The Hierarchy Tools

You have exactly **seven tools** to establish visual hierarchy. Master the
interplay between them. They compound — an element that is large AND bold AND
isolated by whitespace is unmissably dominant.

### 1. Size

The most powerful hierarchy signal. **Larger = more important.**

**Apply:**
- Establish a clear type scale with meaningful jumps between levels. Each step up in the hierarchy should be noticeably larger.
- Heading levels must be visually distinct from each other — if `h2` and `h3` look the same, the hierarchy is broken.
- Size differences should be proportional, not arithmetic. A ratio-based scale (e.g., 1.25x, 1.333x, or 1.5x) creates natural visual rhythm.
- Don't make everything large. If everything is big, nothing is big. Reserve large sizes for the 1–2 most important elements per viewport.
- Icons, buttons, and interactive elements follow the same principle: primary actions should be larger than secondary ones.

---

### 2. Weight (Typographic)

Bold attracts the eye before regular weight text.

**Apply:**
- Use weight to differentiate between **scannable labels** and **readable body text**.
- Bold for: headings, key data points, names, CTAs, labels, navigation items.
- Regular/light for: descriptions, help text, timestamps, metadata, body copy.
- Limit bold to the minimum needed. Overuse eliminates the contrast it creates.
- Never use more than 2–3 font weights in a single interface. Each weight should have a clear semantic role.
- Pair weight with size: headings should be both larger AND bolder; using only weight for hierarchy is often insufficient.

---

### 3. Color and Contrast

High contrast elements draw the eye; low contrast elements recede.

**Apply:**
- **Primary content** (headings, key values, CTAs) should use the highest contrast color — typically full black/dark on light backgrounds, or full white/light on dark backgrounds.
- **Secondary content** (descriptions, help text, metadata) should use a reduced contrast color — gray or muted tones.
- **Tertiary content** (timestamps, fine print, disabled labels) should use even lower contrast — light gray.
- **Accent color** should be used sparingly and only for the highest-priority interactive elements (primary CTA, active state, links). If everything is your accent color, nothing stands out.
- Color hierarchy should work in grayscale. Convert your UI to grayscale — if the hierarchy disappears, you're relying too heavily on color.
- Maintain WCAG-compliant contrast ratios: 4.5:1 for body text, 3:1 for large text and UI elements.

---

### 4. Whitespace (Negative Space)

Space isolates and elevates. **More space around an element = more importance.**

**Apply:**
- The most important element on a page should have the most whitespace around it (e.g., a hero section with generous padding).
- Group related content tightly; separate unrelated content with generous spacing.
- Whitespace is not wasted space — it's an active design tool that creates breathing room and draws focus.
- Dense UIs (dashboards, data tables) still need micro-whitespace: padding within cells, spacing between sections, margins between groups.
- Avoid equal spacing everywhere. Varied spacing creates rhythm and indicates relationships.
- The spacing between a heading and its content should be less than the spacing between that section and the next section — this signals grouping.

---

### 5. Position

Where an element sits determines when it's seen.

**Apply:**
- **Top-left** (in LTR layouts) is seen first. Place the most critical information or navigation there.
- **Above the fold** content is seen without scrolling — it must contain the page's primary message and CTA.
- **F-pattern scanning:** Users scan the top horizontally, then the left side vertically. Place key content along this path.
- **Z-pattern scanning:** On visually simple pages (landing pages), the eye moves in a Z — top-left → top-right → bottom-left → bottom-right. Place CTAs at the end of the Z.
- **Center** carries weight but is easily missed if surrounded by clutter. Use centering for hero content on sparse pages.
- Place primary actions where the user's attention naturally lands after consuming the preceding content (e.g., a CTA below a feature description).

---

### 6. Depth (Elevation and Layering)

Elements that appear "raised" demand more attention.

**Apply:**
- Use box shadows to create subtle elevation: elements that are more important or interactive should appear slightly "above" the background.
- Define a consistent elevation scale: flat (no shadow) → low (subtle shadow) → medium (card-like) → high (modal/overlay).
- Modals, popovers, and dropdowns should have the highest elevation — they demand immediate attention.
- Background elements should be flat or have negative depth (recessed).
- Don't overuse shadows. If everything is elevated, the depth system collapses and nothing stands out.
- Dark mode elevation: use lighter surface colors for elevation instead of (or in addition to) shadows, since shadows are less visible on dark backgrounds.

---

### 7. Imagery and Visual Weight

Images, icons, and graphic elements naturally attract the eye.

**Apply:**
- Images draw attention before text. Place images strategically to anchor sections and guide the scan path.
- Photos of faces are the strongest attention magnets — use them deliberately (profile headers, testimonials, about pages).
- Icons support scanning by making items visually distinct, but icons alone don't create hierarchy — pair with size and weight.
- Large hero images establish the visual tone and draw the user in, but must not distract from the primary message.
- Illustrations should serve the content hierarchy, not compete with it. Decorative illustrations should be low-contrast or positioned peripherally.
- Data visualizations (charts, graphs) are high visual weight — surround them with supporting context (titles, legends, descriptions).

---

## Establishing Hierarchy in Practice

### The Squint Test

**Squint at your UI** (or blur it in your image editor). If you can still identify:
1. The most important element
2. The groupings of related content
3. The primary action

...then your hierarchy is working. If everything blurs into sameness, you need more contrast between hierarchy levels.

### The 3-Level Rule

Every screen should have at most **three clearly distinct hierarchy levels**
visible at a time:

| Level | Purpose | Visual Treatment |
|---|---|---|
| **Primary** | The one thing users must see | Largest, boldest, most contrast, most whitespace |
| **Secondary** | Supporting information | Medium size, regular weight, slightly muted |
| **Tertiary** | Supplementary details | Smallest, lightest, most muted |

If you have more than 3 levels competing for attention, the hierarchy feels flat.
Use progressive disclosure (tabs, accordions, "show more") to manage additional levels.

### Content-First Approach

1. **List all content elements** for the page/component.
2. **Rank them by importance** from the user's perspective (not the developer's or stakeholder's).
3. **Assign hierarchy levels** (primary, secondary, tertiary).
4. **Apply the seven tools** to establish visual differentiation.
5. **Validate** with the squint test.

---

## Layout Patterns and Their Hierarchy Implications

### Hero Sections
- Single focal point: headline + CTA.
- Everything else (nav, secondary text, background) must recede.
- Hero headline should be 2–3x larger than body text.

### Dashboards
- Lead with the most critical KPI — largest card, most prominent position.
- Secondary metrics in a grid, equal weight within their tier.
- Dense dashboards still need a focal point — don't make everything equal.

### Lists and Feeds
- Each item should have internal hierarchy: title (primary) → description (secondary) → metadata (tertiary).
- The current/active item should be visually elevated above siblings.
- List items should be scannable by title alone — users won't read descriptions during scanning.

### Forms
- Labels are primary within each field group; inputs are secondary; help text is tertiary.
- Section headings break long forms into scannable chunks.
- The submit button is the terminal focal point — it should stand out from all form fields.
- Required field indicators must be visible but not dominant.

### Cards
- Internal hierarchy: image (visual anchor) → title (primary text) → description (secondary text) → metadata/actions (tertiary).
- Card-to-card hierarchy: if some cards are more important, differentiate by size, position, or visual treatment (featured card pattern).

---

## Review Checklist

After building any layout, verify:

1. **Squint test** — Can you still identify the most important element when blurred?
2. **3-level check** — Are there at most 3 clearly distinct hierarchy levels?
3. **Focal point** — Is there exactly one primary focal point per viewport?
4. **Type scale** — Are heading levels visually distinct from each other and from body text?
5. **Color hierarchy** — Does the hierarchy hold in grayscale?
6. **Whitespace** — Is space used to isolate important elements and group related ones?
7. **Position** — Is critical content placed where scan patterns naturally flow?
8. **Depth** — Is elevation used consistently and sparingly?
9. **CTA clarity** — Is the primary action unmistakably the most prominent interactive element?
10. **Scan path** — Can a user find the essential information in the first 3 seconds?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| Everything is the same size and weight | No hierarchy — user doesn't know where to start | Establish at least 3 distinct visual levels |
| Multiple competing focal points | User's attention is split — nothing is "first" | One primary per viewport; demote others |
| CTA has same visual weight as surrounding content | Primary action is invisible | Increase size, contrast, whitespace, or use accent color |
| Headings that don't look like headings | Content reads as a wall of text | Increase size and weight gap between headings and body |
| Equal spacing everywhere | No visual rhythm, groupings are unclear | Vary spacing: tight within groups, generous between them |
| Color-only hierarchy | Breaks for colorblind users, breaks in grayscale | Combine color with size, weight, and spatial differences |
| Important information buried below the fold | Users may never scroll to see it | Move above fold, or provide clear visual cues to scroll |
| Decorative elements competing with content | User is distracted from the actual information | Reduce contrast on decorative elements; they should recede |
| Dense layout with no breathing room | Overwhelming, hard to scan, feels cheap | Add whitespace between sections; consider progressive disclosure |
| Navigation visually louder than page content | Frame dominates the picture | Nav should be structurally present but visually recessive |
