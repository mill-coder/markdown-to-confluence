# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Status

The repository is empty — no code exists yet. This file records the intended goal and constraints so the first implementation stays aligned with them.

## Goal

Build a single, self-contained `index.html` page that runs locally (opened directly from the filesystem, no server, no build step). The user pastes Markdown into the page; the page converts it and places the result on the clipboard so it can be pasted directly into a Confluence page editor with formatting preserved — heading hierarchy, bullet/numbered lists, tables, code blocks, links, bold/italic.

## Key constraints

- **One file, no dependencies fetched at runtime.** It must work offline from `file://`. If a Markdown parser library is used, vendor/inline it rather than linking a CDN.
- **The clipboard payload is the hard part, not the Markdown parsing.** Confluence's editor pastes richest from `text/html` clipboard content. The converter must produce HTML that Confluence's paste handler maps cleanly to its own elements (tables, lists, headings). Plan to test against the real Confluence editor and adjust the emitted HTML — semantic HTML alone is not guaranteed to round-trip.
- Use `navigator.clipboard.write()` with a `ClipboardItem` carrying `text/html` (and a `text/plain` fallback) so rich formatting survives the paste.

## Commands

No build/lint/test tooling exists yet. Until then, "run" means opening `index.html` in a browser. If tooling is added, document it here.
