# Digital Monopoly — Project Notes & Findings

A homemade version of **Monopoly: Super Electronic Banking**, played with a printed
board, physical cards, tokens and a die — with an Android phone acting as the
electronic bank (replacing Hasbro's "EBU" banking unit).

---

## 1. How the original game works

The original uses an **Electronic Banking Unit (EBU)** as the brain:
- Runs on 3 AAA batteries; has a barcode scanner, a small display, and just 3 buttons: ✓, **M**, ✕.
- Holds ALL state — every player's cash and which properties they own. Sleeps after 5 min and saves the game.
- Each player has a **barcoded Bank card** (matched to their token) and a Reference card.
- **The cards store no money.** Tapping/scanning a card just tells the unit *who* is acting. All logic lives in the bank. (This is the key idea we're copying.)

### Stripped down vs classic Monopoly
- No paper money. No houses or hotels. No mortgaging. No normal selling (only debt-selling).
- One die only (no doubles; you can't go to jail by dice roll).
- 2–4 players. Everyone starts with **M1500**.
- Only **16 properties** (streets only — no railroads/utilities), in **8 colour pairs** of 2.

### The interaction language: "mode + tap"
Press **M** to cycle a mode icon, then tap a card to commit:
- M ×1 = Collect (GO / +M200)
- M ×2 = Auction (gavel)
- M ×3 = Jail
- M ×4 = Flight
- M ×5 = Trade

### Rent (no houses, so simple)
- Own **1** of a colour pair → rent = **half** the purchase price.
- Own **both** of a colour pair → rent = **full** purchase price (each).

### Special spaces
- **Flight** spaces replace railroads: pay M100 to fly to any property space (free for Frequent Flyer token).
- **Forced Trade** spaces: you and a chosen player each swap one property. No negotiation; can't be declined. Trading is ONLY possible on these spaces.

### Token rewards (why the bank must know *who* you are)
| Token | Reward |
|---|---|
| World Traveler (car) | +M50 for rolling a 6 |
| Frequent Flyer (plane) | Flies free instead of paying M100 |
| Super Saver (safe) | +M50 when landing on Chance |
| Big Spender (bag) | +M50 when first to buy in a colour group |

### Bankruptcy & ending
- Forgiving: if in debt, sell properties back to bank at purchase price. If still negative with no properties, bank clears debt to M0 and you keep playing — bankruptcy never removes a player.
- Game ends when all 16 properties are owned. Final score = cash + property (at purchase price). Highest wins.

---

## 2. The real board (read off a photo of the actual game)

**32 spaces total**: 4 corners + 16 streets (8 colour pairs) + 4 Chance + 4 Flight + 4 Forced Trade.
It keeps the classic Monopoly property names but trims to 2 per colour group.

Clockwise from GO (prices in M):

| # | Space | Price | Group |
|---|---|---|---|
| 0 | GO (collect M200) | — | corner |
| 1 | Mediterranean Ave | 60 | brown |
| 2 | Chance | — | — |
| 3 | Baltic Ave | 60 | brown |
| 4 | Flight | — | — |
| 5 | Oriental Ave | 100 | light blue |
| 6 | Forced Trade | — | — |
| 7 | Vermont Ave | 100 | light blue |
| 8 | Jail (just visiting) | — | corner |
| 9 | St. Charles Place | 140 | pink |
| 10 | Chance | — | — |
| 11 | States Ave | 140 | pink |
| 12 | Flight | — | — |
| 13 | St. James Ave | 180 | orange |
| 14 | Forced Trade | — | — |
| 15 | Tennessee Ave | 180 | orange |
| 16 | Free Parking | — | corner |
| 17 | Kentucky Ave | 220 | red |
| 18 | Chance | — | — |
| 19 | Indiana Ave | 220 | red |
| 20 | Flight | — | — |
| 21 | Atlantic Ave | 260 | yellow |
| 22 | Forced Trade | — | — |
| 23 | Ventnor Ave | 260 | yellow |
| 24 | Go to Jail | — | corner |
| 25 | Pacific Ave | 300 | green |
| 26 | Chance | — | — |
| 27 | North Carolina Ave | 300 | green |
| 28 | Flight | — | — |
| 29 | Park Place | 350 | dark blue |
| 30 | Forced Trade | — | — |
| 31 | Boardwalk | 400 | dark blue |

True Monopoly group colours:
brown `#955436`, light blue `#AAE0FA`, pink `#D93A96`, orange `#F7941D`,
red `#ED1B24`, yellow `#FEF200`, green `#1FB25A`, dark blue `#0072BB`.

---

## 3. Our build — decisions so far

- **Everything physical**: printed board, printed cards, tokens, a die. Played like a normal board game.
- **The phone is the bank ONLY.** It sits to the side; you tap a card on it when money moves (buy, rent, pass GO). It is NOT a screen everyone stares at.
- **Tags = identity, app = brain.** We use the NFC tag's permanent unique ID (UID) and keep a lookup table in the app (`UID → card`). Tags stay blank/reusable; no writing needed.
- **Cards are credit-card format** — 85.6 × 54 mm (ISO ID-1), landscape. Content on the left, a 25 mm round zone on the right for the NFC tag.
- **Tags purchased**: 25 mm round NFC stickers.
- **Tokens/figurines**: keep it light for the first playable version (reuse pieces, paper stand-ups, or LEGO). 3D printing is a "later" nice-to-have. The NFC tag goes on the *card*, not the figurine — so figurines need no electronics.

---

## 4. Web NFC — proven working ✓

- Web NFC (`NDEFReader`) reads each tag's UID. **Confirmed working** on the Android phone in Chrome.
- **Hard constraints learned:**
  - Works ONLY in **Chrome on Android**. No iPhone, no desktop.
  - Must be served over **HTTPS** (or localhost). Opening the HTML file directly does NOT work — no permission prompt appears, it silently fails.
  - NFC must be toggled ON in Android settings/quick-settings.
  - Permission is requested **when the page first calls `scan()`** (after tapping the button) — not from a settings menu. It's per-site. If you tap Block, reset it via the site-permissions icon by the address bar.
- **Deploy trick that worked**: drag the HTML onto a static host (Netlify Drop / tiiny.host) to get an instant HTTPS link. Note: the root URL needs the file named `index.html`, otherwise append the filename to the URL (e.g. `/nfc-scan-demo.html`).

---

## 5. What's built

- `card-generator.html` — edit all 40 cards (16 deeds, 20 Chance, 4 player cards) and export print-ready A4 PDFs at credit-card size, with cut lines and a 25 mm round NFC zone marked on each card.
- `nfc-scan-demo.html` — minimal Web NFC reader. Proves the phone can read tag UIDs. (Tested ✓)

## 6. Next steps (not yet built)

1. **Registration screen** — tap each tag, assign it to a card, save the lookup table (JSON). Likely the first screen ("Setup") of the bank app.
2. **Bank app** — holds all cash + property; implements the "mode + tap" actions (Pass GO, Buy, Pay rent, Flight, Chance, Jail, Auction, Forced Trade, Sell-to-bank, Undo last).
3. Optional: prettier printed-card styling (colour bands, layout) in the PDF export.
4. Optional: print paper stand-up tokens for the 4 characters.
