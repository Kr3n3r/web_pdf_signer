# AGENTS.md — web_pdf_signer

## Project overview
Single-file browser PDF signing tool (`firmapdf.html`). No build system, no dependencies beyond CDN.

## Setup
- No installation needed. Open `firmapdf.html` in any modern browser.
- Requires internet on first load (CDN: pdf.js 2.16.105, pdf-lib 1.17.1).

## Key facts
- **pdf.js v2.16.105** (UMD global `pdfjsLib`). Do NOT upgrade to 3.x+ without switching to ESM imports.
- **pdf-lib v1.17.1** (UMD global `PDFLib`). Loaded from jsdelivr CDN.
- Worker must be set: `pdfjsLib.GlobalWorkerOptions.workerSrc = '...'`
- Page numbering: pdf.js is 1-indexed (`getPage(1)`), pdf-lib is 0-indexed (`pages[0]`).

## Git workflow (GitHub Flow)
- Feature branches from `master`: `git checkout -b feature/<name>`
- PR + merge to `master` when ready. No direct commits to `master`.

## Architecture notes
- PDF rendered to `#pdfCanvas` via pdf.js. User draws on `#drawCanvas` (overlay).
- Pointer Events API handles touch + mouse uniformly.
- Smooth curves via `quadraticCurveTo` with midpoints.
- On save: draw canvas → PNG blob → pdf-lib `embedPng` → placed on target page.
- Coordinate mapping: `pdfX = canvasX / scale`, `pdfY = pageHeight - (canvasY / scale)`.

## Testing
- Manual only: open `firmapdf.html` in browser, load a PDF, draw, save, verify output.
- No automated tests.

## Repo
https://github.com/Kr3n3r/web_pdf_signer (private)
