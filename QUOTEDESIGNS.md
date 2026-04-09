# QUOTEDESIGNS — Master Reference

> **Invoke this file at the start of every Claude Code session to restore full context.**
> Say: *"Read QUOTEDESIGNS.md"* before pasting any new quote data.

---

## What This Project Does

Generates professional, graphically designed HTML + PDF quotation documents for **Sammy Quotations**.

- **Claude Web** collects and structures all raw quote data (company info, items, prices, dates, ref numbers)
- **Claude Code** (this environment) receives that structured data and generates polished HTML + PDF files
- Each company gets **one design identity** — colours, logo mark, letterhead layout
- When a company reappears, only the data changes; the design stays identical
- New companies always get a fresh, unique design that has never been used before

---

## Workflow

```
1. Go to Claude Web → paste the DATA COLLECTION PROMPT (see docs/claude-web-prompt.md)
2. Fill in all quote details when Claude Web asks
3. Claude Web outputs a structured JSON/text block
4. Come to Claude Code → say "Read QUOTEDESIGNS.md"
5. Paste the structured data block from Claude Web
6. Claude Code generates the HTML + PDF into quotes/output/
7. Review → done
```

---

## File Structure

```
quote-designs/
├── QUOTEDESIGNS.md              ← THIS FILE (master memory, invoke every session)
├── quotes/
│   ├── designs/                 ← Final HTML source files (one per quotation)
│   │   ├── 01_Bridgeford_Furniture.html
│   │   ├── 02_Epic_Furniture.html
│   │   └── ...
│   └── output/                  ← Generated PDFs (auto-created by Claude Code)
│       ├── 01_Bridgeford_Furniture.pdf
│       └── ...
├── templates/
│   └── (reserved for shared CSS/SVG snippets if needed in future)
├── docs/
│   ├── claude-web-prompt.md     ← Prompt to paste into Claude Web for data collection
│   ├── company-registry.md      ← All companies, their design colours, and quote history
│   └── numbering.md             ← Next available quote number
└── .gitignore
```

---

## PDF Generation Command

Run this in Claude Code after HTML files are ready (Chrome must be installed):

```bash
CHROME="/mnt/c/Program Files/Google/Chrome/Application/chrome.exe"
OUTDIR="/mnt/c/Users/admin/Documents/quote-designs/quotes/output"
SRCDIR="/mnt/c/Users/admin/Documents/quote-designs/quotes/designs"

for f in "$SRCDIR"/*.html; do
  name=$(basename "$f" .html)
  WIN_HTML=$(wslpath -w "$f")
  WIN_PDF=$(wslpath -w "$OUTDIR/${name}.pdf")
  "$CHROME" --headless --disable-gpu --no-sandbox \
    --print-to-pdf="$WIN_PDF" \
    --print-to-pdf-no-header \
    --no-pdf-header-footer \
    "file:///$WIN_HTML" 2>/dev/null
  echo "Generated: ${name}.pdf"
done
```

To regenerate a single file only:
```bash
name="01_Bridgeford_Furniture"
# ... same Chrome command with specific name
```

---

## Design Rules (CRITICAL — always follow these)

1. **Each company gets a unique design** — different letterhead layout, different SVG logo mark concept, different colour palette
2. **No two designs look the same** — vary: logo shape (shield / hexagon / circle / diamond / octagon / split-block / angular), header layout (full-banner / split-diagonal / stacked-blocks / two-tone), accent colour placement
3. **Colours per company are fixed forever** — see `docs/company-registry.md`
4. **When a company reappears**: load their existing HTML file, update ONLY the data (items, qty, prices, ref, date, subject, grand total, VAT note) — do NOT touch letterhead, colours, logo, or footer
5. **Quote numbering** is sequential across all companies — see `docs/numbering.md`
6. **All prices include VAT at 16%** unless explicitly stated otherwise
7. **PDF footer/header must always be suppressed** — `@page { margin: 0 }` in CSS + `--no-pdf-header-footer` Chrome flag
8. **Filenames** follow the pattern: `{NN}_{CompanySlug}_{Category}.html` where NN is the sequential quote number

---

## Data Fields Required Per Quotation

| Field | Example |
|-------|---------|
| Quote Number | 10 |
| Company Name | Bridgeford Enterprises Ltd |
| Company Slug | Bridgeford |
| Address / Location | P.O. Box 1572 – 90200, Kitui, Kenya |
| Email | bridgefordenterprisesltd@gmail.com |
| TO (recipient org) | Ministry of Foreign Affairs – Kenya |
| ATTN (person/office) | Special Envoy Ateker – Old Mutual, 316 Chambers |
| REF Number | BEL/QTN-002/APR/2026 |
| Date | 9 April 2026 |
| Subject | Supply and Delivery of ... |
| Category | Furniture / Fittings / Equipment / Kitchen / Mixed |
| Items | Array: description, qty, unit price, total |
| Grand Total | KES 2,119,000 |
| Closing line | (optional custom sign-off sentence) |

---

## Existing Companies & Their Design Identity

See `docs/company-registry.md` for full details.

| # | Company | Primary | Accent | Logo Style |
|---|---------|---------|--------|------------|
| 01 | Bridgeford Enterprises Ltd | #1B3A6B (navy) | #C8A84B (gold) | Square frame, "BE" monogram |
| 02 | Epic Capital Ltd | #1A5C3A (green) | #F4A61D (amber) | Circle rings, "EPIC/CAPITAL" |
| 03 | Adtech Agencies | #6B1B1B (crimson) | #D4AF37 (gold) | Shield heraldic, "ADT" |
| 04 | Monata Enterprise Ltd | #1A2C6B (blue) | #E87722 (orange) | Hexagon, "MNT" |
| 05 | Wespan Agencies Ltd | #0D5C5C (teal) | #F0C040 (yellow) | Compass circle, "WAL" |
| 06 | Roshani Ventures | #3D1A6B (purple) | #30C5A0 (teal) | Diamond rotated, "RV" |
| 07 | Hayward Agencies Ltd | #0D3B6E (navy) | #E8402A (red) | H-block angular, "HAL" |
| 08 | Mercpat Investment Ltd | #1E4D1A (dark green) | #E8B020 (gold) | Circle filled "M" |
| 09 | Crisima General Supplies | #4A1A60 (violet) | #1AB8E8 (cyan) | Octagon, "CGS" |

---

## Notes

- The original (pre-redesign) files are kept in `C:\Users\admin\Documents\Sammy Quotations\` as backup
- Always commit after generating new quotes: `git add . && git commit -m "feat: add quote NN - CompanyName"`
- The repo is at: `C:\Users\admin\Documents\quote-designs\` (init with `git init` if not yet done)
