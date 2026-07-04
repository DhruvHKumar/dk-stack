---
name: web-animation-patterns
description: >
  Framer-style web animation pattern library that provides the specific
  vocabulary and implementation techniques for modern, cinematic web animations.
  Triggers when building scroll-triggered reveals, spring physics animations,
  text splitting effects, parallax, magnetic interactions, layout animations,
  stagger sequences, or any "award-winning website" style motion. Complements
  the motion-design skill (principles) with concrete patterns and code.
---

# Web Animation Patterns — The Modern Motion Vocabulary

You are a **Web Animation Pattern Specialist**. You know the specific animation
techniques that define modern, high-end web experiences — the kind seen on
Awwwards, FWA, and sites built with Framer, GSAP, and Motion. This skill is
your **pattern library**: the concrete recipes for scroll reveals, spring
physics, text splits, parallax, and gesture-driven motion.

This skill complements the **motion-design** skill (which covers *when* and
*why* to animate). This skill covers *what* to animate and *how*.

> The motion-design skill is the theory. This skill is the cookbook.

---

## Pattern Categories

1. [Scroll-Triggered Reveals](#1-scroll-triggered-reveals)
2. [Spring Physics](#2-spring-physics)
3. [Text Animation](#3-text-animation)
4. [Stagger & Orchestration](#4-stagger--orchestration)
5. [Parallax & Scroll-Linked](#5-parallax--scroll-linked)
6. [Layout Animation](#6-layout-animation)
7. [Presence Animation](#7-presence-animation-mountunmount)
8. [Gesture-Driven Motion](#8-gesture-driven-motion)
9. [Cursor & Magnetic Effects](#9-cursor--magnetic-effects)
10. [Continuous & Ambient Motion](#10-continuous--ambient-motion)
11. [Number & Data Animation](#11-number--data-animation)
12. [Path & Morphing](#12-path--morphing-animation)

---

## 1. Scroll-Triggered Reveals

The signature of modern web design. Elements animate in as they enter
the viewport.

### The Core Pattern: Fade-Up Reveal

The most common and versatile scroll animation. Element starts invisible
and translated down, then fades in and slides to its final position.

```css
/* CSS-only with @starting-style and IntersectionObserver class toggle */
.reveal {
  opacity: 0;
  transform: translateY(30px);
  transition: opacity 0.6s ease-out, transform 0.6s ease-out;
}

.reveal.is-visible {
  opacity: 1;
  transform: translateY(0);
}
```

```javascript
// IntersectionObserver — the right way to trigger scroll reveals
const observer = new IntersectionObserver(
  (entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        entry.target.classList.add('is-visible');
        observer.unobserve(entry.target); // Animate once
      }
    });
  },
  { threshold: 0.15, rootMargin: '0px 0px -50px 0px' }
);

document.querySelectorAll('.reveal').forEach((el) => observer.observe(el));
```

### Reveal Variants

| Variant | Transform Start | Use Case |
|---|---|---|
| Fade up | `translateY(30px)` | Default — works everywhere |
| Fade down | `translateY(-30px)` | Headers, top-anchored elements |
| Fade left | `translateX(40px)` | Right-aligned content, alternating layouts |
| Fade right | `translateX(-40px)` | Left-aligned content |
| Scale up | `scale(0.95)` | Cards, images, hero elements |
| Scale + fade up | `translateY(20px) scale(0.98)` | Premium feel — combines both |
| Clip reveal | `clip-path: inset(100% 0 0 0)` | Cinematic — content "wiped" into view |
| Blur reveal | `filter: blur(10px); opacity: 0` | Dreamy, editorial — use sparingly |

**Apply:**
- `threshold: 0.1–0.2` — trigger when 10–20% of the element is visible. Don't wait for full visibility.
- `rootMargin: '0px 0px -50px 0px'` — slight negative bottom margin prevents triggering at the very edge.
- **Animate once.** Unobserve after triggering. Re-animating on scroll-back feels glitchy, not premium.
- Translate distance should be **20–40px** — enough to be perceived, not enough to look like a page jump.
- Duration: **0.5–0.8s** for reveals. Longer than 1s feels sluggish.

### CSS Scroll-Driven Animations (Native, No JS)

For scroll-progress-linked animations (not threshold-triggered):

```css
/* Animation timeline tied to scroll position */
@keyframes reveal-on-scroll {
  from {
    opacity: 0;
    transform: translateY(40px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.scroll-reveal {
  animation: reveal-on-scroll linear both;
  animation-timeline: view();
  animation-range: entry 0% entry 40%;
}
```

This runs on the **compositor thread** — zero main-thread cost, butter-smooth.
Use for progress bars, parallax-style reveals, and scroll-linked transformations.

---

## 2. Spring Physics

Springs create motion that feels **physical and alive** — not robotic.
A spring-based animation has no fixed duration; it settles naturally based
on physics parameters.

### The Spring Model

A spring animation is defined by three parameters:

| Parameter | What It Controls | Range | Effect |
|---|---|---|---|
| **Stiffness** | How tight the spring is | 50–1000 | Higher = snappier, faster |
| **Damping** | How quickly oscillation stops | 5–100 | Higher = less bounce, more controlled |
| **Mass** | Weight of the animated element | 0.1–5 | Higher = slower, more inertial |

### Spring Presets for Different Feels

| Preset | Stiffness | Damping | Mass | Feel | Use Case |
|---|---|---|---|---|---|
| **Snappy** | 400 | 30 | 0.8 | Quick, responsive, minimal overshoot | Buttons, toggles, small UI |
| **Smooth** | 200 | 25 | 1 | Balanced, elegant, slight settle | Cards, modals, page transitions |
| **Bouncy** | 300 | 10 | 1 | Playful, energetic, visible oscillation | Playful brands, celebrations, gamification |
| **Gentle** | 120 | 20 | 1.2 | Slow, heavy, luxurious | Hero sections, large imagery, premium feel |
| **Stiff** | 600 | 40 | 0.5 | Near-instant, barely perceptible settle | Micro-interactions, hover responses |

### Implementing Springs in Pure JS

```javascript
function spring(current, target, velocity, { stiffness = 200, damping = 25, mass = 1 }) {
  const force = -stiffness * (current - target);
  const dampingForce = -damping * velocity;
  const acceleration = (force + dampingForce) / mass;
  const newVelocity = velocity + acceleration * (1 / 60); // 60fps timestep
  const newPosition = current + newVelocity * (1 / 60);
  return { position: newPosition, velocity: newVelocity };
}

// Animation loop
function animateSpring(element, property, target, config) {
  let current = parseFloat(getComputedStyle(element)[property]) || 0;
  let velocity = 0;

  function tick() {
    const result = spring(current, target, velocity, config);
    current = result.position;
    velocity = result.velocity;

    element.style[property] = `${current}px`;

    // Settle threshold — stop when motion is negligible
    if (Math.abs(velocity) > 0.01 || Math.abs(current - target) > 0.01) {
      requestAnimationFrame(tick);
    } else {
      element.style[property] = `${target}px`;
    }
  }

  requestAnimationFrame(tick);
}
```

**Apply:**
- Use springs for **interactive responses** (hover, drag, toggle) — they feel alive.
- Use CSS easing (ease-out) for **one-shot reveals** (scroll triggers) — simpler, more performant.
- Spring damping < 15 creates visible bounce. Match to brand tone: financial apps = high damping, creative apps = low damping.
- Always define a **settle threshold** (< 0.01px movement) to prevent infinite loops.

---

## 3. Text Animation

The hallmark of cinematic web design. Text that doesn't just appear — it
*performs*.

### Word-by-Word Reveal

```javascript
function splitIntoWords(element) {
  const text = element.textContent;
  element.innerHTML = text.split(' ').map((word, i) =>
    `<span class="word" style="--i: ${i}">
      <span class="word-inner">${word}</span>
    </span>`
  ).join(' ');
}
```

```css
.word {
  display: inline-block;
  overflow: hidden; /* Clip mask for the inner element */
}

.word-inner {
  display: inline-block;
  transform: translateY(110%);
  transition: transform 0.5s cubic-bezier(0.16, 1, 0.3, 1);
  transition-delay: calc(var(--i) * 60ms); /* Stagger */
}

.is-visible .word-inner {
  transform: translateY(0);
}
```

### Character-by-Character Reveal

Same concept, split into individual characters:

```javascript
function splitIntoChars(element) {
  const text = element.textContent;
  element.innerHTML = text.split('').map((char, i) =>
    char === ' '
      ? ' '
      : `<span class="char" style="--i: ${i}">${char}</span>`
  ).join('');
}
```

```css
.char {
  display: inline-block;
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.4s ease-out, transform 0.4s ease-out;
  transition-delay: calc(var(--i) * 30ms);
}

.is-visible .char {
  opacity: 1;
  transform: translateY(0);
}
```

### Line-by-Line Reveal with Clip Mask

The "cinematic" text effect — text slides up from behind an invisible edge:

```css
.line-reveal {
  overflow: hidden; /* The clip boundary */
}

.line-reveal span {
  display: block;
  transform: translateY(100%);
  transition: transform 0.7s cubic-bezier(0.16, 1, 0.3, 1);
}

.is-visible .line-reveal span {
  transform: translateY(0);
}
```

### Typewriter Effect

```css
.typewriter {
  overflow: hidden;
  border-right: 2px solid currentColor;
  white-space: nowrap;
  animation:
    typing 3s steps(40, end),
    blink-caret 0.75s step-end infinite;
  width: 0;
}

.is-visible .typewriter {
  width: 100%;
}

@keyframes typing {
  from { width: 0; }
  to { width: 100%; }
}

@keyframes blink-caret {
  50% { border-color: transparent; }
}
```

**Apply:**
- **Word split** is the sweet spot for most use cases — readable and dramatic.
- **Char split** for hero headlines only. On body text, it's distracting and inaccessible.
- Stagger delay per item: **30–80ms** for characters, **50–100ms** for words.
- Always keep the original text content intact for screen readers — split only the visual layer.
- Use `overflow: hidden` on the wrapper for clip-mask reveals — it's what creates the "emerging" effect.
- The easing `cubic-bezier(0.16, 1, 0.3, 1)` (aggressive ease-out) is the signature curve for text reveals.

---

## 4. Stagger & Orchestration

Multiple elements animating in a coordinated sequence. The foundation of
"premium" feeling interfaces.

### CSS Custom Property Stagger

```css
.stagger-group > * {
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.5s ease-out, transform 0.5s ease-out;
  transition-delay: calc(var(--stagger-index, 0) * 80ms);
}

.stagger-group.is-visible > * {
  opacity: 1;
  transform: translateY(0);
}
```

```html
<div class="stagger-group">
  <div style="--stagger-index: 0">First</div>
  <div style="--stagger-index: 1">Second</div>
  <div style="--stagger-index: 2">Third</div>
</div>
```

### Auto-Index with JS

```javascript
document.querySelectorAll('.stagger-group').forEach(group => {
  Array.from(group.children).forEach((child, i) => {
    child.style.setProperty('--stagger-index', i);
  });
});
```

### Orchestration Principles

| Pattern | Stagger Delay | Max Total Duration | Use Case |
|---|---|---|---|
| **Quick cascade** | 30–50ms | < 400ms | Nav items, small lists |
| **Standard stagger** | 60–100ms | < 800ms | Card grids, feature lists |
| **Dramatic sequence** | 100–150ms | < 1200ms | Hero sections, onboarding |
| **Wave** | 40–60ms (with direction) | < 600ms | Grid items, data visualization |

**Apply:**
- **Cap total stagger duration.** If you have 20 items × 100ms = 2 seconds of stagger. Too long. Either reduce the delay or animate in visible batches.
- The last item's animation should finish within **1.2 seconds** of the first — beyond that, it feels like lag.
- Stagger by **visual order**, not DOM order — top-to-bottom, left-to-right in LTR layouts.
- Items off-screen should not stagger — they haven't been "revealed" yet.

---

## 5. Parallax & Scroll-Linked

Elements moving at different speeds as the user scrolls, creating depth.

### CSS-Only Parallax (Scroll-Driven)

```css
.parallax-slow {
  animation: parallax-shift linear both;
  animation-timeline: scroll();
}

@keyframes parallax-shift {
  from { transform: translateY(0); }
  to { transform: translateY(-100px); }
}

.parallax-fast {
  animation: parallax-shift-fast linear both;
  animation-timeline: scroll();
}

@keyframes parallax-shift-fast {
  from { transform: translateY(0); }
  to { transform: translateY(-250px); }
}
```

### Scroll Progress Bar

```css
.scroll-progress {
  position: fixed;
  top: 0;
  left: 0;
  height: 3px;
  background: var(--color-accent);
  transform-origin: left;
  animation: scale-x linear both;
  animation-timeline: scroll(root);
}

@keyframes scale-x {
  from { transform: scaleX(0); }
  to { transform: scaleX(1); }
}
```

### Horizontal Scroll Section

```css
.horizontal-scroll-container {
  height: 300vh; /* Controls scroll distance */
}

.horizontal-scroll-track {
  position: sticky;
  top: 0;
  height: 100vh;
  overflow: hidden;
}

.horizontal-scroll-content {
  display: flex;
  gap: 2rem;
  animation: scroll-horizontal linear both;
  animation-timeline: scroll(nearest);
}

@keyframes scroll-horizontal {
  from { transform: translateX(0); }
  to { transform: translateX(calc(-100% + 100vw)); }
}
```

**Apply:**
- **CSS `animation-timeline: scroll()`** is the preferred approach — runs on the compositor, zero JS, silky smooth.
- Parallax distance should be **subtle** (50–150px over a full viewport scroll). Aggressive parallax causes motion sickness.
- **Always disable for `prefers-reduced-motion`.** Parallax is the #1 trigger for vestibular disorders.
- Foreground elements should move faster than background — this creates natural depth perception.
- Never parallax text. Parallax should only affect images, shapes, and decorative elements.

---

## 6. Layout Animation

Smoothly animating elements when their position or size changes in the DOM.

### FLIP Technique (First, Last, Invert, Play)

The gold standard for layout transitions:

```javascript
function flipAnimate(element, changeCallback) {
  // FIRST: Record current position
  const first = element.getBoundingClientRect();

  // Change: Apply the DOM/layout change
  changeCallback();

  // LAST: Record new position
  const last = element.getBoundingClientRect();

  // INVERT: Calculate the delta and apply inverse transform
  const deltaX = first.left - last.left;
  const deltaY = first.top - last.top;
  const deltaW = first.width / last.width;
  const deltaH = first.height / last.height;

  element.style.transform = `translate(${deltaX}px, ${deltaY}px) scale(${deltaW}, ${deltaH})`;
  element.style.transformOrigin = 'top left';

  // PLAY: Animate to the new position
  requestAnimationFrame(() => {
    element.style.transition = 'transform 0.4s cubic-bezier(0.2, 0, 0, 1)';
    element.style.transform = 'none';

    element.addEventListener('transitionend', () => {
      element.style.transition = '';
      element.style.transformOrigin = '';
    }, { once: true });
  });
}
```

### View Transitions API (Native)

```javascript
// For same-document transitions
document.startViewTransition(() => {
  // DOM changes happen here
  updateContent();
});
```

```css
/* Customize the transition */
::view-transition-old(root) {
  animation: fade-out 0.3s ease-out;
}

::view-transition-new(root) {
  animation: fade-in 0.3s ease-in;
}

/* Named transitions for shared elements */
.card-image {
  view-transition-name: hero-image;
}
```

**Apply:**
- **FLIP is the workhorse** for list reordering, filter transitions, and tab switches.
- **View Transitions API** for page-level transitions — simpler API, browser-optimized.
- Layout animations should be **fast** (300–500ms). Users are waiting for the new state.
- Never animate layout properties directly (`width`, `height`, `top`, `left`). Always use `transform`.

---

## 7. Presence Animation (Mount/Unmount)

Elements that animate gracefully when they enter or leave the DOM.

### CSS `@starting-style` (Native Entry Animations)

```css
.modal {
  opacity: 1;
  transform: translateY(0);
  transition: opacity 0.3s ease-out, transform 0.3s ease-out;

  /* Define the "entry" state */
  @starting-style {
    opacity: 0;
    transform: translateY(20px);
  }
}
```

### Exit Animations with `display`

```css
.toast {
  transition:
    opacity 0.3s ease-in,
    transform 0.3s ease-in,
    display 0.3s allow-discrete;

  @starting-style {
    opacity: 0;
    transform: translateY(-10px);
  }
}

.toast.is-dismissed {
  opacity: 0;
  transform: translateY(-10px);
  display: none;
}
```

**Apply:**
- `@starting-style` is the modern CSS solution for entry animations without JS.
- `allow-discrete` on `transition` enables animating `display: none` natively.
- Exit animations should be **faster than entry** (~70% of entry duration).
- Elements should exit in the **reverse direction** of their entry.

---

## 8. Gesture-Driven Motion

Animations that respond to user gestures — drag, swipe, pinch.

### Draggable Element

```javascript
function makeDraggable(element, { bounds = null, onRelease = null } = {}) {
  let isDragging = false;
  let startX, startY, currentX = 0, currentY = 0;

  element.addEventListener('pointerdown', (e) => {
    isDragging = true;
    startX = e.clientX - currentX;
    startY = e.clientY - currentY;
    element.style.cursor = 'grabbing';
    element.setPointerCapture(e.pointerId);
  });

  element.addEventListener('pointermove', (e) => {
    if (!isDragging) return;
    currentX = e.clientX - startX;
    currentY = e.clientY - startY;

    // Apply with transform for performance
    element.style.transform = `translate(${currentX}px, ${currentY}px)`;
  });

  element.addEventListener('pointerup', () => {
    isDragging = false;
    element.style.cursor = 'grab';
    if (onRelease) onRelease({ x: currentX, y: currentY });
  });
}
```

### Swipe-to-Dismiss

```javascript
function swipeToDismiss(element, { threshold = 100, onDismiss }) {
  let startX, currentX = 0;

  element.addEventListener('pointerdown', (e) => {
    startX = e.clientX;
    element.style.transition = 'none';
    element.setPointerCapture(e.pointerId);
  });

  element.addEventListener('pointermove', (e) => {
    currentX = e.clientX - startX;
    const opacity = 1 - Math.abs(currentX) / (threshold * 2);
    element.style.transform = `translateX(${currentX}px)`;
    element.style.opacity = Math.max(opacity, 0);
  });

  element.addEventListener('pointerup', () => {
    element.style.transition = 'transform 0.3s ease-out, opacity 0.3s ease-out';
    if (Math.abs(currentX) > threshold) {
      element.style.transform = `translateX(${currentX > 0 ? '100%' : '-100%'})`;
      element.style.opacity = '0';
      setTimeout(() => onDismiss?.(element), 300);
    } else {
      element.style.transform = 'translateX(0)';
      element.style.opacity = '1';
      currentX = 0;
    }
  });
}
```

**Apply:**
- Use **Pointer Events** (not mouse/touch events) for unified input handling.
- Always use `setPointerCapture` to avoid losing the gesture mid-drag.
- Provide **visual feedback during gesture** (opacity, rotation, scale changes proportional to drag distance).
- Define a **commit threshold** — the distance at which the gesture "completes" vs "snaps back."
- Spring-animate the snap-back for a natural feel.

---

## 9. Cursor & Magnetic Effects

Subtle cursor-following effects that make the interface feel reactive.

### Magnetic Button

```javascript
function magneticButton(element, { strength = 0.3, radius = 100 } = {}) {
  element.addEventListener('mousemove', (e) => {
    const rect = element.getBoundingClientRect();
    const centerX = rect.left + rect.width / 2;
    const centerY = rect.top + rect.height / 2;
    const distX = e.clientX - centerX;
    const distY = e.clientY - centerY;
    const distance = Math.sqrt(distX ** 2 + distY ** 2);

    if (distance < radius) {
      const pull = (1 - distance / radius) * strength;
      element.style.transform = `translate(${distX * pull}px, ${distY * pull}px)`;
    }
  });

  element.addEventListener('mouseleave', () => {
    element.style.transition = 'transform 0.4s cubic-bezier(0.16, 1, 0.3, 1)';
    element.style.transform = 'translate(0, 0)';
    element.addEventListener('transitionend', () => {
      element.style.transition = '';
    }, { once: true });
  });
}
```

### 3D Card Tilt on Hover

```javascript
function tiltCard(element, { intensity = 15 } = {}) {
  element.style.transformStyle = 'preserve-3d';
  element.style.transition = 'transform 0.1s ease-out';

  element.addEventListener('mousemove', (e) => {
    const rect = element.getBoundingClientRect();
    const x = (e.clientX - rect.left) / rect.width - 0.5;
    const y = (e.clientY - rect.top) / rect.height - 0.5;

    element.style.transform = `
      perspective(800px)
      rotateY(${x * intensity}deg)
      rotateX(${-y * intensity}deg)
      scale(1.02)
    `;
  });

  element.addEventListener('mouseleave', () => {
    element.style.transition = 'transform 0.5s cubic-bezier(0.16, 1, 0.3, 1)';
    element.style.transform = 'perspective(800px) rotateY(0) rotateX(0) scale(1)';
  });
}
```

**Apply:**
- Magnetic effects are **desktop-only** — disable on touch devices.
- Keep magnetic strength **subtle** (0.2–0.4). Too strong feels like the UI is broken.
- Tilt intensity: **10–20 degrees** max. Beyond that, content becomes unreadable.
- Always spring-animate the return to resting position.
- These effects are **enhancements** — the UI must work perfectly without them.

---

## 10. Continuous & Ambient Motion

Subtle, looping animations that keep the interface feeling alive.

### Infinite Marquee / Ticker

```css
.marquee {
  overflow: hidden;
  white-space: nowrap;
}

.marquee-track {
  display: inline-flex;
  animation: marquee 30s linear infinite;
}

/* Duplicate content for seamless loop */
.marquee-track > * {
  flex-shrink: 0;
}

@keyframes marquee {
  from { transform: translateX(0); }
  to { transform: translateX(-50%); }
}

/* Pause on hover for accessibility */
.marquee:hover .marquee-track {
  animation-play-state: paused;
}
```

### Gradient Animation

```css
.animated-gradient {
  background: linear-gradient(-45deg, #ee7752, #e73c7e, #23a6d5, #23d5ab);
  background-size: 400% 400%;
  animation: gradient-shift 15s ease infinite;
}

@keyframes gradient-shift {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}
```

### Floating Elements

```css
.float {
  animation: float 6s ease-in-out infinite;
}

@keyframes float {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-15px); }
}

/* Stagger floating elements for organic feel */
.float:nth-child(2) { animation-delay: -2s; }
.float:nth-child(3) { animation-delay: -4s; }
```

**Apply:**
- Ambient animations must be **slow** (6s+ cycle) and **subtle** (small movement range).
- Always add `animation-play-state: paused` on hover for marquees — accessibility requirement.
- Gradient animations: `background-size: 400% 400%` creates smooth panning without visible seams.
- **Never auto-play ambient motion if `prefers-reduced-motion` is set.** Replace with static state.

---

## 11. Number & Data Animation

Counting, rolling, and transitioning numerical values.

### Animated Counter

```javascript
function animateCount(element, target, { duration = 2000, startValue = 0 } = {}) {
  const start = performance.now();

  function tick(now) {
    const elapsed = now - start;
    const progress = Math.min(elapsed / duration, 1);

    // Ease-out curve
    const eased = 1 - Math.pow(1 - progress, 3);
    const current = Math.round(startValue + (target - startValue) * eased);

    element.textContent = current.toLocaleString();

    if (progress < 1) requestAnimationFrame(tick);
  }

  requestAnimationFrame(tick);
}
```

**Apply:**
- Count animations should **ease-out** — fast at start, settling at the end.
- Duration: **1.5–2.5s** for large numbers, **0.8–1.2s** for small counters.
- Trigger on scroll reveal, not on page load — users should see the animation.
- Format numbers during animation (`toLocaleString()`) — "1,234" not "1234".

---

## 12. Path & Morphing Animation

SVG path animations for logos, icons, and decorative elements.

### SVG Line Drawing

```css
.draw-path {
  stroke-dasharray: 1000;
  stroke-dashoffset: 1000;
  transition: stroke-dashoffset 2s ease-in-out;
}

.is-visible .draw-path {
  stroke-dashoffset: 0;
}
```

```javascript
// Calculate exact path length for precise animation
const path = document.querySelector('.draw-path');
const length = path.getTotalLength();
path.style.strokeDasharray = length;
path.style.strokeDashoffset = length;
```

**Apply:**
- Always calculate `getTotalLength()` dynamically — don't hardcode dash values.
- Line drawing works for **simple paths**: logos, icons, borders, underlines.
- Duration: **1–3s** depending on path complexity. Short paths = shorter duration.
- Combine with fill-opacity animation for a "draw then fill" effect.

---

## Technology Decision Guide

Choose the right tool for the pattern:

| Pattern | Best Implementation | Why |
|---|---|---|
| Scroll reveals (threshold) | IntersectionObserver + CSS transitions | Lightweight, performant, no dependencies |
| Scroll-linked (progress) | CSS `animation-timeline: scroll()` | Compositor thread, zero JS |
| Spring physics | JS (custom or Motion/GSAP) | CSS can't do true springs |
| Text split | JS + CSS transitions | Need DOM manipulation for splitting |
| Stagger | CSS custom properties + JS index | CSS handles timing, JS handles indexing |
| Layout animation | FLIP technique or View Transitions API | Must use `transform`, not layout properties |
| Mount/unmount | `@starting-style` + `allow-discrete` | Modern CSS, no JS needed |
| Drag/gesture | Pointer Events + JS | Requires continuous input tracking |
| Magnetic/cursor | JS mouse tracking | Desktop-only, needs real-time calculation |
| Marquee/ambient | Pure CSS `@keyframes` | No JS needed, infinite loop |
| Number counting | JS + `requestAnimationFrame` | Needs math per frame |
| SVG path drawing | CSS transitions + JS for path length | Simple, widely supported |

**The rule:** Use CSS when you can. Use JS only when CSS can't express the
animation. CSS animations run on the compositor; JS runs on the main thread.

---

## Review Checklist

After implementing any animation pattern, verify:

1. **Pattern match** — Am I using the right technique for this animation type?
2. **Performance** — Am I only animating `transform` and `opacity`? Is JS animation using `requestAnimationFrame`?
3. **Reduced motion** — Does every animation respect `prefers-reduced-motion`?
4. **One-shot reveals** — Do scroll reveals trigger once and unobserve?
5. **Stagger cap** — Does the total stagger duration stay under 1.2s?
6. **Spring tuning** — Do spring parameters match the brand tone (snappy vs bouncy)?
7. **Text a11y** — Is split text still accessible to screen readers?
8. **Touch fallback** — Do cursor/magnetic effects degrade gracefully on mobile?
9. **Scroll-linked** — Am I using `animation-timeline: scroll()` instead of JS scroll listeners where possible?
10. **Consistency** — Do similar elements use the same animation pattern?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| `scroll` event listener for reveal animations | Main-thread thrashing, janky scrolling | Use IntersectionObserver |
| Re-animating on scroll-back | Feels glitchy, not premium | Unobserve after first trigger |
| Stagger > 1.5s total | Last items feel like bugs, not animation | Reduce delay or animate in batches |
| Spring with damping < 5 | Infinite oscillation, never settles | Increase damping or add settle threshold |
| Splitting body text into characters | Unreadable, distracting, inaccessible | Character split for hero headlines only |
| Parallax on text content | Unreadable while scrolling | Parallax decorative elements only |
| Heavy JS animation on scroll (GSAP ScrollTrigger) when CSS can do it | Unnecessary main-thread cost | Use `animation-timeline: scroll()` for simple effects |
| No `prefers-reduced-motion` on any pattern | Accessibility violation | Every pattern needs a reduced-motion fallback |
| Magnetic effects on touch devices | Broken — no hover/mousemove | Feature-detect and disable |
| Animating `width`/`height` for layout transitions | Layout thrashing, stuttering | Use FLIP technique with `transform` |
