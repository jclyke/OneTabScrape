# OneTab → Checkvist

A single-file web app that converts a [OneTab](https://www.one-tab.com/) HTML export into [Checkvist](https://checkvist.com)-ready markdown.

No installation, no build step, no backend. Just open `index.html` in any browser.

---

## What it does

OneTab lets you save all your open browser tabs into a list. When you export that list as HTML, this tool parses it and formats the tab groups as Checkvist tasks — ready to paste directly into any Checkvist list.

---

## Usage

1. In Chrome/Firefox, open OneTab and click **Export as HTML** — save the file.
2. Open `index.html` in your browser.
3. Drag the exported `.html` file onto the drop zone (or paste the raw HTML).
4. Choose your output mode (see below).
5. Click **Copy to clipboard** and paste into Checkvist.

---

## Output modes

### Multiline

Each tab group becomes a parent item with indented markdown links beneath it:

```
5/30/2026
  [Exciting AI Updates Weekly](https://youtube.com/...)
  [Makerspaces in São Paulo](https://chatgpt.com/...)

5/28/2026 – Microelectronics Commons
  [Vim configuration file setup](https://claude.ai/...)
  [Southwest Advanced Prototyping Hub](https://microelectronics.asu.edu/...)
```

Paste into Checkvist and choose **"Keep as one item"** to get a single nested task, or **"Split"** to get one top-level item per line (which splits out the individual tabs too).

Options:
- **Include date** — prepend the group's creation date
- **Include group title** — include the user-defined group name (when present)
- **Blank line between groups** — adds spacing for readability

### Single-line (paste & split)

Each tab group is formatted as a single pipe-delimited line:

```
OneTab Links | 5/30/2026 | [Exciting AI Updates Weekly](https://...) | [Makerspaces in São Paulo](https://...)
OneTab Links | 5/29/2026 | [0_ZLOG_2605_May_2026](https://...) | [I'm 71 With No Retirement!](https://...)
OneTab Links | 5/20/2026 | Microelectronics Commons | [Vim configuration file setup](https://...) | ...
```

Paste into Checkvist and choose **"Split"** — you get exactly **one item per tab group**, with all the links inline as clickable markdown. The `OneTab Links` prefix preserves context when you move groups into other lists.

This is the recommended mode for the typical workflow: archive a batch of tab groups by pasting once, then drag individual items into the appropriate Checkvist lists.

---

## Hosting

Because it's a single HTML file with no dependencies, you can host it anywhere:

- **GitHub Pages** — push to `main`, enable Pages in repo settings, deploy from root.
- **Any shared host** — upload `index.html` via FTP/SFTP.
- **Locally** — just double-click the file; no server required.

---

## OneTab export format

The parser targets the current OneTab export structure (2024+):

```
div.tabGroup
  div.tabGroupHeader
    div.tabGroupHeaderRow
      div.tabGroupTitleText   ← "N tabs" or a user-given name
    div.createdDate           ← "Created M/D/YYYY, H:MM:SS AM/PM"
  div.tabList
    div.tab
      a.tabLink[href]
```

If OneTab changes its export format, the parser falls back to a heuristic div scan. `chrome://` and other non-`http` links are silently filtered out.
