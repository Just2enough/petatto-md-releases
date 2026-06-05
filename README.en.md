# Petatto.md

> **Language**: [日本語](README.md) / English

A Windows desktop app that pins Markdown files (.md files) from your Obsidian vault as **sticky notes on your desktop**.

- Editing is primarily done in **Obsidian**. Petatto.md is the place to **glance and check**.
- Light editing inside notes (plain text edits, checkbox toggles) is also supported.
- Designed for keeping TODOs, in-progress tasks, and notes you want to glance at on the desktop.

🌐 **Product site**: <https://just2enough.github.io/petatto-md-site/>

## Privacy

Petatto.md **does not collect, transmit, or store any telemetry, usage data, or analytics. All data stays on your machine.**

Starting with v0.1.1, update checks are provided via `tauri-plugin-updater`. This only queries a `latest.json` to see whether a newer version exists and does not include any usage reporting. The check runs only when you click the **"最新版を確認"** (Check for updates) button at the top right of the main window — there is no automatic check on launch.

## Screenshots

**Sticky notes (Milky palette / Vivid palette, 4 colors each)**

![Milky palette](docs/screenshots/post_it_milky.png)
![Vivid palette](docs/screenshots/post_it_vivid.png)

**Main window** (vault settings / appearance / maintenance)

![Main window](docs/screenshots/main.png)

**Edit mode**

![Edit mode](docs/screenshots/post_it_edit.png)

## Requirements

- Windows 10 / 11 (x64)
- [Microsoft Edge WebView2 Runtime](https://developer.microsoft.com/microsoft-edge/webview2/) (pre-installed on Windows 11 and recent Windows 10; if missing, the MSI installer auto-downloads it from Microsoft — requires an internet connection)
- [Obsidian](https://obsidian.md/) (not required, but assumed for creating and editing target Markdown files)

## Installation

1. Download the released `Petatto-md_x.x.x_x64_ja-JP.msi`.
2. Double-click the installer.
3. If SmartScreen warns you (the warning is displayed in Japanese: "**Windows によって PC が保護されました**" / "Microsoft Defender SmartScreen は認識されないアプリの起動を停止しました"), click **"詳細情報"** (More info) → **"実行"** (Run anyway). Because the MSI is not code-signed, the publisher field shows **"不明な発行元"** (Unknown publisher) — this is expected (use at your own risk).

   ![SmartScreen warning (actual screen)](docs/screenshots/windows_warning.png)
4. Follow the wizard (default install path: `C:\Program Files\Petatto-md\`).
5. Launch **Petatto.md** from the Start menu (Apps & Features and the Start menu show `Petatto-md`; the running window title is `Petatto.md`).

### Uninstallation

Uninstall **Petatto-md** from "Apps & features" or "Settings → Apps".
The DB and logs remain after uninstallation. To remove them completely, delete the folders listed under "Data Storage Locations" below.

## First Run and Vault Setup

1. The main window opens with no vault configured.
2. Click **"vault フォルダを選ぶ"** (Select vault folder) and pick the root folder of your Obsidian vault.
3. Petatto.md scans the vault and displays any Markdown file whose frontmatter contains `petatto-md: true` as a sticky note.

To change vaults later, click **"vault を変更..."** (Change vault...) in the vault card of the main window (a restart will be prompted).

> Note: The current UI labels are in Japanese. English UI is on the roadmap — 👍 [this issue](https://github.com/Just2enough/petatto-md-releases/issues/1) if you want it.

## Pinning a Markdown File

### Method 1: Pin via the tray menu

Right-click the Petatto.md tray icon → **"新規付箋化..."** (Pin new note...) → pick a Markdown file from your vault.
The `petatto-md: true` flag is automatically inserted into the file's frontmatter.

![Tray menu](docs/screenshots/tasktray.png)

### Method 2: Edit the frontmatter directly (in Obsidian)

Add YAML frontmatter at the top of the target file:

```markdown
---
petatto-md: true
---

# This Week's Tasks

- [ ] Prepare Monday meeting materials
- [x] File expense reports
- [ ] Reply to person X
```

When saved, Petatto.md auto-detects it and shows the sticky note on your desktop.

### Unpinning

Click the **"×"** button on the sticky note header. The `petatto-md: true` flag is removed and the note disappears (the file itself remains).

## Sticky Note Operations

| Operation | How |
|---|---|
| Move | Drag the header |
| Resize | Drag the edges/corners (min 180×80px) |
| Change color | Click the color button in the header — cycles through the 4 colors of the current palette (Milky / Vivid) |
| Change font size | Click the `A±` button in the header — popover `▲ Size N/5 ▼` provides 5 steps |
| Rename file | Double-click the file name in the header — inline edit (Enter to confirm, Esc to cancel) |
| Enter edit mode | Double-click the body |
| Exit edit mode | Click outside the edit area / press `Esc` |
| Toggle checkbox | Click `- [ ]` / `- [x]` |
| Click URL | Click http/https URLs in the body — opens in your default browser |
| Close (= unpin) | Click the **"×"** button in the header |

### Supported Markdown Elements

Elements rendered in view mode:

- Headings (`#` through `######`)
- Lists (`-` / `*` / numbered)
- Checkboxes (`- [ ]` / `- [x]`, clickable to toggle)
- Emphasis (`**bold**` / `*italic*` / `~~strikethrough~~` / `__bold__` / `_italic_` / `***bold italic***`)
- Links (`[text](url)`) + raw http/https URLs (click to open in default browser)
- Plain text

**Unsupported** elements (images, code blocks, tables, blockquotes, horizontal rules, etc.) are shown as plain text (no crash). For the intentionally out-of-scope areas, see [Non-Goals](non_goals.md).

## System Tray

Right-click the Petatto.md tray icon to see:

- **Note list** (visible `●` / hidden `○`): Click to toggle visibility
- **全付箋を中央に集める** (Gather all to center): Cascade-arrange stray notes near the center of the primary monitor
- **全付箋を最前面に表示** (Bring all to front): Temporarily lift all visible notes to the foreground
- **新規付箋化...** (Pin new note...): Pick a Markdown file from your vault to pin
- **設定...** (Settings...): Open the main window (vault operations, appearance, data folder access)
- **終了** (Quit): Exit the app (all notes fade out and close)

## Data Storage Locations

| Type | Path |
|---|---|
| Note positions, sizes, colors, font size (DB) | `%LOCALAPPDATA%\com.just2enough.petatto-md\petatto.sqlite` |
| Logs | `%LOCALAPPDATA%\com.just2enough.petatto-md\logs\petatto.log` (5MB × 5 generations rotated) |

The DB and logs live under the same parent folder. Click **"データフォルダを開く"** (Open data folder) in the main window to open this folder in Explorer (for bug reports; see "Troubleshooting & Crash Reports" below).

Petatto.md does **not** write per-note state (position, color, etc.) into the Markdown files themselves. Only the `petatto-md: true` flag in the frontmatter is the source of truth for "is this pinned?".

## Troubleshooting & Crash Reports

Petatto.md **does not send crash reports automatically** (zero-telemetry policy). Instead, users manually collect local logs and report them.

### How to Collect Logs

1. Click **"データフォルダを開く"** (Open data folder) in the main window — Explorer opens `%LOCALAPPDATA%\com.just2enough.petatto-md\`.
2. Inside the folder, copy `logs\petatto.log` (and rotated generations like `petatto.log.1` if needed).
3. **Check for personal information**: Logs may include file names and paths from your vault. Redact or edit as needed before sharing.
4. Report via one of the following:
   - **note article comments (Japanese)**: https://note.com/just2enough/n/n0e7c73253b99
   - **GitHub Issues (for bug reports with log attachments)**: https://github.com/Just2enough/petatto-md-releases/issues

### Common Issues

- **No notes shown**: Check that the vault is set correctly (Main window → vault card). Make sure files have `petatto-md: true` in the frontmatter.
- **A note went off-screen**: Use "全付箋を中央に集める" (Gather all to center) or "全付箋を最前面に表示" (Bring all to front) in the tray menu.
- **Old notes remain after vault change**: Restart the app (prompted by the main window).
- **SmartScreen warning**: When you first double-click the MSI, "**Windows によって PC が保護されました**" (Windows protected your PC) appears. Click **"詳細情報"** (More info) → **"実行"** (Run anyway). Because the MSI is not code-signed, the publisher field shows **"不明な発行元"** (Unknown publisher) — use at your own risk.

## License

Copyright © 2026 Just2enough. All rights reserved.

Petatto.md is **free for both personal and organizational use**. Redistribution, modification, and reverse engineering are prohibited.

See the bundled [LICENSE](LICENSE) file for full terms.

This software is built on top of many open-source components. For the full list of components and their license texts, see the bundled [THIRD-PARTY-NOTICES.txt](THIRD-PARTY-NOTICES.txt).

## Support the Development ☕

If you find this tool useful, your support helps me keep building it.

<a href="https://github.com/sponsors/Just2enough">
  <img src="https://img.shields.io/badge/Sponsor-EA4AAA?style=flat&logo=github-sponsors&logoColor=white" alt="GitHub Sponsors" />
</a>
<a href="https://www.buymeacoffee.com/just2enough">
  <img src="https://img.shields.io/badge/Buy_me_a_coffee-FFDD00?style=flat&logo=buy-me-a-coffee&logoColor=black" alt="Buy Me a Coffee" />
</a>
