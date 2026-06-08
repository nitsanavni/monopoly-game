# Design decisions

Our running decisions for the homemade Monopoly Super Electronic Banking bank app.

## Architecture
- Everything physical (board, cards, tokens, die). Phone sits to the side as the bank terminal.
- Cards are dumb identity tokens. All game state lives in the app, keyed by card id.
- 25mm round NFC tags, one per card. (Stickers not in hand yet — simulate first.)

## The one rule that matters most
- Every card read goes through a single function: `onCardTap(cardId)`.
- Today a button/chooser calls it. Later the NFC reader calls the exact same function.
- Swapping in real NFC adds code; it deletes nothing.

## Card types (three kinds)
- player    — the 4 tokens (who is transacting)
- property  — the 16 Title Deeds (which property)
- chance    — the 20 Chance cards (apply effect)
Each tag carries a `kind` + its data. Mode + which card came first decides the action.

## Simulation (before stickers arrive)
- Chosen: on-screen tap circle.
- Flow: tap circle -> choose type -> choose specific card -> fires onCardTap.

## Transaction feel
- A payment should feel like a card machine: pulse -> brief process -> green approved + receipt.
- Haptic buzz (navigator.vibrate) on tap and on approval.
- Open question: keep the ~1s processing pause (weighty) or make it snappy (~300ms)?

## Screens
- Home (balances + action modes)
- Action flow (generic tap screen, reused by most actions)
- Auction (special: live bid, timer)
- Players (balances, tokens, property counts)
- Setup (one-time card registration)
- Settings (undo, end game, sound, new game)

## Defaults chosen
- Balances shown on Home.
- Flows start by picking a mode, then tapping cards.

## Key rules from the manual (affect bank logic)
- Rent is OWNER-initiated: owner taps property card first, then payer taps.
- Rent is calculated: 1 of a colour pair = half price; both = full price.
- In Jail: cannot collect rent, cannot bid; can be forced-traded.
- Bankruptcy is forgiving: clear debt to M0, keep playing.
- Undo: only the single most recent transaction.
- Game ends when all 16 properties owned; score = cash + properties at purchase price.

## Currency
- Symbol is M.

## Deployment plan
- Single-file web app, deploy to Netlify, open in Chrome on Android (Web NFC).
