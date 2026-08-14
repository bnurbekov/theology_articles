# theology_articles

Source content for [reformedbaptists.net](https://reformedbaptists.net) — Gospel
and Reformed theology articles, in whatever languages they've been translated
into. This repo holds the text; the site itself (build, rendering, i18n) lives
in a separate `commonplace` repo, which fetches everything here over GitHub's
raw-content API **at deploy time only**. Nothing here is served directly, and
editing a file in this repo doesn't change the live site by itself — see
"Publishing changes" below.

## Adding or editing an article

Each article is one Markdown file, named `<slug>.md` (the slug becomes the
article's URL and its English-derived fallback title, e.g.
`london_baptist_1689.md` → `/london_baptist_1689`). The file's first `# `
heading is pulled out as the article's title and shown as the page's own
`<h1>` — don't also repeat it as a `##` inside the body.

Longer, chaptered documents (like the confession) use:

```
# Document Title

## Chapter 1: Chapter Title

**1.** First paragraph...

**2.** Second paragraph...
```

The `## Chapter N: Title` / `**N.**` structure isn't required by the site's
renderer — it's plain Markdown either way — but it's what the annotation
desk (see below) and the table-of-contents feature both key off of, so keep
it for any document meant to support either. A shorter article (like
`resources.md`) can just be ordinary prose with `##` section headings.

## Translations

Add `<slug>_<lang>.md` alongside the English original — same slug, a
language suffix before `.md`. Supported codes: `ru`, `kk` (`kz` is accepted
as an alias and normalized to `kk`), `uk`, `es`, `de`, `fr`, `ar`. A
translation's own title is pulled from its own leading `# ` heading, so
translated articles get a translated title too, not just translated body
text.

Some translations lead with an AI-translation disclaimer before the real
title, either as its own `# ` heading or a `> [!NOTE]`-style blockquote
admonition — either is recognized and skipped in favor of the real title
underneath it, in any of the languages above.

## Ordering articles

Add a file named `ORDER` (no extension) to the repo root, one slug per
line, to control the order articles appear in on the site:

```
gospel_1
gospel_2
reformed_baptist_doctrines_overview
london_baptist_1689
resources
```

Blank lines and lines starting with `#` are ignored. Any article found in
the repo but missing from `ORDER` is appended afterward, sorted
alphabetically — so forgetting to list a new article never makes it
disappear, it just sorts to the end.

## Excluding files from the build

Add a file named `IGNORE` (no extension) to the repo root, same
one-per-line / blank-lines-and-`#`-comments-skipped format as `ORDER`, to
keep specific files out of the site entirely without deleting them from
the repo — e.g. to pull a `_notes.json` file off the live site while it's
still being cleaned up:

```
# still reviewing this
london_baptist_1689_notes.json
```

Matched against both a file's full path and its bare filename.

## Voice-over audio

Optional, per article and language. Drop a `.wav` file next to the
markdown, same `<slug>.wav` / `<slug>_<lang>.wav` naming convention as
translations (e.g. `gospel_1.wav`, `london_baptist_1689_ru.wav`). The site
only records the file's raw GitHub URL at build time and streams it
straight from GitHub when a visitor presses play — the audio itself is
never fetched or embedded, since `.wav` files are too large for that. An
article with no matching `.wav` just shows no player.

## Review notes (proposed edits + commentary)

Optional, per article and language. Drop a `<slug>_notes.json` (or
`<slug>_notes_<lang>.json` for a translation's own notes) file next to the
markdown — proposed edits and commentary anchored to specific text in that
document, grouped by chapter. The site shows these on the article's own
`/<slug>/<lang>/notes` page, and optionally inline in the article body as
a word-level diff.

Each entry anchors to a quote via exact text plus a bit of surrounding
context (so the same phrase occurring twice in a document resolves to the
right occurrence), and can either be a plain note or a proposed edit
(`from`/`to`). An edit can also span a range of paragraphs — e.g. proposing
that an entire chapter be removed — via a `range` object with its own end
anchor.

Rather than hand-writing this JSON, use the **annotation desk** — an
in-browser tool (linked from the article page on the site itself, or open
`lbc-annotation-desk.html` from the `commonplace` repo directly) for
selecting text in the confession, attaching a note or proposed edit, and
exporting a `notes.json` in the right shape. It's currently built
specifically around `london_baptist_1689.md`'s chapter/paragraph
structure described above.

## The gospel document

`gospel.md` (and its translations, `gospel_ru.md`/`gospel_kk.md`/etc.) is
special-cased by the site: it always renders in a pinned banner at the top
of the homepage rather than in the ordered article list, regardless of
whether it's mentioned in `ORDER`. `gospel_1.md`/`gospel_2.md` are
ordinary articles despite the shared name prefix — only the bare
`gospel[_lang].md` files get the banner treatment.
