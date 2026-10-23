# Notes for implementation and design

Export version: 2026-10-21_1437 (iteration 3: the design reworked to follow the Figma "Verze k diskuzi")

## ⚠️ Key information is missing

Missing / unconfirmed (marked `MISSING ⚠️` in `task-template.md`): baseline, target, owner and period of the renewal/churn metric; real prices (the Figma prices are used, unconfirmed); length of the Plus package and what the validity bar means exactly; period for the cumulative saving; business case; link target of the non-Plus hint. **Please reach out to the design department** before treating this as a build spec.

## How to look at the prototype

- Open `kosik-plus-savings-perception-v2.html` in a browser. If it stays blank, bring the tab to the foreground (the page waits for rendering frames before it mounts).
- Flow (same as the Figma left -> middle -> right): `#/basket` -> "Doprava a platba" -> `#/cashdesk` (no time chosen) -> tap "Vyberte čas doručení" (drawer) -> tap a window (e.g. 14:00–14:30) -> tile with the strip. Choose a payment method and pay -> `#/objednavka-odeslana`.
- On mobile the order summary opens by tapping the total in the header.
- Dark chip bottom-left = **prototype-only** scenario switch: Plus (demo savings 310 Kč, already paid back) -> Plus (20 Kč, not yet) -> without Plus. URL: `?plus=0`, `?saved=70`.
- **Demo clock (prototype-only):** the page clock is moved to 9:30 of today so the drawer shows "Už dnes od 13:00" and the same windows as the Figma at any time of day. `?clock=real` disables it, `?clock=HH:MM` sets another time. The date itself is the real one.

## Decisions and why

- **Figma wins wherever it speaks.** Slot drawer, Plus row, Delivery tile and the section texts follow the frames (see the deviation list below). The old green banner in the drawer, the green "S Košík Plus ušetříte" line on the tile, the "Doprava zdarma" tag and the pre-selection price preview from iterations 1–2 were removed because the Figma replaces them.
- **Prices from the Figma** replace the earlier assumptions (49/0 and 89/99 with −50): 30 min 59 -> 29, priority 79 -> 39, 1 h 39 -> Zdarma, 2 h Zdarma.
- **The checkout starts without a chosen time** (Figma left frame); "Dokončit" stays disabled until a time and a payment method are chosen.
- **Free-delivery progress** ("355,37 Kč do dopravy zdarma") is no longer treated as reached for Plus customers; the Figma shows it for a Plus customer too.
- **The two bars mean different things and look different on purpose.** Figma bar (gray track, green block on the right) = remaining validity of the advantageous delivery. Addition: savings bar in the summary / confirmation = purple, filled from the left, on a white track, always with text.
- **Additions kept (a, b, c), restyled in the Figma language:** the summary card, the cumulative savings and the confirmation page reuse the purple strip (badge + "Díky Košík Plus ušetříte …"), crown/green chips and the green Plus price, so they read as one family with the Figma frames. They are flagged as additions in the task template and need design review.

## Deviations from the Figma (complete list)

1. **Day tabs** (Dnes / Zítra / …) are in the drawer; the Figma frame has none (production feature, needed to see other days).
2. **Drawer header is partly production**: close button size/position and the header border come from the DS drawer; "Upravit" is inert (no address editing in the prototype).
3. **Data differs from the frames**: cart total 861,64 Kč (Figma 824,34 Kč), header "Ušetříte 24,50 Kč" (Figma 201,64 Kč), the order progress tile shows 1 filled step (Figma 2), first slot is 13:00 only thanks to the demo clock.
4. **Address tile**: label "Doma", name, address and phone were changed to the Figma values, but the name is in the production weight (semi-bold) whereas the Figma has regular.
5. **Pay button and agreement text** are the production component ("Dokončit a zaplatit" / "Koupit s platbou u kurýra"), not the Figma "Dokončit objednávku" + agreement paragraph. Not restyled (out of scope). Note: "objednávka" is not in the copy glossary ("nákup").
6. **Header** shows the generic user icon, not the "OH" avatar.
7. **Strip width**: the purple strip spans the tile body with 12 px side margins like the Figma; the tile and strip share one white card via a wrapper (the DS tile has no slot below the content), with the elevation re-applied on the wrapper.
8. **"−50 %" badge** is computed (share of the regular price rounded to 5 %) instead of a fixed label; a free window shows "−100 %".
9. **Hard-coded Figma values** where the DS has no token: split-chip colors `#ededed` / `#def5e8`, strip colors `#f3e3f6` / `#6b1c78`. Text/background tokens (`ds-contrast-green`, `ds-tint-highlight-green`, `ds-neutral-*`, text styles) are used elsewhere. The crown and "Košík Plus" logo are the SVGs from the Figma, inlined.
10. **Switch labels** were changed to the Figma copy ("Nákup můžu převzít až o 30 minut dřív, pokud bude mít kurýr náskok.", "Přidat poznámku pro kurýra", "Použít jinou fakturační adresu"); the sentence about the window extension not affecting the price was dropped with it.
11. **Basket** also shows the Plus row (the Figma shows only the checkout); the tile there is not outlined (production behaviour).
12. **Customer without Plus**: not in the Figma; the quiet text hints are an addition.
13. The Figma screens are static; the interactive details (drawer animation, pulse of the tile after choosing, focus) are production behaviour of the DS components.

## What was changed in the file (for developers)

Based on `kosik-plus-savings-perception.html` (iteration 2). All changes are inside `PROTO:` regions; search for `PLUS-FIGMA` (new) and `PLUS-SAVINGS` / `PLUS-DELIVERY` (earlier).

- `PROTO:EDIT app`, first `<script>` (`window.PlusDelivery`): demo clock; prices per Figma; `days()` generates the demo windows; `decorate()` / `findRaw()` use them; `PlusAssets` (inlined Figma SVGs); `ProtoKit.PlusValidityRow` (Plus row); scenario chip as before.
- `PROTO:COMPONENT checkout-delivery`: `DeliverySlotAvailability` (split chip, free tag), `TransportDrawer` (address subtitle, Plus row, no banner, 20 px title), `TimeslotsTile` (chip + strike-through, purple strip in one card, text format "Dnes 14:00–14:30"), translations (copy per Figma).
- `PROTO:COMPONENT checkout-summary`: the purple card incl. cumulative savings.
- `PROTO:EDIT app`: Plus row in basket and checkout, checkout starts without a chosen time, address data, confirmation page restyled.

## Open questions

1. What exactly does the right-aligned green part of the validity bar mean (days left vs. elapsed), how long is the package, and what is shown when it runs out or is about to?
2. **Both bars at once:** the validity bar ("17 dní") is in the Figma; the savings progress (price of the package vs. savings) is our addition. Should both appear, and if so where, so they are not confused? Alternatively show the cumulative saving only after the purchase.
3. Is "−50 %" a fixed label or a computed share, and what should it show for free windows?
4. Real prices and the rules behind them (1-hour window 39 Kč below the threshold; 2-hour window free for all; priority 79 -> 39).
5. Which period counts against the package price; does a renewal reset the counter; what if savings far exceed the price?
6. Where should the non-Plus hint lead (Plus offer page / purchase modal)?
7. E-mail / app confirmation of the savings; the confirmation page itself is not in the Figma.
8. Baseline, target, owner and measurement period of the renewal/churn metric.
