# quote-designs

Graphically designed HTML + PDF quotation generator for Sammy Quotations.

## Quick Start

1. Collect quote data → see `docs/claude-web-prompt.md` (paste into Claude Web)
2. Open Claude Code → say `"Read QUOTEDESIGNS.md"`
3. Paste structured data → Claude Code generates files automatically

## Key Files

| File | Purpose |
|------|---------|
| `QUOTEDESIGNS.md` | Master reference — invoke at start of every session |
| `docs/claude-web-prompt.md` | Prompt to paste into Claude Web for data collection |
| `docs/company-registry.md` | All company design identities and quote history |
| `docs/numbering.md` | Sequential quote numbering log |
| `quotes/designs/` | HTML source files |
| `quotes/output/` | Generated PDFs |

## Design Principles

- Every company has one locked design identity (colours, logo, layout)
- Returning companies: data updated only, design never changed
- New companies: unique design never seen before in this project
- All PDFs suppress browser headers/footers — clean print output only
