# Copilot Instructions for svelte-gomoku

## Project overview
- This repository contains a Svelte 5 component library for a playable Gomoku board.
- The main component is implemented in src/lib/Gomoku.svelte and re-exported from src/lib/index.ts.
- The project also includes demo routes for engine play and customization in src/routes/engine and src/routes/customization.

## Coding conventions
- Use TypeScript and keep the code strict and type-safe.
- Prefer simple, readable logic over clever abstractions.
- Preserve the existing public component API unless a change is explicitly requested.
- Keep behavior consistent with the documented props, callbacks, and methods in the README.
- Follow Svelte 5 patterns and keep the component compatible with runes-based syntax.

## Component-specific guidance
- The core game state lives in src/lib/Gomoku.svelte.
- Keep the following behavior intact unless a change is explicitly required:
  - move placement, undo, and reset flow
  - win and draw detection
  - player turn switching
  - bindable props such as locked, size, showGhost, moveNumber, history, board, current, and winner
- Avoid breaking the callback flow for onMove, onWin, and onDraw.
- Keep the board interaction accessible and usable in the demo pages.

## Styling
- Keep styles local to components when possible.
- Theme-related CSS should remain compatible with the files in src/lib/themes and the template in static/style-template.css.
- Avoid introducing styling that breaks existing themes or the demo layouts.

## Validation
- Before finishing changes, run npm run check.
- Prefer minimal, targeted changes and avoid unrelated refactors.