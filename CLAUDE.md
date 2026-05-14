# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Goal

A single, self-contained `index.html` page that runs locally (opened directly from the filesystem, no server, no build step). The user pastes Markdown into the page; the page converts it and places the result on the clipboard so it can be pasted directly into a Confluence page editor with formatting preserved — heading hierarchy, bullet/numbered lists, tables, code blocks, links, bold/italic.

## Architecture

Everything lives in `index.html` — markup, CSS, and JS in one file. The script has two parts:

- **Markdown → HTML parser** (`parseMarkdown` / `buildList` / `parseInline`). A line-based block parser handles fenced code, headings, horizontal rules, GFM pipe tables, blockquotes (recursive), lists, and paragraphs. `parseInline` protects code spans and links via placeholder stashing *before* HTML-escaping, then applies emphasis regexes. Output is plain semantic HTML.
- **Wiring** — debounced live preview, and `copyForConfluence`, which writes a `ClipboardItem` with both `text/html` and `text/plain`, falling back to a `contentEditable` + `execCommand` copy when the async Clipboard API is unavailable.

## Key constraints

- **One file, no dependencies fetched at runtime.** It must work offline from `file://`. Don't add a build step or CDN links — fonts use system stacks for this reason; vendor/inline anything new.
- **The clipboard payload is the hard part, not the Markdown parsing.** Confluence's editor pastes richest from `text/html` clipboard content. The emitted HTML is clean semantic markup but is not guaranteed to round-trip — test against the real Confluence editor and adjust; nested lists and tables are the most likely spots to need tuning.

## Commands

No build/lint/test tooling exists. "Run" means opening `index.html` in a browser. If tooling is added, document it here.
