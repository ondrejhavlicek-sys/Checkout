# Notes for implementation and design

Export version: 2026-10-23_0909 — iteration 1 (the three Figma features; "enhancements" are not included yet)

## ⚠️ Key information is missing

See `task-template.md` (fields marked `MISSING ⚠️`): success metrics, business value, real prices and
rules for Benjamínek and voucher, length of the Plus package, backend contract, how customers are informed.
**Please reach out to the design department** before treating this as a build spec.

## How to look at the prototype

- Open `kosik-fresh-slot-discounts.html` in a browser (single file, no server). If the page stays blank,
  bring the tab to the foreground (the page waits for rendering frames before it mounts).
- Flow: `#/basket` (opens here) → "Doprava a platba" → `#/cashdesk` (no time chosen, tile with the dark
  "required" outline) → tap the tile "Vyberte čas doručení" → drawer → tap a window → back on the cash desk:
  the tile pulses, the discount bar slides in, then the stable selected state. The tile can be tapped again to
  change the window.
- Dark chip bottom-left = **PROTOTYP ONLY, not part of the design**: switches the discount reason
  (Košík Plus / klub Benjamínek / voucher na dopravu zdarma / bez slevy). Changing it re-prices the window
  that is already chosen. The voucher reason also shows a "DOPRAVA-ZDARMA" voucher in "Kredity a slevy".
- Demo clock (prototype only): "now" is today 9:30 (the time in the Figma status bar), so the drawer
  always starts with the priority windows from 13:00 and the tile says "Už dnes od 13:00".
- The CTA "Dokončit a zaplatit" stays disabled until a delivery time is chosen (and a payment — a saved
  Mastercard •••• 2030 is preselected). The CTA leads to the kit's placeholder page (the kit never places
  an order).

## What was built on top of the kit template

Base: `dist/spa-standalone.html` (basket → cashdesk with a shared cart). The default route now redirects
to the basket; the session cart starts as the captured basket (10 products).

Edited regions (everything else is untouched kit code):

- `PROTO:EDIT head` — title and the `proto-reveal` keyframes.
- `PROTO:EDIT app` — cash desk page: Plus strip, address for the drawer, feedback handler, switch,
  data (customer from the Figma, saved card, generated transport windows, thresholds).
- `PROTO:COMPONENT checkout-delivery` — the slot cell (`DeliverySlotAvailability`), the delivery tile
  (`TimeslotsTile`), the drawer header/strip, the demo clock and the discount model
  (`ProtoKit.slotDiscount`, `ProtoKit.PlusStatusStrip`, `ProtoKit.pragueDate`). Marked with `PROTO:` /
  `PROTOTYPE ONLY` comments.

Discount model (all prototype assumptions, unconfirmed): Plus and Benjamínek = half the price rounded
down, a window of up to 39 Kč is free; voucher = every window free. Windows carry the base price, every
screen shows the discounted price; the summary "Doprava" row and the total use the discounted price.

## Deviations from the Figma

1. **Day tabs** in the drawer (Dnes / Zítra / …) and a 16 px empty spacer under the header are production
   behaviour of the kit's drawer; the Figma "mobile_today" frame has neither.
2. **Drawer header**: DS drawer title (smaller than the Figma title); the address wraps and "Upravit" follows
   the text on narrow screens. "Upravit" is inert (address editing is not part of the prototype).
3. **Progress tile at the top** is the kit's production order-progress bar: "Zbývá 304,44 Kč do dopravy
   zdarma" and one milestone circle before the bar; the Figma has "304,44 Kč do dopravy zdarma" and two.
   Free-delivery threshold set to 1 148,07 Kč so the gap is 304,44 Kč with the captured basket.
4. **Numbers**: the header shows the real cart total 861,64 Kč / "Ušetříte 24,50 Kč" (Figma: 824,34 Kč /
   201,64 Kč). After a window is chosen the total includes the delivery price (890,64 Kč for 29 Kč); the
   Figma total does not change. "Ušetříte" does **not** include the delivery saving (as in the Figma).
5. **Production copy kept** where the Figma differs: "Jiná fakturační adresa" (Figma: "Použít jinou
   fakturační adresu"), "Poznámka pro kurýra" (Figma: "Přidat poznámku pro kurýra"), the longer early-delivery
   sentence, the CTA "Dokončit a zaplatit" (Figma: "Dokončit objednávku") and the legal text above it.
6. **CTA disabled** until a time is chosen (production behaviour, the aux text says "Vyberte prosím čas
   doručení"); the Figma shows it enabled in the "before" frame.
7. **Card expiry** 10/28 instead of 10/25 (10/25 is in the past).
8. Strings aligned to the Figma: "Vyberte čas doručení", "rychlé doručení", en dash in "Dnes 14:00–14:30".
9. **Colours**: the nearest DS tokens are used, so a few values differ slightly (see DS gaps).
10. **Percent** is rounded down to a multiple of 5 % ("−50 %" for 50.8 %); "−100 %" for free windows.
11. **Crown icon**: the DS "Crown" is two-tone; both parts are painted in the main purple to match the
    Figma chip (`--ds-icon-fill-secondary` override on that icon only).
12. The slot grid, the order of priority/standard windows and the green selected/hover states are the
    kit's production drawer; the Figma only shows the discount cells.

## Design-system gaps (hard-coded or composed)

- **Košík Plus wordmark** — no DS logo; the Figma SVG is inlined as a data URI (`PROTO:CONFIG plus-logo`).
- **Validity / progress bar** — no DS component; built from tokens (`ds-neutral-20` track, `ds-accent-green`
  fill, `rounded-ds-sm`, `h-2`) with `role="progressbar"`.
- **Split chip (reason icon + price)** — no DS component; composed from tokens. Icon cell `ds-neutral-5`
  (#F0F0F0; Figma #EDEDED), price cell `ds-tint-highlight-green` (#E5F9EE; Figma #DEF5E8), text
  `ds-state-success`.
- **Discount bar in the tile** — no DS component; `ds-primary-light` (#F4E4FF; Figma #F3E3F6), badge
  `ds-primary` (#651B69; Figma #6B1C78), badge text `text-style-xs` 11 px + stronger weight (Figma 10 px bold).
  It is laid into the DS `Tile` by turning the tile body into `display: contents` and wrapping the tile row
  (`classes.base`/`classes.body`), so the tile stays one DS component and one focus target.
- **Reveal animation** of the bar — custom CSS keyframes (320 ms, 420 ms delay so the drawer is gone first);
  the DS only has `animate-ds-pulse-green`. Disabled under `prefers-reduced-motion`.
- **Prototype switch** — plain buttons styled with tokens (not DS components); not part of the design.
- Arbitrary values: `tw-pt-[3px]` (address row alignment), `tw-bottom-[150px]` and `tw-z-[100000]` (switch).
- The icon-only reason in the drawer chip (crown / bear / voucher) breaks the "never an icon alone" rule;
  kept as in the Figma with hidden text for screen readers — **needs design sign-off**.

## Feedback state after choosing a window (requirement 3)

Sequence after the tap in the drawer: drawer closes (~450 ms) → the tile is brought into view if needed →
the DS green pulse on the tile (≈600 ms after the choice, production's `useAnimationEffect`) → the discount
bar slides in (starts at 420 ms) → polite announcement for screen readers ("Čas doručení vybrán: Dnes
14:00–14:30, 29 Kč, sleva díky Košík Plus") → stable selected state (white tile, no further motion). The
drawer keeps the chosen window highlighted green. Nothing is time-limited: the animation only decorates,
all information stays on screen.

## Tested

Chrome (Claude browser pane), 393 × 851, 375 × 812 and 1280 × 800:
basket → cash desk → drawer → choose a window → cash desk with feedback, for all four states (Plus,
Benjamínek, voucher, bez slevy); changing the window; choosing a window on another day; changing the
reason with a window already chosen; switching reason while the drawer is open (the click is outside the
drawer, so the drawer closes — it is a prototype-only control); no horizontal overflow at 375 px;
no console errors on a fresh load. The pulse and the reveal were verified by sampling class names/computed
style during the transition plus screenshots. Screenshots are in `comparison/` (Figma frames next to
the prototype states; `proto-393-state-*` shots were taken before the last cosmetic tweaks — crown colour
and en dash).

## Not tested

Real phones / touch, Safari, Firefox, keyboard-only walkthrough and a screen reader (the announcement and
the hidden texts are implemented but not heard), 768–991 px widths, dark/high-contrast modes, the destination
page after "Dokončit a zaplatit" (kit placeholder), cash desk with an empty cart, other cart contents (the free-delivery gap is only
correct for the captured basket), the basket page after the changes beyond opening it and moving on.
