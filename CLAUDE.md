# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Browser-based Tetris game implemented as a single self-contained HTML file with inline CSS and JavaScript. No build tools, frameworks, or dependencies.

## How to Run

Open `tetris.html` directly in a browser. No server or build step required.

`tetris.js` is a Node.js utility script that regenerates `tetris.html` via `node tetris.js`. The two files contain identical game code — `tetris.js` simply writes the HTML content to disk.

## Architecture

Everything lives in `tetris.html`:

- **CSS** (inline `<style>`): Dark-themed layout using CSS Grid for the 10x20 game board and 4x4 next-piece preview. Game-over overlay uses fixed positioning with `.show` class toggle.
- **JavaScript** (inline `<script>`): All game logic in global scope. No modules or classes.

Key data structures:
- `board` — 2D array (20 rows x 10 cols), stores color strings for locked pieces, `0` for empty
- `currentPiece` / `nextPiece` — objects with `{shape, color, x, y}` where `shape` is a 2D binary array
- `SHAPES` / `COLORS` — parallel arrays indexed by piece type (I, O, T, L, J, S, Z)

Game loop: `setInterval` at `1000 - (level-1)*100` ms. Each tick calls `movePiece(0,1)`, locking the piece if it can't move down. Rendering re-paints all cells every frame by setting `style.background` on DOM elements.

Scoring: lines cleared × 100 × level. Level = `floor(score/1000) + 1`. Soft drop +1/cell, hard drop +2/cell.

Rotation uses transpose-and-reverse without wall kicks — rotation is rejected entirely if the result collides.
