# Hebrew-English-Epub/MOBI-converter

Local web app for converting `DOCX`, `TXT`, and `Markdown` manuscripts into `EPUB` or `MOBI` files, with support for Hebrew, English, and mixed-language content.

## Features

- Runs locally with Flask at `http://localhost:5000`
- Converts `.docx`, `.txt`, `.md`, and `.markdown` files to `.epub` or `.mobi`
- Detects title, author, chapters, and document language before conversion
- Lets you edit metadata before download
- Applies right-to-left layout automatically for Hebrew and other RTL languages
- Preserves basic bold and italic formatting from DOCX files

## Requirements

- Python 3.9+
- `pip`
- [Calibre](https://calibre-ebook.com) — **required only for MOBI output**

## Install

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install flask ebooklib python-docx markdown2
```

For MOBI output, install Calibre from [https://calibre-ebook.com](https://calibre-ebook.com) and make sure `ebook-convert` is available on your system. EPUB output works without Calibre.

## Run

```bash
python3 epub_app.py
```

Then open [http://localhost:5000](http://localhost:5000).

## How To Use

1. Open the app in your browser.
2. Drag in a source file or click to browse.
3. Review the detected title, author, chapters, and language.
4. Optionally edit metadata such as title, author, publisher, date, tags, rights, and description.
5. Select the output format — `EPUB` or `MOBI`.
6. Click `Convert & Download`.

## Supported Input Formats

### DOCX

- Detects chapters from Word heading styles, outline levels, and common chapter-title patterns
- Tries to preserve visible text even when tracked changes or revision markup exist
- Keeps basic bold and italic formatting

### TXT

- Splits chapters using common chapter-heading patterns such as `Chapter`, `Part`, `פרק`, and `חלק`
- Falls back to a single `Content` chapter when no headings are found

### Markdown

- Uses `# Title` as the book title
- Uses `## Heading` as chapter boundaries

## Metadata Fields

The UI lets you set these EPUB metadata fields:

- `title`
- `author`
- `language`
- `publisher`
- `date`
- `subject` / tags
- `rights`
- `description`

If `title` is left blank, the app uses the detected document title or filename.

## Language And Layout

- Hebrew content is exported with RTL direction and right-aligned text
- English content is exported with LTR direction and left-aligned text
- Mixed-language content is handled with EPUB styling intended to improve bidirectional text rendering

## Output Formats

### EPUB

No extra dependencies required. Works out of the box.

### MOBI

Requires [Calibre](https://calibre-ebook.com) to be installed on the machine running the server. The app uses Calibre's `ebook-convert` CLI tool under the hood — it first builds an EPUB, then converts it to MOBI. If Calibre is not found, the app will return an error with a link to download it.

## Notes

- Temporary uploaded and generated files are stored under `/tmp/epub_converter`
- Old temporary files are cleaned up automatically after about 10 minutes
- The app currently runs on port `5000`
- This project is a local utility app and does not include authentication or multi-user storage

## Troubleshooting

### Missing dependency error

Install the required packages again:

```bash
pip install flask ebooklib python-docx markdown2
```

### MOBI conversion fails — Calibre not found

Install Calibre from [https://calibre-ebook.com](https://calibre-ebook.com). On macOS you can also use Homebrew:

```bash
brew install --cask calibre
```

After installing, restart the app. EPUB output is unaffected and does not require Calibre.

### Empty or badly split chapters

- In `DOCX`, use Word heading styles for chapter titles when possible
- In `TXT`, make sure chapter headings are on their own lines
- In `Markdown`, use `##` for chapter headings

### Wrong text direction

Set the `Language` field manually before converting, for example `he` or `en`.
