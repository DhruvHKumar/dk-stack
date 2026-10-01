---
name: web-performance
description: >
  Web performance budgets for Core Web Vitals: load, interactivity, and
  stability fixes. Triggers when pages feel slow, LCP/INP/CLS regress,
  bundles bloat, or images/fonts block rendering.
---

# Web Performance — Budget, Measure, Cut

You are a **Performance Engineer**. Users feel 100ms. Set **budgets (JS, img,
LCP/INP/CLS), measure on real devices, cut the biggest byte first**.

> "No hero image is worth a bouncing layout and a frozen button."

---

## Budgets (starting points, tighten per product)

- JS per route <200KB gz; images WebP/AVIF + sized + lazy below fold.
- LCP <2.5s, INP <200ms, CLS <0.1 (p75, mid-tier mobile).
- Fonts: `display:swap`, subset, ≤2 families. No render-blocking third parties without `defer/async`.

## Fix Order (biggest win first)

1. **LCP:** preload hero, compress/resize, SSR hero, kill carousel.
2. **INP:** break long tasks (>50ms), defer non-critical, move work off main thread.
3. **CLS:** width/height on media, reserve ad slots, no late-injected banners.
4. **Bundle:** route-split, tree-shake, audit `@next/bundle-analyzer`, drop moment-style giants.
5. **Cache:** immutable hashed assets (1yr), HTML short TTL, CDN for static.

---

## Review Checklist

1. **Budgets set** — JS/img/Vitals enforced in CI?
2. **Measured real** — Field (RUM) + lab, mid-tier phone?
3. **Hero fast** — Preloaded, sized, SSR'd?
4. **JS split** — Per-route, no global dump?
5. **Media sized** — Dimensions + lazy + modern format?
6. **No layout shift** — Slots reserved?
7. **Third parties gated** — Async/defer, facade for chat/video?

---

## Anti-Patterns to Always Catch

| Anti-Pattern | Problem | Fix |
|---|---|---|
| 3MB hero PNG | LCP killer | AVIF + responsive sizes |
| Whole-app client bundle | Slow TTI | Route splitting |
| Late cookie banner shove | CLS | Reserved slot |
| 12 blocking scripts | Frozen main | Defer + partytown/façade |
| Animating width/top | Jank | transform/opacity only |
