# Notes for implementation and design

Export version: 2026-10-21_1353 (iteration 2 of 2026-10-14_1007)

## ⚠️ Key information is missing

Missing / unconfirmed (marked `MISSING ⚠️` in `task-template.md`): baseline, target and owner of the renewal/churn metric; real pricing rules; Plus package duration and billing period; which savings period counts against the package price; business case; how customers are informed outside the checkout; link target of the non-Plus hint. **Please reach out to the design department** before treating this as a build spec.

## How to look at the prototype

- Open `kosik-plus-savings-perception.html` in a browser (self-contained). If it stays blank, bring the tab to the foreground (the page waits for rendering frames before it mounts).
- Flow: `#/basket` -> choose "Doprava a platba" -> `#/cashdesk` -> choose a payment method (e.g. "Hotově či stravenkami u kurýra") -> pay button -> `#/objednavka-odeslana`. The confirmation page also opens directly by URL with a demo order.
- On mobile the order summary opens by tapping the total in the header; on desktop it is the right rail.
- Dark chip bottom-left = **prototype-only** scenario switch, not part of the design. Cycles: Plus, demo savings 310 Kč (already paid back) -> Plus, demo savings 20 Kč (not yet; this purchase gives 69 of 100 Kč) -> without Plus. URL: `?plus=0`, `?saved=70` (that value makes "this purchase pays it back").

## Decisions and why

- **One card, not three banners.** The delivery saving (large figure) and the cumulative result live in a single card in the summary, so the customer reads one thing at one place. Hierarchy: this purchase first (concrete, in Kč), the cumulative below it.
- **The summary card has a larger figure than in iteration 1.** The earlier plain one-line callout was easy to miss, especially on mobile where the summary sits in a popover. Delivery tile and slot list stay as they were (the saving is stated there at the moment of choice).
- **Cumulative against the package price, three honest states.** Already paid back / this purchase pays it back / not yet. Wording is factual ("Ještě 31 Kč a Plus se vám vrátí"); the "not yet" state does not blame the customer and does not push to buy more. The progress bar always has its text next to it.
- **Package price stated as "Plus stojí 100 Kč"**, not "za měsíc/rok", because the period is unknown.
- **Confirmation page is minimal and keeps the same card in the past tense**, so the customer sees the result of the purchase just completed. It follows the checkout rule (gray page, white borderless tiles). No e-mail design.
- **Non-Plus stays a quiet text line** (requester: Plus customers are the primary audience).
- **Cumulative demo numbers are round and invented** (310 / 20 Kč, 6 purchases) and labelled as demo in the scenario chip and in the code (`DEMO DATA, NOT REAL`).

## Assumptions I had to make (not answered by the requester)

1. Pricing rules, thresholds and the 100 Kč package price are carried over from iteration 1 (prototype assumptions).
2. "Cumulative" = sum of delivery savings since the start of the membership (period unknown); only delivery counts, because product/Plus-price discounts were out of scope.
3. The confirmation page uses the vocabulary "Nákup" (not "objednávka") per the glossary; the route name is the one the prototype already used (`/objednavka-odeslana`).
4. The "Doručení" line on the confirmation shows the weekday and time only, because the captured slots carry a past date.
5. Header of the confirmation page: only the logo (no search, no cart), as it is an end-of-flow page.

## What was changed in the file (for developers)

Based on `kosik-plus-delivery-savings.html`, which is based on `dist/spa-standalone.html`. All changes are inside `PROTO:` regions. Search for `PLUS-SAVINGS` (new in this iteration) and `PLUS-DELIVERY` (iteration 1).

- `PROTO:EDIT app`, first `<script>` (`window.PlusDelivery`): new `PACKAGE_PRICE`, demo `savedBefore` / `ordersBefore`, `lastOrder`, and `cumulative(thisSaving)` returning the state (`returned` / `crossing` / `partial`), the remaining amount and the bar percentage. **This is where real values from the backend would come from.** The scenario chip now cycles three scenarios.
- `PROTO:COMPONENT checkout-summary`: `plusCard` computed and the new card markup (replaces the callout under the total).
- `PROTO:EDIT app`: new template `spa-page-confirmation` and component `SpaPageConfirmation`; route `/objednavka-odeslana`; `$spa.toConfirmation()`; in the cashdesk page `submitOrder()` stores a snapshot (delivery time, address, regular and Plus transport price, total without transport) and both pay buttons call it before navigating.

## Open questions

1. Real Plus rules: price and billing period of the package; which benefits besides delivery exist and should count as savings.
2. Which period counts for "paid back": since the first purchase, since the current payment, or per billing period? Does a renewal reset the counter?
3. What if the customer saved much more than the package price: keep showing the full bar, or switch to a multiplier ("3,6x")? (Not designed; the full bar plus the total is shown now.)
4. Where should the non-Plus hint lead (Plus offer page / purchase modal)?
5. Should the same savings appear in the confirmation e-mail, app notification and order detail? Not designed.
6. Baseline, target, owner and measurement period of the renewal/churn metric; how to attribute a renewal to this change.
7. Should the basket show the card before a delivery time is chosen (e.g. based on the cheapest slot)?
