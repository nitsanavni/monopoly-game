# Monopoly Super Electronic Banking — Bank App Action Spec

Derived from the official rules (Monopoly Wiki / Hasbro). This is the
authoritative list of everything the "bank" (our phone app) must do.

Currency symbol in our build: **M**

---

## 1. Money actions (committed by a card tap)

| Action | Who taps | Amount | Notes |
|---|---|---|---|
| Pass GO | own card | +M200 | Collected when reaching/passing GO. Not collected during a Flight. |
| Buy property | property card, then own card | − purchase price | Property becomes the buyer's. |
| Pay rent | **owner taps property card**, then payer taps own card | rent → owner | Owner initiates. Cannot collect rent while in Jail. |
| Flight | own card | −M100 | Free for Frequent Flyer. Fly to any of the 16 property spaces. |
| Chance | Chance card, then own card | varies | Bank applies the card's reward/penalty. |
| Get out of Jail (pay) | own card | −M100 | One of three ways out. |
| Auction | highest bidder taps own card | − final bid | Starts at M10, +M10 per bid. In-Jail players cannot bid. |

### Rent calculation (important — not a fixed number)
- Own **1** property in a colour pair → rent = **half** the purchase price.
- Own **both** properties in a colour pair → rent = **full** purchase price (for each).

---

## 2. Property moves (no cash — bank updates ownership)

- **Forced Trade**: on a Forced Trade space, you and a chosen player each give
  one property to the other. Bank records new owners. Cannot be declined once made.
  Trading is ONLY possible on this space.
- **Sell to bank (debt only)**: when in debt, tap a property card to sell it back
  at purchase price. Bank pays you and keeps the property. Repeat until ≥ M0.

---

## 3. Token rewards (each triggers a bank credit)

| Token | Reward |
|---|---|
| World Traveler (yellow car) | +M50 for rolling a 6 |
| Frequent Flyer (blue plane) | Flies free instead of paying M100 |
| Super Saver (gray safe) | +M50 when landing on Chance |
| Big Spender (red bag) | +M50 when first to buy in a colour group (full price, 2 still unowned, no auction) |

---

## 4. Rules that affect bank logic

- Start: each player M1500. At least 2 players must be registered to start.
- Single die. No doubles. No going to Jail by dice roll.
- No houses/hotels. No mortgaging. No normal selling (only debt-selling).
- In Jail: CANNOT collect rent, CANNOT bid in auctions, CAN be forced-traded.
- Bankruptcy is forgiving: if negative with no properties, bank clears debt to M0;
  player keeps playing. Bankruptcy never removes a player.
- Undo: only the single most recent transaction can be undone.
- Game ends when all 16 properties are owned. Final score = cash + properties
  (at purchase price). Highest wins.

---

## 5. App mode list (what our UI needs)

Money modes:
1. Pass GO (collect M200)
2. Buy property
3. Pay rent  (owner-first tap order!)
4. Flight (M100, free for Frequent Flyer)
5. Chance (apply card effect)
6. Jail — pay M100
7. Auction

Property modes:
8. Forced Trade
9. Sell to bank (debt)

Plus: register players (one-time), undo last transaction.
