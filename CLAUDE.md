# Monopoly game

A homemade version of **Monopoly: Super Electronic Banking**: printed board,
physical cards/tokens/die, with an Android phone acting as the electronic bank
(replacing Hasbro's EBU banking unit).

## Core idea
- **Tags = identity, app = brain.** NFC cards store no money. Tapping a card tells
  the bank *who/what* is acting; all game state lives in the app, keyed by card id.
- Every card read goes through a single entry point: `onCardTap(cardId)`. Simulated
  taps call it today; real Web NFC calls the exact same function later (adds code,
  deletes nothing).
- Currency symbol is **M**. 2–4 players, everyone starts at M1500.

## Stack / constraints (proven)
- Single-file web app, deployed over HTTPS (Netlify Drop / tiiny.host).
- Web NFC (`NDEFReader`) reads tag UIDs — works **only in Chrome on Android**, HTTPS
  required, NFC toggled on. Opening the HTML file directly does not work.
- Cards are ISO ID-1 (85.6×54mm) with a 25mm round NFC zone; 25mm round NFC stickers.

## Layout
- `ref/` — reference bundles (design docs + prototypes). Source material, not the build.
  - `ref/digital-monopoly/` — richest notes: full 32-space board, rules, NFC findings,
    `card-generator.html` (export print-ready cards), `nfc-scan-demo.html` (tested ✓).
  - `ref/monopoly-bank/` — action spec, design decisions, two sim demos
    (`tap-circle-chooser.html`, `multi-card-reader.html` with real balance math).

## Workflow
- Always commit and push directly to `main`.
