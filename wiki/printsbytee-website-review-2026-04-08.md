---
title: PrintsbyTee Website Review — 2026-04-08
created: 2026-04-08
updated: 2026-04-08
tags: [printsbytee, review, ux, conversion, e-commerce]
related: [[printsbytee-mvp]], [[q2-goals-2026]]
---

# PrintsbyTee Website Review — 2026-04-08

**Reviewer:** Frontend Dev Subagent
**Live URL:** [printsbytee.co.uk](https://printsbytee.co.uk)
**Project:** `~/projects/printsbytee` (Next.js + TypeScript + Tailwind + shadcn/ui + Framer Motion)
**Full report:** `/root/sule-memory/printsbytee-review-2026-04-08.md`

---

## Overall Assessment

PrintsbyTee is a visually distinctive brand with strong identity — deep emerald/gold palette, elegant Playfair typography, smooth Framer Motion animations throughout. However, the site has **zero actual product photography** (all products use CSS gradient placeholders), **no e-commerce cart/checkout system** (all purchasing is manual enquiry via email/WhatsApp), and missing several critical trust-building pages (shipping, returns, privacy). The conversion flow is severely compromised by friction-heavy "Enquire to Order" CTAs and non-existent trust signals. These issues are not cosmetic — they directly prevent sales.

---

## 🔴 Priority 1 — Must Fix

| # | Issue | File(s) | Effort |
|---|-------|---------|--------|
| 1 | **Gradient placeholders instead of real photos** — every product renders CSS gradients. Customers cannot buy what they cannot see at £185-205 price points | `data/products.json`, `lib/products.ts` | 🔴 High |
| 2 | **No cart/checkout — enquiry-only flow** — "Enquire to Order" is a `mailto:` link. No Add to Cart, no cart drawer, no payment integration | `ProductCTA.tsx`, `ProductInfo.tsx` | 🔴 High |
| 3 | **Size selector error on page load** — "Please select a size" shows immediately before user interacts | `ProductSizeSelector.tsx:26` | 🟢 Low |
| 4 | **Mobile nav hides Shop behind 2 taps** — hamburger → drawer → products. Primary conversion path buried | `Header.tsx:73-81`, `MobileMenu.tsx` | 🟢 Low |
| 5 | **Filter buttons under 44px touch target** — "All", "Laura Set" etc. too small on mobile | `ProductGrid.tsx:22-34` | 🟢 Low |

### Fix for Size Selector Error (Quick Win)

```tsx
// In ProductSizeSelector.tsx — add hasInteracted state
const [hasInteracted, setHasInteracted] = useState(false);

// Show error only after interaction, not on page load
{hasInteracted && !selectedSize && (
  <p className="text-sm text-red-500">Please select a size</p>
)}
```

---

## 🟡 Priority 2 — Should Fix

| # | Issue | File(s) | Effort |
|---|-------|---------|--------|
| 6 | Missing policy pages (Shipping, Returns, Privacy, Terms) — no trust signals | `Footer.tsx` | 🟢 Low |
| 7 | No payment method icons in footer (Visa, Mastercard, PayPal) | `Footer.tsx` | 🟢 Low |
| 8 | WhatsApp tooltip text hidden on mobile (no hover) | `WhatsAppButton.tsx:45` | 🟢 Low |
| 9 | Newsletter form commented out — one line to uncomment | `app/page.tsx:11` | 🟢 Low |
| 10 | "You May Also Like" section renders empty space when < 3 products in category | `[slug]/page.tsx:50-60` | 🟢 Low |
| 11 | Bento grid shows only 5 of 20 products — 15 products never shown on homepage | `BentoGrid.tsx:88` | 🟢 Low |
| 12 | No quick-view on collection page cards | `ProductCard.tsx` | 🟢 Low |
| 13 | "Notify Me" badge too small on out-of-stock products | `ProductCard.tsx:49-53` | 🟢 Low |
| 14 | Contact page email/WhatsApp not clickable links | `ContactInfo.tsx` | 🟢 Low |

---

## 🔵 Priority 3 — Nice to Have

| # | Issue | Effort |
|---|-------|--------|
| 15 | Custom Orders text link hard to tap on mobile | Low |
| 16 | Unused `Hero.tsx` component in codebase | Trivial |
| 17 | No "Best Seller" / "New Arrival" badges on product cards | Low |
| 18 | Sizing guide accordion has no actual measurement data | Medium |
| 19 | No Instagram/social feed integration | Low-Medium |
| 20 | No breadcrumb back to shop on mobile product pages | Low |

---

## Key Wins (Easiest → Hardest)

1. **Uncomment Newsletter** — 1 line, high ROI (`app/page.tsx:11`)
2. **Fix size selector error** — 5 lines of code (`ProductSizeSelector.tsx`)
3. **Add Shop button to mobile header** — 1 button component
4. **Add payment icons to footer** — drop in SVG icons
5. **Make WhatsApp label visible on mobile** — CSS change
6. **Create policy pages** — 3 simple content pages
7. **Fix bento grid** — increase product count from 5 to 8+
8. **Add cart + checkout system** — biggest impact but highest effort

---

## Related Docs

- [[printsbytee-mvp]] — Original MVP spec and shipped features
- [[q2-goals-2026]] — Q2 Goals including PrintsbyTee growth tasks
- `/root/sule-memory/printsbytee-payment-plan.md` — Payment integration research
- `/root/sule-memory/printsbytee-payment-architecture.md` — Payment integration architecture

---

*Review generated: 2026-04-08 · Agents: frontend-dev (review), researcher-agent (payments), software-architect (payments)*
