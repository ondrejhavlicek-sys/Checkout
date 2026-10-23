# Task Template — Úspora na dopravě s Košík Plus v košíku a pokladně

Export version: **2026-10-14_1007**
Prototype file: `kosik-plus-delivery-savings.html` (same bundle). Open it in a browser; start at `#/basket`, continue to `#/cashdesk`. A dark chip in the bottom-left corner (prototype-only) switches between a Plus and a non-Plus customer; `?plus=0` in the URL starts as non-Plus.

> Source: a short brief from the requester (no PRD). Nothing here was confirmed with business, so most fields below are marked `MISSING ⚠️`. Recipient: reach out to the design department to finalize them.

---

## Key information

### Problem statement

A customer who has bought the Košík Plus loyalty package pays less for delivery, but the cart and checkout do not show it. The customer cannot see what Plus gave them, and a customer without Plus cannot see what they are missing. We want to show the delivery saving where the delivery price is decided: the Delivery tile, the slot list and the order summary.

### Value

- **For the customer** — Sees, at the moment of choosing a time, that the package pays off: the regular price struck through next to the Plus price, and a saving stated in Kč. No need to calculate or trust that the discount was applied.
- **For the business** — `MISSING ⚠️` (hypothesis, to confirm): higher perceived value of the package, so higher Plus renewal and fewer cancellations; for customers without Plus, a concrete reason to buy it.

### Measurement of success

`MISSING ⚠️` — the requester did not name a success metric. Candidate metrics for the design department to confirm:

- **Customer-wise (usability)** — customers can correctly say how much they saved on delivery (short post-checkout question or usability test); fewer support contacts of the type "was my Plus discount applied?".
- **Business-wise** — Plus renewal rate / churn of Plus customers; Plus purchase rate among customers who saw the non-Plus hint in checkout; share of Plus customers choosing a paid (exact/priority) slot.

**How the customer finds out** — `MISSING ⚠️`. In the prototype the saving is visible in context only (cart tile, slot drawer, summary, header "Ušetříte" pill). Not designed: email/app communication, how long the hint stays, a first-time explanation.

### Limitations

- **All prices are prototype assumptions**, not production pricing (all `MISSING ⚠️`, need confirmation):
  - Standard slot: regular fee **49 Kč** below the free-delivery threshold **1 199 Kč**, free above it. Plus: **0 Kč**.
  - Exact (30-minute) and priority slots: regular price as in the captured data (**89 / 99 Kč**). Plus: **50 Kč less** (39 / 49 Kč).
  - The captured production data had no regular delivery fee on standard slots, so 49 Kč was added to make the saving visible.
- The real Plus package is a paid service (production config: `convenienceFee`, 100 Kč). The prototype shows the saving per order and does **not** net it against the package price or show a running total over time.
- Prototype data is one captured cart (10 products, 843,63 Kč), one address (Praha 6) and fixed days of slots.
- The basket and the checkout use two separate captured slot lists, so the slot chosen in the basket is not carried into the checkout (checkout opens with its own captured slot).
- Whether Plus also affects thresholds, vouchers, free-delivery progress is unknown. The prototype only treats the free-delivery milestone in the progress bar as reached for Plus customers.
- Not covered: purchase flow of Plus, expired or about-to-expire Plus, additional order, anonymous customer (slot prices are hidden for them in production).

### Requirements

1. **Slot list (drawer)** — every slot whose price Plus lowers shows the regular price struck through next to the Plus price (free = "Zdarma" tag). A banner above the days explains the benefit in one sentence.
2. **Delivery tile (cart and checkout)**
   - No slot chosen: standard delivery shown as `~~49 Kč~~ Zdarma` with "Standardní doručení s Košík Plus".
   - Slot chosen: `~~49 Kč~~ Doprava zdarma` and "S Košík Plus ušetříte 49 Kč".
3. **Order summary** — transport row shows the struck-through regular price next to the Plus price; under the total, one callout "Košík Plus vám šetří X Kč na dopravě." Savings are not subtracted again from the total (the total already contains the Plus price).
4. **Running total of savings** — the delivery saving is added to the existing "Ušetříte" figure in the header on both pages.
5. **Free-delivery progress bar** — for a Plus customer the free-delivery milestone is shown as reached whatever the cart value.
6. **Non-Plus customer (secondary, subtle)** — plain text only: a one-sentence hint in the slot drawer, a hint on the Delivery tile ("S Košík Plus byste ušetřili 49 Kč") and in the summary. No strike-through, no highlight, no extra CTA. Text-only hints; a link to the Plus offer is `MISSING ⚠️`.
7. Mobile first. Verified at 375 px and 1280 px. Uses design-system tokens, `price-highlight-tag` and the green tint used elsewhere for savings. Czech copy, formal address (vykání), no humor.
8. Alignment with product vision — `MISSING ⚠️`, to be confirmed by the design department.

### Resources

- Brief from the requester (chat); no PRD, no Figma, no analytics were provided — `MISSING ⚠️`.
- Kit docs used: `docs/consumer/GUIDE.md`, design principles, page construction (checkout = gray background), copywriting, `checkout-delivery`, `checkout-summary`, `checkout-shell`.
- Starting template: `dist/spa-standalone.html` (basket → cashdesk with a shared cart).

### More information and notes

See `notes.md` in this bundle (what was changed in the file, open questions).
