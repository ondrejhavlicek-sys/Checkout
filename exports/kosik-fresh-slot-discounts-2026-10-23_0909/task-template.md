# Task template — Slevy na čas doručení v pokladně (Košík Plus, klub Benjamínek, voucher)

Export version: 2026-10-23_0909 (first iteration: the three Figma features, before the user's "enhancements")

> **⚠️ Key information is missing.** Fields marked `MISSING ⚠️` were not provided during the session
> and were not guessed. The recipient should reach out to the design department to finalize them
> before treating this as a build spec.

## Key information

### Problem statement

The delivery discount that the customer gets (Košík Plus, klub Benjamínek, voucher na dopravu zdarma)
is not visible where the customer chooses a delivery time. In the Figma "Verze k diskuzi" three
screens are proposed:

1. **Pokladna before a time is chosen** — shows the active Košík Plus package (logo, "Výhodná doprava
   ještě 17 dní", validity bar; already in production) and the delivery tile "Vyberte čas doručení /
   Už dnes od 13:00".
2. **Výběr času doručení (drawer)** — every window shows the price before the reduction (struck
   through, like the product box) and a light-green price tag with the reason of the discount as an
   icon (crown = Košík Plus, bear = klub Benjamínek, voucher = voucher na dopravu zdarma).
3. **Pokladna after a time is chosen** — the delivery tile shows the new price, the old price, the
   percentage of the discount and the reason ("−50 % Díky Košík Plus ušetříte 30 Kč").
   Returning from the drawer gives visible feedback on the tile.

Why the problem exists / who is affected: MISSING ⚠️ (no research, complaints or analytics were
provided).

### Value

- _For the customer_ — The customer sees what the delivery really costs, what it would cost
  without the discount and why they get it — at the moment they choose the time.
  (Derived from the Figma frames; no customer research was provided: MISSING ⚠️.)
- _For the business_ — MISSING ⚠️ (not stated).

### Measurement of success

How will we know that this problem solution succeeded or failed?

- Customer-wise (usability): MISSING ⚠️ — no concrete outcome was agreed (e.g. fewer support
  contacts about delivery price, higher share of customers who pick a discounted window; no
  baseline, target or period given).
- Business-wise: MISSING ⚠️ — no metric, baseline, target, owner or period given.

How will customer find out about this? (when, who, how, for how long): MISSING ⚠️ — the feature
itself informs at the point of choice (drawer + tile). No other communication was specified.

### Limitations

Known limitations:

- Prices and discount rules in the prototype are **assumptions** taken from the Figma and not confirmed:
  30 min 59 → 29 Kč, priority 79 → 39 Kč, 1 h 39 Kč → Zdarma, 2 h Zdarma (Plus). The same rule is
  used for Benjamínek (assumption) and a free-delivery voucher makes every window free (assumption).
- The discount percentage is shown rounded **down to a multiple of 5 %** to match "−50 %" in the Figma
  (the real share is 50.8 % for 30/59). The rounding rule for production is MISSING ⚠️.
- The length of the Košík Plus package (the validity bar) is unknown — the bar uses 17 of 33 days
  as a demo value. MISSING ⚠️.
- The crown / bear / voucher icon carries the reason **without a text label** in the drawer (as in the
  Figma). The accessibility guideline says an icon must always be paired with text; this needs sign-off
  from the design department. The prototype adds a visually hidden text for screen readers.
- Backend contract (where the base price, the discount price, the percentage and the reason come
  from, and what happens when several reasons apply at once): MISSING ⚠️.

### Requirements

Known requirements (from the Figma and the brief):

1. Pokladna shows the Košík Plus status (logo, days left, validity bar) above the delivery tile when
   the customer has an active package.
2. Delivery tile without a chosen time: "Vyberte čas doručení" + earliest delivery ("Už dnes od 13:00").
3. Drawer: every window with a reduced price shows the price before the reduction (subdued text,
   strikethrough — same style as the crossed price in the product box) and the price in a light-green tag
   with the reason icon (crown, bear, voucher).
4. Windows that are free without any discount show "Zdarma" in the green tag without an icon.
5. Delivery tile with a chosen time: time, new price + old price, a bar with the discount percentage and
   the reason ("Díky Košík Plus ušetříte 30 Kč").
6. Returning from the drawer, the tile visibly reflects the selection: the tile pulses (DS pulse
   animation), the discount bar slides in, the result is announced to assistive technology, and then the tile
   stays in the stable selected state.
7. A state without a discount keeps the current plain prices.
8. Mobile first (393 / 375 px), works at 1280 px.
9. Czech copy, vykání, no humor.

Anything beyond this (the user announced "enhancements" for a next round): MISSING ⚠️ — not yet specified.

### Resources

- Figma: https://www.figma.com/design/RfcmogmHScG4IusIONEwe0/Convenience-fee---Packaging-fee--KO%C5%A0%C3%8DK-PLUS-?node-id=3079-43042
  (section "Verze k diskuzi" 3079:43042; frames "Cash desk page" 3079:37990, "Slot selection" 3078:36905,
  "Cash desk page" 3079:38485)
- Prototype: `kosik-fresh-slot-discounts.html` (same folder). Start from the kit template
  `dist/spa-standalone.html` (basket → cashdesk), Fresh design system
- Screenshots and Figma references: `comparison/`
- Research, analytics, copy approval, backend spec: MISSING ⚠️

## More information and notes

See `notes.md` (how to open the prototype, deviations from the Figma, design-system gaps, what was not tested).
