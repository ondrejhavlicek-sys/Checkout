# Task Template — Vnímání úspory s Košík Plus (doprava, celková úspora vs. cena balíčku, potvrzení nákupu)

Export version: **2026-10-21_1353**
Prototype file: `kosik-plus-savings-perception.html` (self-contained, open in a browser). Routes: `#/basket` -> `#/cashdesk` -> `#/objednavka-odeslana` (new confirmation page; reached by choosing a payment method and pressing the pay button). A dark chip in the bottom-left corner (prototype-only) cycles three scenarios: Plus customer whose Plus has already paid back / Plus customer who is not yet paid back / customer without Plus. `?plus=0` starts without Plus, `?saved=N` sets the demo cumulative saving.

> Iteration 2 of `exports/kosik-plus-delivery-savings-2026-10-14_1007`. Source: answers from the requester in chat (no PRD). Fields not confirmed by business are marked `MISSING ⚠️`. Recipient: reach out to the design department to finalize them.

---

## Key information

### Problem statement

A customer who pays for Košík Plus does not see, at the moments that matter, what the package gives them back. The earlier iteration made the delivery saving of a single purchase visible, but it was a small plain line and said nothing about whether the package pays off over time, or what the customer got after the purchase. We want Plus customers to perceive that the package is worth its price, so that they renew it.

### Value

- **For the customer** — Sees in checkout how much Plus saves on this purchase, how that adds up against what Plus costs, and gets the same confirmation after the purchase. No need to calculate whether the package pays off.
- **For the business** — Higher perceived value of the package, which should support Plus renewal and reduce churn (see metric). `MISSING ⚠️`: the requester named renewal/churn as the goal; no business case, volumes or financial target were provided.

### Measurement of success

- **Primary business metric (chosen by the requester): Plus renewal rate / churn of Plus customers.**
  - Baseline: `MISSING ⚠️`
  - Target: `MISSING ⚠️`
  - Measurement period / cohort (e.g. customers who saw the new summary at least N times before the renewal date): `MISSING ⚠️`
  - Owner: `MISSING ⚠️`
- **Customer-wise (usability)** — `MISSING ⚠️`, suggestion: in a short test or post-purchase question, customers can say correctly (a) how much they saved on delivery in this purchase and (b) whether Plus has paid back so far.
- Supporting signals (suggested, not confirmed): support contacts of the type "was my Plus discount applied?", share of Plus customers who choose a paid (exact/priority) slot.

**How the customer finds out** — In the prototype: in the order summary (card, both pages), in the Delivery tile and slot list (from iteration 1), in the header "Ušetříte" pill, and on the new confirmation page. Not designed (`MISSING ⚠️`): e-mail confirmation, app notification, order detail, first-time explanation, how long the card stays visible.

### Limitations

- **All prices and figures are prototype assumptions, not production pricing** (`MISSING ⚠️`, need confirmation):
  - Standard slot regular fee **49 Kč** below the free-delivery threshold **1 199 Kč**; Plus **0 Kč**.
  - Exact / priority slots regular **89 / 99 Kč**; Plus **50 Kč** less.
  - Plus package price **100 Kč** (taken from production config `convenienceFee`). **Duration and billing period of the package are unknown** — the copy therefore says "Plus stojí 100 Kč" and avoids "za měsíc / za rok".
- **Cumulative savings are DEMO DATA, not real.** "Dosud ušetřeno" before the purchase is an invented round number (310 Kč = already paid back, 20 Kč = not yet), 6 previous purchases. In production this needs a backend value: sum of Plus savings since the membership started (or since the last payment — `MISSING ⚠️`, which period counts against the package price?).
- Only the **delivery saving** is counted. Product / Plus-price discounts, vouchers and other Plus benefits are not included (not in scope per the requester). If other benefits exist, the cumulative number would be higher.
- The cumulative saving at checkout is a projection ("with this purchase"); if the purchase is changed or cancelled it changes. On the confirmation page it is stated as fact.
- The basket page shows the savings card only after a delivery time is chosen (no slot = no delivery price to compare).
- The confirmation page is a **new prototype page** built from DS components; it is not a capture of a production page. The e-mail "Potvrzení vám pošleme e-mailem" is only copy; no e-mail is designed.
- Prototype data is one captured cart (10 products, 843,63 Kč), one address (Praha 6), fixed slots (the captured slots carry a past date, so the confirmation shows only the weekday, as the Delivery tile does).
- Not covered (explicitly out of scope): expiring Plus, Plus purchase flow, product discounts, additional order, anonymous customer.

### Requirements

1. **Summary card (basket and checkout; popover on mobile)** — for a Plus customer with a chosen delivery time, one green card under the total:
   1. Large figure: "S Košík Plus ušetříte **49 Kč**" + "na dopravě v tomto nákupu".
   2. Separated block "Celková úspora s Plus" with a progress bar (role `progressbar`, with a text value) and one of three states:
      - **Already paid back** (savings before this purchase >= package price): "Košík Plus se vám už vrátil" / "S tímto nákupem ušetříte celkem 359 Kč. Plus stojí 100 Kč."
      - **This purchase pays it back**: "Tímto nákupem se vám Košík Plus vrátí" / "Ušetříte celkem 119 Kč. Plus stojí 100 Kč."
      - **Not yet**: "Ušetříte celkem 69 Kč ze 100 Kč" / "Ještě 31 Kč a Plus se vám vrátí."
   3. The saving is not subtracted from the total again (the total already contains the Plus price). It is still added to the header "Ušetříte" figure (iteration 1).
2. **Confirmation page after the purchase** (`#/objednavka-odeslana`, checkout gray background, white tiles):
   1. Success message "Nákup je odeslán".
   2. Delivery tile: delivery time, address, transport row (strike-through regular price + "Zdarma" tag), "Celkem k platbě".
   3. Plus customer: green card in the past tense — "S Košík Plus jste ušetřili **49 Kč**" and the same three cumulative states ("Od začátku členství jste ušetřili celkem 359 Kč za 7 nákupů. Plus stojí 100 Kč.").
   4. One primary button "Zpět na úvod".
3. **Delivery tile and slot list** — as in iteration 1 (strike-through regular price, Plus price, saving stated in words).
4. **Non-Plus customer (secondary, unchanged in spirit)** — plain gray text hints only: on the Delivery tile, in the summary, and on the confirmation page ("S Košík Plus byste na dopravě v tomto nákupu ušetřili 49 Kč."). No card, no progress, no CTA. A link target for the hint is `MISSING ⚠️`.
5. **Copy rules** — Czech, formal address (vykání), no humor, no manipulation: the progress is stated factually; no exclamation marks; the vocabulary from the glossary (Nákup, Doprava, Doručení). Never show a negative framing ("ještě jste nevydělali").
6. **Accessibility** — progress bar is not the only carrier of the information (text beside it, `aria-valuetext`); success is not signaled by color alone (icon + text); text colors from the design tokens only. Verified at 375 px and 1280 px.
7. Mobile first. Alignment with product vision — `MISSING ⚠️`.
8. Backend needs (for the frontend team): cumulative Plus saving to date, Plus package price and period, regular and Plus price per slot, number of purchases with Plus.

### Resources

- Brief from the requester (chat). No PRD, no Figma, no analytics provided — `MISSING ⚠️`.
- Earlier export: `exports/kosik-plus-delivery-savings-2026-10-14_1007` (notes.md, task-template.md).
- Kit docs used: GUIDE.md, design principles, page construction (checkout = gray background), accessibility, copywriting (glossary), colors / typography / spacings, buttons, `checkout-summary`, `checkout-delivery`, `checkout-shell`, `prototype-file`.
- Starting file: `kosik-plus-delivery-savings.html` (SPA bundle from `dist/spa-standalone.html`).

### More information and notes

See `notes.md` in this bundle (decisions, assumptions, what was changed, open questions).
