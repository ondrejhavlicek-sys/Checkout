# Task Template — Vnímání úspory s Košík Plus (doprava, celková úspora vs. cena balíčku, potvrzení nákupu) — verze podle Figmy

Export version: **2026-10-21_1437** (iteration 3; earlier: `2026-10-14_1007`, `2026-10-21_1353`)
Prototype file: `kosik-plus-savings-perception-v2.html` (self-contained, open in a browser). Routes: `#/basket` -> `#/cashdesk` -> `#/objednavka-odeslana` (confirmation, reached by choosing a delivery time + payment method and pressing the pay button).
A dark chip in the bottom-left corner (prototype-only) cycles three scenarios: Plus customer whose Plus has already paid back / Plus customer who is not yet paid back / customer without Plus. `?plus=0` starts without Plus, `?saved=N` sets the demo cumulative saving, `?clock=real` turns off the demo clock (see notes.md).

> Design source: Figma file "Convenience fee - Packaging fee (KOŠÍK PLUS)", section "Verze k diskuzi" (node 3079:43042): frames "Cash desk page" before choosing (3079:37990), "Slot selection" (3078:36905), "Cash desk page" after choosing (3079:38485). Where the Figma speaks, the prototype follows it; everything it does not show is marked below as an **addition beyond the Figma**. Nothing was confirmed with business, so unconfirmed fields are marked `MISSING ⚠️`. Recipient: reach out to the design department to finalize them.

---

## Key information

### Problem statement

A customer who pays for Košík Plus does not see, at the moments that matter, what the package gives them back. The checkout must make the Plus price advantage visible where the delivery time and price are chosen (slot list, Delivery tile), and the customer should also see how the savings add up against the package price and get the result after the purchase. The primary audience is the Plus customer; the goal is to support Plus renewal.

### Value

- **For the customer** — Sees the Plus price next to the regular one at the moment of choosing a time (crown chip + struck-through price, "−50 %" strip, "ušetříte 30 Kč"), how long the advantageous delivery still lasts (17 days), and — as additions — how much Plus has saved against its price and a confirmation after the purchase.
- **For the business** — Higher perceived value of the package, which should support Plus renewal and reduce churn. `MISSING ⚠️`: no business case, volumes or financial target were provided.

### Measurement of success

- **Primary business metric (chosen by the requester): Plus renewal rate / churn of Plus customers.**
  - Baseline: `MISSING ⚠️`
  - Target: `MISSING ⚠️`
  - Measurement period / cohort: `MISSING ⚠️`
  - Owner: `MISSING ⚠️`
- **Customer-wise (usability)** — `MISSING ⚠️`, suggestion: in a short test customers can say correctly how much they saved on this delivery and how long the advantageous delivery lasts; they do not confuse the validity bar with a savings bar.
- Supporting signals (suggested, not confirmed): support contacts "was my Plus discount applied?", share of Plus customers choosing 30-minute / priority windows.

**How the customer finds out** — In the prototype: Plus row in "Čas doručení" (basket and checkout) and in the slot drawer, crown chips on slots, Delivery tile with the strip, header "Ušetříte" pill, and (additions) the summary card and the confirmation page. Not designed (`MISSING ⚠️`): e-mail / app communication, first-time explanation, what happens when the 17 days run out.

### Limitations

- **All prices are taken from the Figma frames and are NOT confirmed production pricing** (`MISSING ⚠️`):
  - 30-minute window: regular **59 Kč**, Plus **29 Kč** (saving 30 Kč, badge "−50 %")
  - Priority window (30 min and 1 h): regular **79 Kč**, Plus **39 Kč** (saving 40 Kč)
  - 1-hour window: regular **39 Kč** (free from the free-delivery threshold), Plus **Zdarma**
  - 2-hour window: **Zdarma** for everyone (no strike-through price)
  - Free-delivery threshold **1 199 Kč** (production data; the Figma shows "304,44 Kč do dopravy zdarma" for a Plus customer too)
  - Plus package price **100 Kč** (from production config `convenienceFee`) and the period are not in the Figma.
- **"Výhodná doprava ještě 17 dní" bar**: the Figma bar is a gray track with a green block aligned to the right, 181 of 329 px (55 %). The prototype reads it as "17 of ~31 days remain" (`VALIDITY_TOTAL = 31` is an **assumption**, `MISSING ⚠️`: length of the package, and whether the green part means "remaining" or "elapsed").
- **"−50 %" badge**: in the Figma it is a fixed label. 30/59 is 51 %, 40/79 is 51 %; the prototype computes the share and rounds it to 5 %, so both give "−50 %", a free window gives "−100 %". `MISSING ⚠️`: how the badge is defined in production.
- **Cumulative savings are DEMO DATA, not real** (310 Kč = already paid back, 20 Kč = not yet, 6 earlier purchases). Counted: delivery savings only. `MISSING ⚠️`: period against which the package price is compared.
- The demo window list is generated (today from 13:00 with the priority group, then 2-hour blocks) and the page clock is set to 9:30 so the screens look like the Figma at any time of day. The captured August slots are not used.
- The basket and the checkout keep separate slot choices (a slot chosen in the basket is not carried to the checkout). The checkout starts with no slot chosen, as in the Figma.
- Not covered: expired / about-to-expire Plus, Plus purchase flow, product discounts, additional order, anonymous customer, e-mail.

### Requirements

**From the Figma (authoritative):**

1. **Section "Čas doručení" (basket and checkout)** — for a Plus customer a Plus row above the tile: "Košík Plus" logo on the left, "Výhodná doprava ještě **17** dní" on the right, an 8 px bar below (gray track, green part right-aligned). Not shown to customers without Plus.
2. **Delivery tile, no time chosen** — outlined tile "Vyberte čas doručení / Už dnes od 13:00" (checkout, required state).
3. **Slot drawer** — title "Vyberte čas doručení" (20 px), pin + delivery address + link "Upravit", the Plus row, then the slots on gray. Priority group on a light purple panel with the header "Prioritní rychlé doručení" (star icon). Slot tile: time, icons (star = priority, clock = closing soon), then the price as a **split chip** (crown on gray | Plus price on light green, green text) with the regular price struck through below. Free-for-Plus windows: chip with crown + "Zdarma" and the struck-through regular price. Windows that are free for everyone: plain green "Zdarma" tag, no strike-through. Three columns (30 min / 1 h / 2 h) with the tall tiles spanning rows.
4. **Delivery tile after choosing** — "Dnes 14:00–14:30", green chip "29 Kč" + struck-through "59 Kč", and inside the same white card a purple strip: badge "−50 %" + "Díky **Košík Plus** ušetříte **30 Kč**". Under the tile the toggle "Nákup můžu převzít až o 30 minut dřív, pokud bude mít kurýr náskok." and "Přidat poznámku pro kurýra".
5. Section texts per Figma: "Použít jinou fakturační adresu", "Přidat poznámku pro kurýra"; address tile with the label "Doma".

**Additions beyond the Figma (flagged, need design review):**

6. **Day tabs in the drawer** (Dnes / Zítra / …) — kept from the production drawer, the Figma frame does not show them.
7. **Summary (basket / checkout; popover on mobile)** — when a Plus delivery is chosen, one purple card under the total in the Figma strip language: badge "−50 %" + "Díky Košík Plus ušetříte 30 Kč na dopravě". The delivery saving is also added to the header "Ušetříte" figure. The saving is not subtracted again from the total.
8. **Cumulative savings vs. package price** (in the same card; progress bar fills left to right in purple so it is not confused with the green validity bar): "Celková úspora s Plus", then one of three states — already paid back ("Košík Plus se vám už vrátil"), this purchase pays it back ("Tímto nákupem se vám Košík Plus vrátí"), not yet ("Ušetříte celkem 50 Kč ze 100 Kč" / "Ještě 50 Kč a Plus se vám vrátí."). Copy is factual, no pressure.
9. **Confirmation page** (`#/objednavka-odeslana`, gray page, white tiles): "Nákup je odeslán", delivery time + address + transport row with the chip, the same purple card in the past tense ("Díky Košík Plus jste ušetřili …"), button "Zpět na úvod". Customer without Plus: one gray text line only.
10. **Customer without Plus** (not in the Figma) — no Plus row, plain prices on slots, one gray hint line in the drawer, on the tile ("S Košík Plus byste ušetřili 30 Kč.") and in the summary. No CTA; link target `MISSING ⚠️`.
11. Mobile first, verified at 375/393 px and 1280 px. Czech copy, formal address (vykání), no humor. Accessibility: both bars have `role="progressbar"` with text; success is not communicated by color alone.
12. Alignment with product vision — `MISSING ⚠️`.
13. Backend needs: Plus validity (days left, total), regular and Plus price per window, cumulative Plus saving, package price and period.

### Resources

- Figma: https://www.figma.com/design/RfcmogmHScG4IusIONEwe0/Convenience-fee---Packaging-fee--KO%C5%A0%C3%8DK-PLUS- — section "Verze k diskuzi", node 3079:43042 (frames listed above).
- Side-by-side screenshots: `comparison/` in this bundle.
- Earlier exports: `exports/kosik-plus-delivery-savings-2026-10-14_1007`, `exports/kosik-plus-savings-perception-2026-10-21_1353`.
- Kit docs used: GUIDE.md, design principles, page construction (checkout = gray background), accessibility, copywriting (glossary), colors / typography, buttons, `checkout-delivery`, `checkout-summary`, `prototype-file`.
- No PRD, analytics or business data provided — `MISSING ⚠️`.

### More information and notes

See `notes.md` in this bundle (every deviation from the Figma, decisions, what was changed in the file, open questions).
