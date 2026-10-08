# Tools

## build_offer_bank_docx.py

Converts any structured markdown deliverable (offer bank, niche bible, deep dive, competitive intel, etc.) into a polished `.docx` using the same brand styling we use for member-facing artifacts.

Despite the name, this script handles **any** markdown — not just offer banks. The styling rules are generic to our deliverable format.

### Usage

```bash
# Output saved next to input as .docx
python3 tools/build_offer_bank_docx.py path/to/garage_door_offer_bank.md
python3 tools/build_offer_bank_docx.py path/to/window_replacement_niche_bible.md

# Or specify output path explicitly
python3 tools/build_offer_bank_docx.py input.md output.docx
```

### Requires

```bash
pip3 install python-docx
```

### What it renders

| Markdown | Rendered as |
|---|---|
| `# Title` | H1 (22pt, dark navy, bold) |
| `## Section` | H2 (16pt, dark navy, bold, spaced before) |
| `### Item` | H3 (13pt, slate blue, bold) |
| `> "quote"`<br>`> — Attribution` | Indented italic blockquote with grey left border + small grey attribution line |
| `**Label:**` body | Bold label inline with body text |
| `- item` | Bullet list |
| `1. item` | Numbered list |
| Markdown table | Styled table with dark navy header row and white text |
| `---` | Horizontal rule |
| `**bold**`, `*italic*`, `` `code` `` | Inline formatting preserved |

### Workflow for a Niche Bible

The Niche Bible (Stage 4 of [02_niche_research_and_validation.md](../02_niche_research_and_validation.md)) is built by:

1. Stitching the validation summary + Phase 0 deep dive + offer bank into one master markdown file at `[niche]_niche_bible.md`.
2. Running this script on it.

The script handles all formatting — heading hierarchy, blockquotes for ad-copy examples, tables for AOV / channel CPL / operator tier breakdowns, source attribution lines.

### Workflow for any other deliverable

1. Write the markdown using our standard patterns (the [garage door offer bank](../garage_door_offer_bank.md) is a good reference).
2. Run the script.
3. Hand the `.docx` output to the member.

No script edits required for new niches or new deliverable types — the markdown is the single source of truth.
