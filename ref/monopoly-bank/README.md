# Monopoly bank app — project bundle

Everything we've decided and built so far for the homemade
Monopoly Super Electronic Banking bank app.

## What's inside

docs/
- bank_actions_spec.md  — the full, manual-accurate list of bank actions
- decisions.md          — our running design decisions

demos/  (open in a browser — double-click)
- tap-circle-chooser.html  — tap circle -> pick type -> pick card (our latest)
- multi-card-reader.html   — handles player / property / chance cards, real money math

## Where we are
- App design + flows worked out.
- Card taps simulated on-screen (no NFC stickers needed yet).
- One entry point, onCardTap(cardId), so real NFC drops in later with no rewrites.

## Next step
Build the real single-file web app, deploy to Netlify, open in Chrome on Android.
