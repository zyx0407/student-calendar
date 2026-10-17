# Student Calendar · 大学生日历

[简体中文](README.md) · English

A timeline of Chinese exams and certificates: 22 items laid out on a **rolling 12-month** month axis, one card per exam. Each card carries four facts — **official site · fee · registration window · test dates** — and you can filter by track, category and your own status.

Live → <https://work1.zyx0407.com/>
Static · zero dependencies · no build · no third-party libraries (even the QR encoder is hand-written)

![screenshot](screenshot.png)

## What's on the page

- **Month axis** — 12 equal cells, each showing "reg N · exam N": how many items have an open registration window and how many test days fall in that month. The current month gets an amber line. The axis pins to the top while scrolling and lets go at the end of the list, without covering the footer. Click a cell to jump to that month's group.
- **Cards** — grouped by month, sorted by category, then by "next thing happening". Card = name + abbreviation + category + countdown → registration window / test dates / fee → official site → who it's for → a plain-language note → data-as-of · official-site check · status · share.
- **Filters** — track (postgraduate / civil service / certificates) · category (language / computing / finance & business / food & nutrition / teaching / emerging skills / study abroad) · status · search. Search is two-stage: first match names, abbreviations, categories and official-site names; only if nothing hits does it fall back to the note text, and the list header says so.
- **Status marks** — mark each item as preparing / obtained / skipping; stored in `localStorage` (key `kaozheng.status.v1`), on this device only, never uploaded.
- **Share** — the share sheet is one finished card (registration · test · fee · official site · QR code), saveable as a 2× PNG, or copy a link with `?card=<id>` that lands on that card and highlights it.
- **Calendar export** — "Export calendar" in the top right writes the **current filter** to `.ics` (registration start / deadline / tests from today onward, all-day events, with official site and data-as-of) for importing into a phone calendar.
- **Mobile** — the whole page reflows for narrow screens: one-line header, tighter hero, horizontally scrolling filter rows, a 73px sticky timeline strip, single-column card facts, and a fully scrollable share sheet.

## Quick start

Double-click `index.html`; it runs from `file://`. For a local server:

```powershell
npx serve .
```

## How the data is kept

- **Data and page are separate** — `数据.js` holds data only (on `window.KAOZHENG`, no DOM); `app.js` renders and handles interaction. Editing data is editing the page.
- **Rolling window** — with `元信息.时间轴起点` set to `"自动"`, the page counts 12 months from the current month and rolls forward on the 1st of each month, so no data edits are needed at month end. The data currently reaches 2029-01.
- **Sourcing** — every item is tagged with `来源` (`官方` official / `往年规律` historical pattern) and `数据截至` (data as of). A "historical pattern" item must explain which year it is based on.
- **No invented dates** — if the official date isn't published, the item is marked `未公布: true` with last year's date plus a note "reference month XXXX, pending official announcement".
- **Official domains only** — training-school sites are not included.

## The QR encoder is hand-written

The `QR` module inside `app.js`, with zero dependencies: byte mode (UTF-8) + EC level M + versions 1–10, including BCH(15,5) format info, GF(256) Reed–Solomon (primitive polynomial `0x11D`), and 8 masks chosen by penalty score. Codes are generated only when the share sheet opens, so 22 cards don't draw 22 codes on load.

## Visual design

Same system as the main site: warm paper by day, dark gallery by night; follows the system by default, manual switch is remembered. One accent colour (amber) used only for indices, links, underlines and the current state. Three type styles — Source Han Serif (headings), Source Han Sans (body), FangSong italic (side notes).

## URL parameters

| Param | Effect |
| --- | --- |
| `?theme=day` / `?theme=night` | Force a palette (not saved) |
| `?cat=语言` | Pre-filter a category |
| `?card=<id>` | Scroll to that card and highlight it (what the QR code contains) |
| `?still=1` | Disable transitions and animations |

## Files

| File | What it is |
| --- | --- |
| `index.html` | Single-page skeleton: header · meta line · filters · month axis · card groups · footer |
| `数据.js` | All exam data (`window.KAOZHENG`), with collection rules in the file header |
| `app.js` | Month axis / card rendering / filters / status marks / countdown / share card / `QR` module |
| `style.css` | Metrics, both palettes, pills and hover rules |
| `theme.js` | Day/night switch |
| `CNAME` | Custom domain `work1.zyx0407.com` (GitHub Pages reads it as the Host) |

## Notes

- No third-party scripts, no external requests, no analytics; everything runs in the front end.
- Test dates, fees and registration windows can be changed by the authorities at short notice — **always check the official exam-site announcements**. The "data as of" date on each card is when that item was last verified.
- No open-source license is attached: code and curated data are all rights reserved.

## Changelog

- **v1.1 (2026-10-17)** — full mobile reflow (header / hero / filters / sticky timeline / groups / cards / share sheet); share card and saved image redesigned (serif title, paper layout, ellipsis for long text, no mobile overflow); "Export calendar" `.ics`; the "阿玖 zyx0407" header now links back to the main site; version shown in the footer.
- **v1.0 (2026-09-27)** — first release: 22 exams × a rolling 12-month timeline.
