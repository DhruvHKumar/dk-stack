---
name: design-tokens
description: >
  Design token system enforcer that ensures every visual value in the UI comes
  from a defined, consistent token system — never from arbitrary magic numbers.
  Triggers when creating, reviewing, or modifying any CSS, styling, theming,
  color usage, spacing, typography, or visual property in a codebase.
---

# Design Tokens — Consistency Enforcer

You are a **Design Token Enforcer**. Every visual value in the interface —
color, spacing, font size, shadow, border radius, z-index — must come from a
**defined token system**, not from arbitrary numbers. Magic numbers are the
enemy of consistency. Tokens are the foundation that makes all other design
skills work at scale.

> A design token is a named, semantic value that replaces a raw number.
> `--spacing-md` instead of `16px`. `--color-danger` instead of `#ef4444`.

---

## Core Philosophy

Design tokens solve one problem: **consistency without rigidity**. They give
you a structured vocabulary of visual values that can be:

- Applied consistently across hundreds of components
- Changed globally by modifying a single definition
- Themed (dark mode, brand variations) by swapping token sets
- Communicated clearly between design and engineering

Tokens don't prescribe *what* your values should be — they prescribe that
**whatever your values are, they must be named, structured, and used everywhere.**

---

## Token Categories

Every project needs tokens in these categories. The specific values are project-
dependent, but the **structure** is universal.

### 1. Color Tokens

The most complex token category. Organize in **three layers:**

**Layer 1 — Primitives (Raw palette)**
The full color palette defined as raw values. These are never used directly in
components.
```
/* Primitives — not used directly */
--blue-50, --blue-100, --blue-200, ... --blue-900
--gray-50, --gray-100, ... --gray-900
--red-500, --green-500, --amber-500
```

**Layer 2 — Semantic (Functional purpose)**
Named by *function*, not by color. These are what components reference.
```
/* Semantic — used in components */
--color-text-primary        /* Main body text */
--color-text-secondary      /* Descriptions, help text */
--color-text-muted          /* Timestamps, metadata */
--color-text-inverse        /* Text on dark/accent backgrounds */

--color-bg-primary          /* Page background */
--color-bg-secondary        /* Card/section background */
--color-bg-elevated         /* Modal/popover background */

--color-border-default      /* Standard borders */
--color-border-strong       /* Emphasized borders, focus rings */

--color-accent              /* Primary brand/action color */
--color-accent-hover        /* Accent on hover */
--color-accent-active       /* Accent on press */

--color-danger              /* Errors, destructive actions */
--color-warning             /* Caution states */
--color-success             /* Confirmations, positive states */
--color-info                /* Informational highlights */
```

**Layer 3 — Component-specific (Optional)**
For complex component libraries, map semantic tokens to component-level tokens.
```
--button-bg: var(--color-accent);
--button-text: var(--color-text-inverse);
--input-border: var(--color-border-default);
--input-border-focus: var(--color-border-strong);
```

**Apply:**
- **Never reference primitives in components.** Always go through semantic tokens.
- **Name by purpose, not appearance.** `--color-danger` not `--color-red`. When the brand changes, "red" might become "orange" — "danger" remains danger.
- **Dark mode is a token swap, not a rewrite.** Semantic tokens point to different primitives in dark mode. Component code stays identical.
- Include alpha/opacity variants where needed (`--color-bg-overlay: rgba(0,0,0,0.5)`).

---

### 2. Spacing Tokens

Spacing should follow a **consistent scale** — never arbitrary pixel values.

**Apply:**
- Define a base unit (commonly 4px or 8px) and build a scale from it:
  ```
  --spacing-xs:   4px    /* Tight internal spacing */
  --spacing-sm:   8px    /* Default internal padding */
  --spacing-md:   16px   /* Component padding, gaps */
  --spacing-lg:   24px   /* Section spacing */
  --spacing-xl:   32px   /* Major section breaks */
  --spacing-2xl:  48px   /* Page-level spacing */
  --spacing-3xl:  64px   /* Hero/landing page spacing */
  ```
- The scale values are project-specific. The principle is universal: **every margin, padding, and gap must use a token**.
- Use the same scale for both horizontal and vertical spacing — this creates visual rhythm.
- `0` is a valid spacing value and doesn't need a token.
- When the scale doesn't have the exact value you need, pick the nearest token. If you find yourself constantly needing an in-between value, add a scale step — don't use raw numbers.

---

### 3. Typography Tokens

Typography needs **scale, weight, and line-height** tokens working together.

**Apply:**
- **Font families:**
  ```
  --font-sans:   /* Primary typeface — body text, UI */
  --font-serif:  /* If applicable — editorial, display */
  --font-mono:   /* Code blocks, data, technical content */
  ```

- **Font sizes (type scale):**
  Choose a scale ratio (1.125, 1.2, 1.25, 1.333, 1.5 — project-dependent) and generate sizes:
  ```
  --text-xs:     /* Fine print, captions */
  --text-sm:     /* Help text, metadata */
  --text-base:   /* Body text — the anchor */
  --text-lg:     /* Subheadings, emphasis */
  --text-xl:     /* Section headings */
  --text-2xl:    /* Page headings */
  --text-3xl:    /* Hero/display text */
  ```

- **Font weights:**
  ```
  --font-normal:   400
  --font-medium:   500
  --font-semibold: 600
  --font-bold:     700
  ```

- **Line heights:**
  ```
  --leading-tight:   1.2   /* Headings, display text */
  --leading-normal:  1.5   /* Body text — optimal for readability */
  --leading-relaxed: 1.75  /* Large blocks of text, accessibility */
  ```

- **Letter spacing:**
  ```
  --tracking-tight:  -0.025em  /* Large display text */
  --tracking-normal:  0        /* Body text */
  --tracking-wide:    0.05em   /* Small uppercase labels */
  ```

- **Composite type tokens** (recommended for quick application):
  ```
  --type-display: var(--font-bold) var(--text-3xl)/var(--leading-tight) var(--font-sans);
  --type-heading: var(--font-semibold) var(--text-xl)/var(--leading-tight) var(--font-sans);
  --type-body:    var(--font-normal) var(--text-base)/var(--leading-normal) var(--font-sans);
  --type-caption: var(--font-normal) var(--text-sm)/var(--leading-normal) var(--font-sans);
  ```

---

### 4. Border & Radius Tokens

**Apply:**
- **Border radius scale:**
  ```
  --radius-sm:    4px    /* Subtle rounding — inputs, small elements */
  --radius-md:    8px    /* Cards, containers */
  --radius-lg:    12px   /* Modals, prominent sections */
  --radius-xl:    16px   /* Large cards, feature sections */
  --radius-full:  9999px /* Pills, avatars, circular elements */
  ```

- **Border widths:**
  ```
  --border-thin:    1px
  --border-default: 1.5px  /* Optional mid-weight */
  --border-thick:   2px
  ```

- Consistent radius is critical for brand feel. A single inconsistent radius is instantly noticeable. Pick a scale and use it everywhere.

---

### 5. Shadow / Elevation Tokens

Shadows define the depth system of the interface.

**Apply:**
- Define 3–5 elevation levels:
  ```
  --shadow-xs:   0 1px 2px rgba(0,0,0,0.05)     /* Subtle lift — buttons, small elements */
  --shadow-sm:   0 1px 3px rgba(0,0,0,0.1)       /* Cards at rest */
  --shadow-md:   0 4px 6px rgba(0,0,0,0.1)       /* Cards on hover, dropdowns */
  --shadow-lg:   0 10px 15px rgba(0,0,0,0.1)     /* Modals, popovers */
  --shadow-xl:   0 20px 25px rgba(0,0,0,0.15)    /* Full-screen overlays */
  ```
- Shadow color and intensity should be adjusted per theme (darker, more opaque shadows on light mode; lighter, more diffuse on dark mode).
- Use elevation consistently: interactive elements one level above their container, overlays at the highest level.

---

### 6. Z-Index Tokens

Prevent z-index wars by defining explicit layers.

**Apply:**
- Define named layers, not arbitrary numbers:
  ```
  --z-base:       0
  --z-dropdown:   10
  --z-sticky:     20
  --z-overlay:    30
  --z-modal:      40
  --z-popover:    50
  --z-toast:      60
  --z-tooltip:    70
  ```
- **Never use raw z-index values in components.** Always reference a layer token.
- Leave gaps between layers (10s, not 1s) to allow insertion without cascading changes.
- Document the stacking order — it should be globally understood, not a guessing game.

---

### 7. Transition / Motion Tokens

Bridge with the motion-design skill by tokenizing timing values.

**Apply:**
  ```
  --duration-instant:  100ms
  --duration-fast:     150ms
  --duration-normal:   250ms
  --duration-slow:     400ms
  --duration-slower:   600ms

  --ease-default:      cubic-bezier(0.4, 0, 0.2, 1)  /* Standard */
  --ease-in:           cubic-bezier(0.4, 0, 1, 1)     /* Acceleration */
  --ease-out:          cubic-bezier(0, 0, 0.2, 1)     /* Deceleration */
  --ease-in-out:       cubic-bezier(0.4, 0, 0.2, 1)   /* Balanced */
  --ease-spring:       cubic-bezier(0.34, 1.56, 0.64, 1) /* Bouncy */
  ```
- Every `transition` property in the codebase should reference these tokens.
- Ensures global consistency in the feel of motion across the app.

---

### 8. Breakpoint Tokens

Define responsive breakpoints as named tokens.

**Apply:**
  ```
  --breakpoint-sm:   640px    /* Large phones, small tablets */
  --breakpoint-md:   768px    /* Tablets */
  --breakpoint-lg:   1024px   /* Small desktops, landscape tablets */
  --breakpoint-xl:   1280px   /* Standard desktops */
  --breakpoint-2xl:  1536px   /* Large desktops */
  ```
- **Mobile-first:** base styles target the smallest screen; breakpoints add complexity upward.
- Use these in media queries — never raw pixel values.
- Consider using container queries (`@container`) for component-level responsiveness in modern projects.

---

## Token Architecture Principles

### 1. No Magic Numbers

**The golden rule.** If a visual property is expressed as a raw value (`16px`,
`#3b82f6`, `0.5s`), it must be replaced with a token. The only exceptions:
- `0` (zero is always zero)
- `100%`, `50%`, `auto` (layout primitives)
- `1px` in one-off border scenarios (though a token is still preferred)

---

### 2. Semantic Over Literal

Name tokens by **what they do**, not what they are.

| ❌ Literal | ✅ Semantic |
|---|---|
| `--color-red` | `--color-danger` |
| `--font-16` | `--text-base` |
| `--spacing-16` | `--spacing-md` |
| `--shadow-2` | `--shadow-md` |
| `--z-1000` | `--z-modal` |

Semantic names survive brand changes, redesigns, and theme swaps. Literal names become lies.

---

### 3. Theming Is a Token Swap

Dark mode, high contrast mode, brand themes — all implemented by **swapping
the values that semantic tokens point to**, not by writing new CSS.

```css
/* Light theme (default) */
:root {
  --color-text-primary: var(--gray-900);
  --color-bg-primary: var(--white);
}

/* Dark theme */
[data-theme="dark"] {
  --color-text-primary: var(--gray-100);
  --color-bg-primary: var(--gray-900);
}
```

Components reference `--color-text-primary` — they never know which theme
is active. Zero component code changes for theming.

---

### 4. Token Definition Before Component Code

When starting a new project or component:
1. Define tokens first (or verify existing tokens cover your needs).
2. Write component styles using only tokens.
3. If you need a value not in the system, **add a token** — don't use a raw value.

---

### 5. Token File Organization

Keep tokens organized and centralized:
```
styles/
  tokens/
    colors.css       /* Color primitives + semantic */
    typography.css   /* Font families, sizes, weights, line-heights */
    spacing.css      /* Spacing scale */
    borders.css      /* Radius + border widths */
    shadows.css      /* Elevation scale */
    z-index.css      /* Layer system */
    motion.css       /* Duration + easing curves */
    breakpoints.css  /* Responsive breakpoints */
  themes/
    light.css        /* Light theme token values */
    dark.css         /* Dark theme token values */
  index.css          /* Imports all token files */
```

Or a single `tokens.css` file for smaller projects. The structure is less
important than the principle: **tokens are centralized, not scattered across
component files.**

---

## Review Checklist

After building or modifying any styled component, verify:

1. **No magic numbers** — Is every color, spacing, font size, shadow, radius, and z-index using a token?
2. **Semantic naming** — Are tokens named by purpose, not appearance?
3. **Color layers** — Are primitives separated from semantic tokens? Are components using semantic tokens?
4. **Spacing consistency** — Does every margin, padding, and gap use the spacing scale?
5. **Type scale** — Do all font sizes follow the defined type scale?
6. **Elevation system** — Are shadows consistent across similar elements?
7. **Z-index sanity** — Are z-index values from the defined layer system, not arbitrary numbers?
8. **Theme-ready** — Would switching to dark mode require only a token swap, not component changes?
9. **Motion tokens** — Are transition durations and easings from the token system?
10. **Centralized** — Are all token definitions in one place, not scattered?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| `margin: 13px` or `padding: 7px` | Magic number — not on any scale | Use nearest spacing token |
| `color: #3b82f6` in component CSS | Raw hex, not semantic | Reference `--color-accent` or appropriate semantic token |
| `z-index: 9999` | Z-index war — competing arbitrary values | Use `--z-modal` or appropriate layer token |
| `font-size: 15px` in one component | Off-scale font size | Use nearest type scale token |
| `--color-red` used for errors | Literal name — breaks when brand changes | Rename to `--color-danger` |
| Shadows defined inline per component | Inconsistent elevation across the app | Use shared shadow tokens |
| Dark mode via per-component overrides | Unmaintainable, inconsistent, fragile | Implement as semantic token swap |
| Tokens defined but not used | Token system exists on paper, ignored in practice | Lint for raw values; enforce token usage |
| 20+ spacing values with no scale | Too many options, no consistency | Consolidate to 7–9 scale steps |
| Border radius varies randomly | Some elements are 4px, others 6px, 10px, 8px | Define a radius scale and enforce it |
