# Hebrew-English-Epub-converter

Local web app for converting `DOCX`, `TXT`, and `Markdown` manuscripts into `EPUB` files, with support for Hebrew, English, and mixed-language content.

## Features

- Runs locally with Flask at `http://localhost:5000`
- Converts `.docx`, `.txt`, `.md`, and `.markdown` files to `.epub`
- Detects title, author, chapters, and document language before conversion
- Lets you edit EPUB metadata before download
- Applies right-to-left layout automatically for Hebrew and other RTL languages
- Preserves basic bold and italic formatting from DOCX files

## Requirements

- Python 3.9+
- `pip`

## Install

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install flask ebooklib python-docx markdown2
```

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
5. Click `Convert & Download EPUB`.

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

### Empty or badly split chapters

- In `DOCX`, use Word heading styles for chapter titles when possible
- In `TXT`, make sure chapter headings are on their own lines
- In `Markdown`, use `##` for chapter headings

### Wrong text direction

Set the `Language` field manually before converting, for example `he` or `en`.
