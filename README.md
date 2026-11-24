# Named Entity Highlighter — Chrome extension

Finds the named entities on the open page, highlights them by colour, adds a checkbox after each one and
exports the selection (or everything) to CSV. **100% local analysis**: no text is ever sent to the Internet.
(Documentation en français : `LISEZMOI.md`.)

| Colour | Type |
|---|---|
| Yellow | People |
| Fuchsia | Works (books, films, paintings, albums…) |
| Green | Institutions / organisations |
| Blue | Places |
| Orange | Events |
| Lavender | Dates |
| Grey | Other proper nouns (off by default) |

## Interface language

The interface follows the browser language: **French** if Chrome is in French, **English** otherwise.
Strings live in `_locales/<lang>/messages.json`; to add a language, copy `_locales/en` to the language
code (e.g. `_locales/es`) and translate the `message` values. Entity detection is multilingual
regardless of the interface language.

## Installation (developer mode)

1. Unzip the archive into a permanent folder.
2. Open `chrome://extensions` and turn on **Developer mode** (top right).
3. Click **Load unpacked** and choose the `ner-surligneur` folder.
4. Pin the extension icon in the toolbar.

## Usage

- **Settings** (click the icon): entity types to detect, CSV separator, show/hide checkboxes.
- **Analyze**: *Analyze this page* button or **Alt+Shift+E**.
- **In the page**: each highlighted entity is followed by a checkbox; checking one occurrence checks
  every occurrence of the same entity.
- **Floating panel** (drag it by its header): **Export all** (top right), **Export selection** (bottom),
  per-type toggles with counts, a filterable list (click a name to scroll to it; use the menu to
  **correct the type** or **ignore** an entity), and **Rescan** for dynamic pages.

The CSV (UTF-8 with BOM) contains `Entity, Type, Occurrences, Page`.

## How detection works

Rule- and dictionary-based NER, no machine learning: capitalised word sequences with name particles,
multilingual trigger words (University, Museum, Festival, street, lake…), titles (Dr, Mrs, president…),
first-name and place gazetteers, initials, acronyms, quotation marks and `<cite>`/`<i>`/`<em>`, context
(“the novel X”, “X (1802–1885)”), page-level consistency, CJK word segmentation and the page's
schema.org data. All word lists are in `src/data.js`.

Permissions: `activeTab`, `scripting`, `storage` only — the extension acts only on the tab where you
trigger it.
