# Notes for implementation and design

Export version: 2026-10-14_1007

## ⚠️ Key information is missing

The brief had no success metric, no real pricing rules and no information about who owns the Plus package. Fields marked `MISSING ⚠️` in `task-template.md` need to be finalized. **Please reach out to the design department** before treating this as a build spec.

## How to look at the prototype

- Open `kosik-plus-delivery-savings.html` in a browser (self-contained, no server needed). The page waits for the tab to be visible before it mounts; in a background tab it stays blank until you switch to it.
- It opens on the home page. The cart is pre-filled with the 10-product demo cart. Click the cart / "Doprava a platba", or go to `#/basket`, then `#/cashdesk`.
- Bottom-left dark chip = **prototype-only** scenario switch (Plus ↔ without Plus). Not part of the design. `?plus=0` starts as a non-Plus customer.

## Decisions and why

- **Saving shown where the price is decided** (slot list, tile, summary), not in a separate banner at the top of the page. Principle: do not make the customer hunt for the information.
- **Strike-through + highlighted Plus price** on slots and summary row, the same pattern customers know from product prices. The saving is also stated in words ("ušetříte 49 Kč") because a struck-through number alone is easy to miss on a small slot tile.
- **Plus customer: green tint, once per place.** Non-Plus customer: gray plain text only, so it does not read as an ad in a flow where the goal is paying.
- **The saving is not subtracted from the total again** — the total already contains the Plus delivery price; the callout only explains it.
- Free-delivery progress is treated as reached for Plus; without Plus it is computed from the real threshold (1 199 Kč).

## What was changed in the file (for developers)

Based on `dist/spa-standalone.html`. Everything outside `PROTO:` regions is untouched; markers are kept.

- New `<script>` block at the start of `PROTO:EDIT app` (`PLUS-DELIVERY`): `window.PlusDelivery` — the scenario switch, the assumed prices and `decorate()` which adds `regularPrice`, `plusPrice`, `price` to each slot of the raw `/transport/windows` payload. **This is where real pricing would come from the backend.**
- `PROTO:COMPONENT checkout-delivery`: slot price (strike-through), Delivery tile (preview / chosen slot / non-Plus hint), drawer banner.
- `PROTO:COMPONENT checkout-summary`: transport row and the savings callout (reads `cartPrices.transportRegular`, `cartPrices.transportPlusSaving`).
- Basket and cashdesk page code (`PROTO:EDIT app`): decorated windows passed to `<timeslots-management>`, regular price kept on the chosen transport, savings added to the header total savings, free-delivery progress flag, demo cart seed (basket-payloads block).
- Search for `PLUS-DELIVERY` to find every change.

## Open questions

1. Real rules: delivery price per slot type with and without Plus; does the discount depend on cart value; does Plus cover all days/zones?
2. Show the saving per order only, or also cumulative ("Od začátku jste s Plus ušetřili…") and against the package price?
3. What does a customer whose Plus expires soon see?
4. Where does the non-Plus hint lead (Plus offer page / purchase modal)? Production already has a Plus purchase modal in the delivery section, not part of this prototype.
5. What is the success metric and who owns it?
6. Should a saving also be shown after the order (confirmation, email, order detail)?
