---
name: liquid-glass
description: >
  Liquid glass design system skill that guides the implementation of Apple-style
  translucent, refractive glass effects in web interfaces. Triggers when creating
  or reviewing any UI that uses glassmorphism, frosted glass, backdrop blur,
  translucent surfaces, or the liquid glass aesthetic. Covers CSS implementation,
  SVG filter techniques, performance, accessibility, and when to use (and not use)
  the effect.
---

# Liquid Glass — Translucent Surface Design

You are a **Liquid Glass Design Specialist**. When a project calls for
glassmorphism, frosted glass, or Apple-style liquid glass aesthetics, you
ensure the implementation is **optically convincing, performant, accessible,
and used with restraint.**

Liquid glass is a premium design material. Done well, it creates depth,
elegance, and spatial hierarchy. Done poorly, it creates unreadable text,
janky performance, and visual noise.

> "Glass is a functional layer, not a decoration."

---

## Core Philosophy

Liquid glass is not about making things "look cool." It's a **spatial design
tool** that communicates hierarchy by establishing layers:

1. **Content layer** — The primary information the user is here for.
2. **Glass layer** — Functional controls (nav, toolbars, floating actions) that float above content, clearly distinct from it.
3. **Overlay layer** — Modals, popovers, and system-level surfaces that demand focus.

The glass effect tells the user: *"This element is above the content. It's a
control surface, not the content itself."*

---

## When to Use Liquid Glass

Use liquid glass **exclusively for functional surfaces** that need to feel
spatially elevated above the content:

| ✅ Use For | Why |
|---|---|
| Navigation bars & tab bars | System-level controls that persist across content |
| Floating action buttons | Elevated interactive elements above scrolling content |
| Sidebars & toolbars | Persistent tool surfaces alongside content |
| Modal sheets & bottom sheets | Layered surfaces that overlay content |
| Popovers & context menus | Temporary elevated surfaces near their trigger |
| Media controls | Play/pause overlays on video/image content |
| Widget containers | Distinct, elevated information cards |
| Status bars & notification banners | System-level information layers |

---

## When NOT to Use Liquid Glass

| ❌ Never Use For | Why |
|---|---|
| Primary content areas (lists, tables, text blocks) | Glass obscures the very content users came to see |
| Every card or container on the page | Overuse collapses the depth system — nothing feels elevated if everything is glass |
| Backgrounds behind dense text | Transparency + blur = readability disaster |
| Low-contrast or busy backgrounds | The effect becomes invisible or creates visual noise |
| Purely decorative purposes with no functional role | Adds performance cost without communicative value |
| Elements that need crisp, precise reading (data tables, code blocks) | Refraction and blur undermine precision |

**The rule of restraint:** If more than 20–30% of the visible viewport is glass,
you've overused it. Glass works because most of the interface is *not* glass.

---

## Implementation Tiers

Liquid glass ranges from simple (CSS-only) to advanced (SVG filters + JS).
Choose the tier that matches the project's needs and performance budget.

### Tier 1 — Frosted Glass (CSS Only)

The foundation. Achievable in pure CSS with excellent browser support.

```css
.glass {
  /* Semi-transparent background — the "tint" */
  background: rgba(255, 255, 255, 0.12);

  /* The blur — the core of the effect */
  backdrop-filter: blur(16px) saturate(180%);
  -webkit-backdrop-filter: blur(16px) saturate(180%);

  /* Subtle border for edge definition */
  border: 1px solid rgba(255, 255, 255, 0.18);

  /* Rounded corners feel natural on glass surfaces */
  border-radius: 16px;

  /* Soft shadow for elevation */
  box-shadow:
    0 4px 30px rgba(0, 0, 0, 0.1),
    inset 0 1px 0 rgba(255, 255, 255, 0.15);
}

/* Dark mode glass */
.glass--dark {
  background: rgba(0, 0, 0, 0.25);
  border: 1px solid rgba(255, 255, 255, 0.08);
  box-shadow:
    0 4px 30px rgba(0, 0, 0, 0.3),
    inset 0 1px 0 rgba(255, 255, 255, 0.05);
}
```

**Key parameters to tune per project:**
- `blur()` value: 8–24px depending on desired translucency (higher = more frosted, less see-through).
- `saturate()`: 120–200% — boosts the vibrancy of the blurred content. Prevents the "washed out" look.
- Background alpha: 0.05–0.3 — lower = more transparent, higher = more tinted.
- Border alpha: 0.08–0.25 — defines the edge. Too high looks like a solid border; too low disappears.

---

### Tier 2 — Enhanced Glass (Specular Highlights + Depth)

Adds the characteristic "shine" and light-reactive quality.

```css
.glass-enhanced {
  /* Base glass */
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(20px) saturate(180%);
  -webkit-backdrop-filter: blur(20px) saturate(180%);
  border: 1px solid rgba(255, 255, 255, 0.15);
  border-radius: 20px;

  /* Specular highlight — the "light catching" effect */
  /* A gradient that simulates light hitting the top edge */
  background-image:
    linear-gradient(
      135deg,
      rgba(255, 255, 255, 0.25) 0%,
      rgba(255, 255, 255, 0.05) 40%,
      rgba(255, 255, 255, 0) 60%
    );

  /* Inner glow on top edge */
  box-shadow:
    0 8px 32px rgba(0, 0, 0, 0.12),
    inset 0 1px 0 rgba(255, 255, 255, 0.2),
    inset 0 -1px 0 rgba(0, 0, 0, 0.05);

  /* Smooth transitions for interactive states */
  transition: box-shadow 0.3s ease, background 0.3s ease;
}

/* Hover lifts the glass surface */
.glass-enhanced:hover {
  box-shadow:
    0 12px 40px rgba(0, 0, 0, 0.15),
    inset 0 1px 0 rgba(255, 255, 255, 0.25),
    inset 0 -1px 0 rgba(0, 0, 0, 0.05);
}
```

**The specular gradient:** The `linear-gradient` overlay simulates light hitting
the surface at an angle. Adjust the angle (135deg) to match the project's
implied light source. Keep the highlight subtle — visible but not glaring.

---

### Tier 3 — Liquid Glass (SVG Refraction)

The full optical effect — light bending through a curved glass surface.
This is the "liquid" in liquid glass.

```html
<!-- SVG filter for refraction effect -->
<svg style="position: absolute; width: 0; height: 0;">
  <defs>
    <filter id="liquid-refraction">
      <!-- Create organic distortion pattern -->
      <feTurbulence
        type="fractalNoise"
        baseFrequency="0.015"
        numOctaves="3"
        seed="2"
        result="noise"
      />
      <!-- Use noise to displace/refract the background -->
      <feDisplacementMap
        in="SourceGraphic"
        in2="noise"
        scale="12"
        xChannelSelector="R"
        yChannelSelector="G"
      />
    </filter>

    <!-- Specular lighting filter for surface shine -->
    <filter id="glass-specular">
      <feSpecularLighting
        surfaceScale="2"
        specularConstant="0.8"
        specularExponent="20"
        lighting-color="white"
        result="specular"
      >
        <fePointLight x="-5000" y="-10000" z="20000" />
      </feSpecularLighting>
      <feComposite
        in="specular"
        in2="SourceAlpha"
        operator="in"
        result="specular-masked"
      />
      <feComposite
        in="SourceGraphic"
        in2="specular-masked"
        operator="arithmetic"
        k1="0" k2="1" k3="0.4" k4="0"
      />
    </filter>
  </defs>
</svg>
```

```css
/* Apply refraction to background content behind glass */
.glass-liquid-bg {
  filter: url(#liquid-refraction);
}

/* Apply specular highlight to the glass surface */
.glass-liquid {
  filter: url(#glass-specular);
  background: rgba(255, 255, 255, 0.08);
  backdrop-filter: blur(24px) saturate(200%);
  -webkit-backdrop-filter: blur(24px) saturate(200%);
  border: 1px solid rgba(255, 255, 255, 0.12);
  border-radius: 24px;
}
```

**SVG filter parameters to tune:**
- `baseFrequency` (feTurbulence): 0.005–0.03 — lower = larger, gentler distortion; higher = finer, more chaotic.
- `scale` (feDisplacementMap): 5–20 — intensity of the refraction distortion. Start low.
- `specularExponent`: 10–40 — sharpness of the specular highlight. Higher = tighter hot spot.
- `surfaceScale`: 1–5 — how "raised" the surface appears to the light.

**⚠️ Tier 3 has significant performance implications.** See the Performance
section below.

---

## The Visual Anatomy of Glass

Every glass surface is built from these visual layers, bottom to top:

```
┌─────────────────────────────────┐
│  5. Content (text, icons)       │  ← Must be high contrast
├─────────────────────────────────┤
│  4. Specular highlight          │  ← Subtle gradient/shine
├─────────────────────────────────┤
│  3. Tint (semi-transparent bg)  │  ← Color + alpha
├─────────────────────────────────┤
│  2. Blur (backdrop-filter)      │  ← Frosting the background
├─────────────────────────────────┤
│  1. Border + Shadow             │  ← Edge definition + elevation
└─────────────────────────────────┘
       ↕ Background content shows through
```

Each layer is tunable. Projects should define glass tokens (see design-tokens
skill) for consistent application:

```css
:root {
  --glass-blur: 16px;
  --glass-saturation: 180%;
  --glass-tint-light: rgba(255, 255, 255, 0.12);
  --glass-tint-dark: rgba(0, 0, 0, 0.25);
  --glass-border-light: rgba(255, 255, 255, 0.18);
  --glass-border-dark: rgba(255, 255, 255, 0.08);
  --glass-radius: 16px;
  --glass-shadow: 0 4px 30px rgba(0, 0, 0, 0.1);
}
```

---

## Performance

Glass effects are GPU-intensive. Treat performance as a first-class concern.

### Rules

1. **`backdrop-filter` is expensive.** Each glass element requires the browser to render and blur the content behind it. Limit glass elements to **3–5 per viewport**.

2. **Animate only `transform` and `opacity`** on glass elements. Never animate `backdrop-filter`, `blur()`, or `background` — these trigger heavy repaints.

3. **Use `contain: layout style paint`** on glass elements to isolate their paint boundaries.

4. **Use `will-change: transform`** on glass elements that will animate (e.g., sliding panels). Remove it when animation completes.

5. **SVG filters (Tier 3) are the most expensive.** Reserve them for hero elements or showcase moments, not repeated list items.

6. **Test on low-end devices.** If the glass effect causes frame drops below 30fps on a mid-range phone, fall back to Tier 1 or a solid semi-transparent background.

7. **Provide a performance fallback:**
   ```css
   @supports not (backdrop-filter: blur(1px)) {
     .glass {
       background: rgba(30, 30, 30, 0.92); /* Nearly opaque fallback */
     }
   }
   ```

8. **Reduce blur radius on mobile.** Lower blur values (8–12px instead of 16–24px) can significantly improve rendering performance on mobile GPUs.

---

## Accessibility

Glass effects present unique accessibility challenges. These are non-negotiable.

### 1. Text Contrast on Glass

Glass backgrounds are inherently variable — the contrast depends on what's
behind them. This makes WCAG compliance harder.

**Apply:**
- **Always test contrast against the worst-case background.** What if the glass hovers over a white image? A bright gradient? Pure white content?
- Add a **semi-opaque backing layer** between the glass blur and the text if needed:
  ```css
  .glass-text-layer {
    /* Extra backing for text readability */
    background: rgba(0, 0, 0, 0.3); /* Dark scrim behind text */
    border-radius: 8px;
    padding: 4px 8px;
  }
  ```
- Use **text-shadow** to improve legibility on variable backgrounds:
  ```css
  .glass-content {
    text-shadow: 0 1px 3px rgba(0, 0, 0, 0.3);
  }
  ```
- **Bold text on glass surfaces.** Increased font-weight improves readability against blurred backgrounds.
- Test with the browser's accessibility inspector — automated contrast checkers may not account for the dynamic backdrop.

### 2. Reduced Transparency

Some users have `prefers-reduced-transparency` enabled. Respect it.

```css
@media (prefers-reduced-transparency: reduce) {
  .glass {
    backdrop-filter: none;
    -webkit-backdrop-filter: none;
    background: rgba(30, 30, 30, 0.95); /* Nearly solid */
  }
}
```

### 3. Reduced Motion

If glass surfaces have animated specular highlights or refractive distortions,
disable them for users who prefer reduced motion.

```css
@media (prefers-reduced-motion: reduce) {
  .glass {
    transition: none;
    animation: none;
  }
  .glass-liquid-bg {
    filter: none; /* Remove SVG refraction animation */
  }
}
```

### 4. High Contrast Mode

In forced-colors / high-contrast mode, glass effects should be replaced with
solid surfaces and visible borders.

```css
@media (forced-colors: active) {
  .glass {
    backdrop-filter: none;
    background: Canvas;
    border: 2px solid CanvasText;
  }
}
```

---

## Dark Mode vs Light Mode Glass

Glass looks and behaves differently across themes:

| Property | Light Mode | Dark Mode |
|---|---|---|
| Tint color | White with low alpha (0.1–0.2) | Black with low alpha (0.15–0.3) |
| Border | White with low alpha (0.15–0.25) | White with very low alpha (0.05–0.1) |
| Specular highlight | Bright, visible gradient | Subtle, nearly invisible |
| Background blur | Standard (16–20px) | Can be slightly higher (18–24px) |
| Saturation boost | Moderate (150–180%) | Higher (180–220%) to prevent muddiness |
| Inner shadow | Light top glow | Very subtle or none |
| Drop shadow | Subtle, light | More prominent, darker |

**Key insight for dark mode:** Glass on dark backgrounds tends to look "muddy."
Increase the `saturate()` value and consider a slightly tinted background
(e.g., `rgba(100, 130, 200, 0.08)` instead of pure black alpha) to keep it
vibrant.

---

## Composing Glass with Other Elements

### Glass + Typography
- Prefer **medium to bold weights** on glass surfaces.
- Increase font size slightly compared to solid backgrounds — blurred backdrops reduce perceived legibility.
- Limit line length on glass. Long paragraphs on translucent surfaces are fatiguing.
- Icons on glass should use a slight drop shadow or be bolder than usual.

### Glass + Interactive Elements
- Buttons on glass should have a **more opaque** background than the glass itself, creating a "glass-on-glass" layered depth.
- Input fields on glass need a solid or near-solid background — typing on translucent inputs is disorienting.
- Hover/focus states on glass elements should increase opacity or glow, not change blur.

### Glass + Images
- Glass over images creates the most dramatic effect. Ensure the image has enough color variation to make the blur visible.
- Glass over solid colors looks flat — consider adding a gradient behind the glass.
- Glass over video is stunning but expensive — test performance heavily.

### Glass + Scroll
- Glass navigation bars that stay fixed while content scrolls beneath are the canonical use case.
- Ensure the glass surface clips content cleanly — no content "leaking" through edges.
- Consider increasing blur or opacity as more content scrolls behind the glass (dynamic glass density).

---

## Review Checklist

After implementing any glass effect, verify:

1. **Purpose** — Is the glass serving a spatial/hierarchical function, not just decoration?
2. **Restraint** — Is glass used on ≤ 20–30% of the viewport? Is it reserved for functional surfaces?
3. **Readability** — Is all text on glass surfaces legible against the worst-case background?
4. **Contrast** — Does text meet WCAG AA contrast ratios even over light/variable backgrounds?
5. **Fallback** — Is there a `@supports` fallback for browsers without `backdrop-filter`?
6. **Reduced transparency** — Is `prefers-reduced-transparency` respected?
7. **Reduced motion** — Are animated glass effects disabled for `prefers-reduced-motion`?
8. **High contrast** — Does the interface work in forced-colors mode?
9. **Performance** — Does the glass effect maintain 60fps on mid-range devices?
10. **Dark mode** — Does the glass look intentional (not muddy) in dark mode?
11. **Tokens** — Are glass values (blur, tint, border, radius) defined as design tokens, not scattered magic numbers?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| Glass on every card and container | Depth system collapses — nothing feels elevated | Reserve glass for nav, toolbars, floating controls |
| Thin/light text on glass background | Unreadable against variable backdrops | Use bold weight + text shadow or backing scrim |
| No `@supports` fallback | Broken UI in unsupported browsers | Solid semi-transparent fallback |
| Animating `backdrop-filter` or `blur()` | Catastrophic performance — drops to single-digit fps | Animate only `transform` and `opacity` |
| Glass over solid white/black backgrounds | Effect is invisible — just looks like a tinted div | Use over gradients, images, or colorful content |
| `blur(50px)` or higher | Completely obscures background, expensive, no transparency benefit | Keep blur at 8–24px range |
| Ignoring `prefers-reduced-transparency` | Excludes users with visual sensitivities | Replace with near-solid background |
| Glass on data tables or code blocks | Precision content becomes unreadable | Use solid backgrounds for precision content |
| Different blur/tint values on every glass element | Inconsistent, looks like different materials | Define glass tokens and use them everywhere |
| SVG refraction on repeated list items | Massive performance hit, unnecessary | Reserve SVG filters for hero/showcase elements |
