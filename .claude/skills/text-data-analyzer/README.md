# text-data-analyzer

A Claude Code skill for analyzing, transforming, and extracting structured information from text data stored in CSV and XLSX files.

## What it does

Use this skill when you have a spreadsheet or CSV with text columns and need to:

- **Extract attributes** from free-text fields (brand, color, size from product names; postal codes from addresses)
- **Generate and test regexes** against real data with match/non-match tables before applying to the full dataset
- **Fuzzy-match or deduplicate** text across columns or files (e.g. find the same company under slightly different names)
- **Normalize text** (strip whitespace, Unicode normalization, casefolding, dedup spaces)
- **Categorize or tag rows** by text content using keywords, regexes, or clustering
- **Find common phrases** or group rows by shared substrings

Typical input: up to ~10,000 rows, text fields of 100–200 characters (product names, addresses, descriptions, company names). Supports Latin, Cyrillic, Arabic, Armenian, CJK, and mixed-script data.

## How to trigger it

Ask naturally — the skill triggers on any signal that you want to process, clean, extract from, or match text stored in a tabular file:

```
"extract brand and color from the product_name column in products.csv"
"regex for Polish postal codes from addresses.xlsx"
"find duplicate company names between these two columns"
"clean up the text in the description column"
"what are the most common phrases in my product names?"
"match source_company to target_company despite spelling differences"
```

## Workflow

Every task runs through three mandatory phases:

1. **Analysis** — loads the file, shows column names, data types, row count, and a representative sample. Identifies the script(s) used in the data.
2. **Approach proposal** — proposes the algorithm (regex, fuzzy match, normalization steps, etc.) and waits for your approval before writing any code.
3. **Execution** — runs the code, shows a results summary (matches, failures, before/after), and saves the output file.

## Output

- A result file (XLSX or CSV) saved alongside the input, with a descriptive suffix (e.g. `products_extracted.xlsx`)
- Optionally a reusable Python or T-SQL script you can run independently
- A summary of successes and failures — rows that didn't match are listed explicitly

## Requirements

The skill runs Python scripts locally. Install the dependencies before use:

```bash
pip install -r .claude/skills/text-data-analyzer/requirements.txt
```

**Python >= 3.14** is required.

### Dependencies (`requirements.txt`)

| Package | Purpose |
|---|---|
| `pandas` | Reading/writing CSV and XLSX, dataframe operations |
| `numpy` | Numeric support for pandas |
| `regex` | Unicode-aware regex (drop-in for `re`, with better Unicode support) |
| `openpyxl` | Reading and writing `.xlsx` files |
| `rapidfuzz` | Fast fuzzy string matching (`token_sort_ratio`, `process.extract`) |
| `chardet` | Detecting file encoding for CSV files |
| `Unidecode` | Transliteration (e.g. Cyrillic to Latin) |
| `xlsxwriter` | Writing formatted XLSX output files |

### Verifying the installation

```bash
python -c "import pandas, openpyxl, rapidfuzz, chardet, unidecode, xlsxwriter; print('OK')"
```
