# PDFUnlocker 🔓

A Passper-for-PDF-style web app to **unlock encrypted PDFs and recover their open
passwords** — 100% client-side (no server, no uploads).

## Features (mirrors Passper for PDF)

| Mode | What it does |
|---|---|
| **Remove known password** | Enter the open password → downloads an unlocked PDF |
| **Dictionary attack** | Built-in common-password list, load your own `.txt` wordlist, optional capitalize / append 1–9 mutations |
| **Brute force** | Pick character sets (a–z, A–Z, 0–9, symbols) + length range 1–6 |
| **Mask attack** | Pattern-based search, e.g. `?u?l?l?l?d?d?d?d` for `John1985` |

Extras: live progress bar, attempts/sec, ETA-style counts, stop/resume, found-password reveal,
one-click download of the unlocked file.

## Run it

```bash
npm install
npm run dev      # → http://localhost:5173
```

Build for production:

```bash
npm run build    # outputs dist/
```

## How it works

- **pdf.js** (`pdfjs-dist`) — detects encryption, validates candidate passwords, renders pages.
- **pdf-lib** — assembles the unlocked output file.
- Recovery = repeatedly opening the PDF with generated candidates (~tens/sec, CPU-bound).

## Important limitation

Browsers can't re-encrypt raw PDF object streams, so the unlocked PDF is **rebuilt page-by-page
as high-res (2×) images**. It opens without a password, but text is not selectable/searchable.
For a text-preserving unlock, use `qpdf` server-side:

```bash
qpdf --decrypt --password=YOURPASS input.pdf output.pdf
```

Only use this tool on PDFs you own or are authorized to access.
