# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

A static single-page flashcard app deployed via GitHub Pages. No build step — `index.html` at the repo root is served directly.

## Architecture

All card data is **embedded in `index.html`** as a `DECKS` JS array, not loaded from the CSV files at runtime. The CSVs in `flashcards/` are the canonical source of truth for card content; when adding or editing cards, update the CSV first, then manually sync the changes into the `DECKS` array in `index.html`.

**Data flow:** `flashcards/*.csv` (source) → `DECKS` array in `index.html` (runtime)

Each deck entry:
```js
{ title: "Deck Name", cards: [{ front: "...", back: "..." }, ...] }
```

The `loadDeck(index)` function switches the active deck and resets navigation state. `render()` reads from `DECKS[activeDeckIndex].cards[deck[currentIndex]]`.

## Design

Dark theme with green accent (`#1DB954`). Key values:
- Backgrounds: `#121212` (page), `#181818` (card front), `#282828` (card back / hover surfaces)
- Accent: `#1DB954` — used on progress bar fill and active deck tab
- Text: `#ffffff` primary, `#b3b3b3` secondary — **card text is always regular weight (400), never bold**
- Font: Montserrat (Google Fonts), loaded in `<head>`
- Buttons and deck tabs use `border-radius: 500px` (pill shape), no border, with `scale` hover transitions
- Cards use `box-shadow` for depth instead of borders

## Deployment

Merging to `main` publishes to GitHub Pages automatically. The site serves `index.html` from the repo root.

## Adding a new deck

1. Create a CSV in `flashcards/` with rows in `question,answer` format (quote fields containing commas).
2. Add a new object to the `DECKS` array in `index.html`.
3. Add a `<button class="deck-tab" data-deck="N">` in the `#deck-tabs` div, where `N` matches the new array index.
