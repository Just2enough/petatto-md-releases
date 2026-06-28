# What Petatto.md Won't Do (Non-Goals)

> **Language**: [日本語](non_goals.md) / English

> This document lists what Petatto.md **intentionally does not implement / leaves out of scope for now**. These aren't missing features — they're deliberate scope boundaries that keep the core concept intact: edit in Obsidian, glance and check on the desktop.

## Premise: Petatto.md is a companion to Obsidian

Petatto.md is **not a replacement Markdown editor for Obsidian**. Editing happens mainly in Obsidian; the desktop side is where you glance at and lightly check notes as sticky notes. Features that break this premise are non-goals by default.

## Rendering scope

- Displaying images
- Rendering code blocks (including syntax highlighting)
- Tables, blockquotes, horizontal rules
- Click-through on wiki links `[[...]]`, and on links with schemes other than http/https (`mailto:` / `file:` / `obsidian://`, etc.)
- Emphasis markup inside headings

> What's supported: headings, paragraphs, lists, checkboxes, emphasis (`**bold**` / `*italic*` / `~~strikethrough~~`), and click-through on http/https links. Unsupported elements are shown as plain text. Rich reading is meant to happen in Obsidian. Frontmatter is not displayed either.

## Sticky note customization scope

- A UI for creating/editing user-defined palettes (only the two built-in palettes "Milky / Vivid", each a 4-color radio selection)
- Single-color palettes (slots=1) or palettes with 5 or more slots (N>4)
- Specifying color via frontmatter (e.g., `petatto-md-color: pink`)

> Only the body font size has a 5-step selection UI (11/12/13/15/17px). The DB schema, `Palette` type, and `list_palettes` API are already structured to support future user-defined palettes, but there's no editing UI for now.

## vault / file management

- Using multiple vaults at once (single vault only)
- Following external renames/moves (from Obsidian, Explorer, etc.); these are handled individually as `<old path removed> + <new path added>`
- Grouping or tagging sticky notes together
- Following symbolic links (symlinks). A `.md` that is a symlink to a real file is not recognized as a sticky note (the vault scan targets regular files only and does not follow links)

> App-initiated file renaming (from the sticky note header) is supported. Detecting a note that has "gone missing" (externally deleted/moved) while the app is running, and saving off your edits, is also supported. What's out of scope is *automatically following external renames*.

> **About symbolic links**: Petatto.md's vault scan targets regular files only and does not follow symlinks. If you use a symlink in Obsidian to share a note with another location, a `.md` that is a symlink to that real file can't be pinned as a sticky note (on Windows, creating a file symlink requires administrator privileges / Developer Mode, so this isn't expected to come up in normal use). Note that the **security containment** for a symlink pointing outside the vault (preventing a real file from escaping the vault) is implemented separately; this item is purely about the feature scope of "not treating a symlinked md as a sticky note."

## Platform / distribution

- mac / Linux support (Windows only)
- Syncing across multiple PCs (sharing positions, etc.)
- Code signing (distributed as an unsigned .msi; the first-run SmartScreen warning is accepted)
- Telemetry collection / automatic crash reporting (not done; logs are stored locally only, and you submit logs manually if there's a problem)

## Main window / editing experience

- Turning the main window into a dashboard / a sticky-note list table / a per-note operation UI (the main window stays focused on settings + a status summary; per-note operations are done from the tray menu and the sticky note header UI)
- Sidebar navigation / tab switching in the main window
- Editing in a separate window (it uses an in-window textarea; to be reconsidered from v1.1.0 onward)
- A 3-way merge screen for edit conflicts (only a two-choice "Overwrite / Discard and reload" dialog)
- A Ctrl+Enter shortcut for edit mode / precise caret placement at the double-click position

## i18n scope

The supported languages are ja / en (two languages). You switch manually from the settings screen, and the change takes effect after a restart (no instant switching). Three or more languages, locale-specific formatting (dates, numbers), and RTL are out of scope.

(Behind-the-scenes text that isn't part of the UI — logs and rare internal errors — is not localized.)

## Future considerations

These aren't "never" — they leave room for future consideration as long as they don't break the core concept.

- A UI for creating user-defined palettes (the DB schema, types, and API are ready)
- Editing in a separate window (aimed at improving the editing experience on very small sticky notes)
- An opt-in startup update check (off by default; only when you turn it on in settings does Petatto.md check GitHub's `latest.json` for a newer version at startup — nothing is sent beyond that update check)
- Customizing fonts and text color (a font selection UI and free body text color)
- Customizing line height (fixed to `line-height: 1.6`)
