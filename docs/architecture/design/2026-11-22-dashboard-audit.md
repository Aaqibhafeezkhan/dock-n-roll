# Dashboard audit, 2026-11-22

The current dashboard measured and scored on eight axes, each finding tied to a need of the [brief](brief.md) (#136), step 2 of the design study (epic #134).

The audit judges; it fixes nothing. Each finding names the study step that takes it. Ambition is the replacement world (brief § Ambition): the current look is evidence, not a base to polish.

## Method

| Point | Value |
|---|---|
| Build | Packaged app (`dist/linux-unpacked`, `main` at `c90c54b`), its own instance under Xvfb, driven over the Chrome DevTools Protocol. |
| Data | A fake Docker Engine on a Unix socket, a fake `ss`, a temporary pin file. Demo stacks only: no screenshot shows a real project. |
| Fixtures | `main`: 10 rows, 2 groups, every status, one long name. `mirror`: the 8 rows and 4 groups listed on the workstation today, same name lengths, neutral names. `peak`: 12 rows, 4 groups (the volume of the brief). Also: empty, pin-file errors, Docker down (at start, and while open). |
| Sizes | 320, 360 × 260 (the 200 % equivalent of 720 × 520), 440 to 680 by steps of 20, 720 × 520 (default), 960 × 640; light, dark, forced colours. |
| Measures | In-page script: bounding boxes, overflow, target sizes, computed fonts, contrast of every text and badge pair, rows above the fold, tab stops. |
| Real volume | Read-only count on the workstation's Docker, figures only. |
| Surface mode | Operate (brief § Users and use scene). |

Screenshots: [`audit/`](audit/), named `<screen>-<width>-<theme>[-forced][-fold].png`; `-fold` is the window as opened, the others are the full page.

## Scores

Scale 0 to 5: 0 absent, 1 broken, 2 weak, 3 adequate, 4 good, 5 excellent.

| Axis | Score | Evidence in one line |
|---|---|---|
| Hierarchy | 2 | The state is the smallest thing on a row; the black Start button is the largest. |
| Typography | 2 | One system family, three sizes in `px` (13, 14, 20), name and status at the same size. |
| Layout | 2 | 3 rows of 8 above the fold at the default size; 187 px of chrome before the first row. |
| Colour | 3 | Every text pair passes AA (lowest 5.11:1); red marks a stopped service as well as a failure; no identity. |
| States | 2 | Failure, pending and pin errors exist; loading, empty logs and Docker-down on the Running tab do not. |
| Motion | 2 | No transition or animation at all: nothing harms, nothing helps a state change to be seen. |
| Accessibility | 3 | Landmarks, live announcements, keyboard and focus are there; zoom is impossible, targets are 30 px high. |
| Content resilience | 2 | A long name breaks letter by letter from 440 to 600 px; Compose errors are shown raw. |
| **Total** | **18 / 40** | |

## Baseline of the success criteria

| Criterion | Today | Evidence |
|---|---|---|
| S1 State read in 2 seconds | Not met | 720 × 520: 3 rows of 8 fully visible (`mirror`), 3 of 12 (`peak`). The summary "2 of 8 on" is visible; a fault below the third row is not. |
| S2 Minimal at rest | Not met | 4 or 5 controls per row. `main`: 48 buttons, 19 disabled (40 %). `mirror`: 45 buttons, 14 disabled. |
| S3 Actions stay close | Met for what exists | Start, stop, restart, logs, open, group start and stop: 1 click. Other container in the logs: 2. A3 (stop everything) does not exist: one click per group and per ungrouped row, up to 7 on `mirror`. A5 does not exist. |
| S4 Every state designed | Not met | Missing: loading, empty log stream, Docker down on the Running tab. Hover exists on rows and buttons; focus exists. |
| S5 Accessible | Partly | Contrast passes; findings F17 to F23 remain. |
| S6 Reflow | Not met | Services tab: no horizontal overflow from 320 to 960 px, but the name column collapses (F9). Running tab: 22 px cut at 320 px (F12). 200 % zoom: not reachable (F17). |

## Findings

Severity (skill `ui-review`): **Must** breaks a Must need or a WCAG 2.2 AA criterion; **Should** weakens a need; **Nice** is polish. "Step" is the study step that settles it.

### Hierarchy

| # | Severity | Evidence | Finding | Need | Step |
|---|---|---|---|---|---|
| F1 | Must | Badge 10 px; status word 14 px regular, muted (6.11:1); name 14 px semibold. `audit/list-720-light-fold.png` | The status, task 1 of the brief, is the least visible element of a row. | V1, V2 | #139, #140 |
| F2 | Must | Primary Start: 57 × 30 px, solid `#1f2328`, on every stopped row. | The strongest shape on screen is an action, not a state: the eye lands on Start before it reads "off". | V1 | #139 |
| F3 | Should | Summary "2 of 10 on": 14 px muted, top right corner, no fault count. | The global state says how many run, not whether something is wrong. | V1 | #138 |
| F4 | Should | Group head: 65 px, of which 38 px for a "Start all / Stop all" line always shown. | Group actions weigh more than the group state ("0 of 2 on", muted). | V3, A2 | #139 |

### Typography

| # | Severity | Evidence | Finding | Need | Step |
|---|---|---|---|---|---|
| F5 | Should | Computed fonts: 20 px/700 (title), 14 px/400 and 600, 13 px/400; `system-ui` (Noto Sans here), `ui-monospace` falls back to DejaVu Sans Mono. | Three sizes, two of them 1 px apart: the scale carries no hierarchy; weight alone separates name from status. | V2, goal 6 | #140 |
| F6 | Must | Every size in `px`; root 16 px. | Sizes do not follow a user font setting. With F17, text cannot be enlarged at all. | X1 | #141 |

### Layout

| # | Severity | Evidence | Finding | Need | Step |
|---|---|---|---|---|---|
| F7 | Must | 720 × 520, `mirror`: rows end at 252, 317, 459, 601, 742, 819, 884, 949 px. 3 visible at 520 px high, 4 at 640 and 700, 7 at 900; all 8 need 949 px. `peak`: 1232 px. | The default window shows 3 services of 8. Width does not help: 960 × 640 shows 4. | V1 (S1) | Q1, Q2, #139 |
| F8 | Should | Header 28 px, tabs 38 px, group head 65 px, paddings: 187 px before the first row (36 % of 520). Row: 65 px, 85 px with a reason, 158 px with a failure. | Chrome and group heads take more height than the rows they introduce: 4 groups cost 260 px. | V1, V3 | #139 |
| F9 | Must | Name column width: 124 px at 600, 84 at 560, 44 at 520, 24 at 500, 4 at 480, 67 at 460 (after the wrap). `audit/list-500-light.png`, `audit/squeeze-480-light-fold.png` | From 440 to 600 px the actions keep their width and the name is squeezed (#127 confirmed). Short names break too ("API", "Shop": one letter per line at 500 px). | X1 (S6) | #139 |
| F10 | Should | 320 px: rows 144 to 201 px, 4 to 5 levels, page 1951 px, 1 row above the fold. 360 × 260: 0 rows above the fold, page 1612 px. `audit/list-320-light-fold.png`, `audit/list-360-light-fold.png` | Narrow and zoomed layouts reflow without overflow but show almost nothing per screen. | X1 | #139 |
| F11 | Should | Logs panel: 316 px high, opens under its row. Rows 6 to 10: the panel starts at 495 to 531 px in a 520 px window. | Logs of a low row open below the fold, with no scroll to them. | D2 | Q4, #138 |
| F12 | Must | Running tab, 320 px: containers table 279 px wide in 257 px, right edge 22 px past the content, no scrollbar. State column 39 px: "running" breaks in two. Page 3023 px (1615 px at 720). `audit/running-320-light-fold.png` | The Project column is cut (#132 confirmed); the tables do not adapt, they shrink. | V4, X1 (S6) | Q3, #139 |
| F13 | Should | Running tab: two tables, no sort, no filter, no link to a service row, 15 containers and 8 ports listed flat. | The tab answers "what else runs" only by reading every line. | V4 | Q3, #138 |

### Colour

| # | Severity | Evidence | Finding | Need | Step |
|---|---|---|---|---|---|
| F14 | Should | `mirror` at rest: 6 red badges of 8, none in fault. Failure text uses the same red family (`#cf222e`). | Red means "stopped" and "failed". At a glance a healthy workstation looks broken, and a real fault does not stand out. | V1 | #140 |
| F15 | Nice | Grey badge 3.04:1 on white (light), 4.12:1 (dark); other badges 4.2 to 7.5:1. Text pairs: 5.11 to 16.3:1 in both themes. | Contrast passes AA everywhere; the grey badge has no margin. | X1 | #141 |
| F16 | Should | Forced colours: badge fill equals the background (1:1). `audit/list-720-light-forced-fold.png` | Badges vanish in forced colours; the status word keeps the meaning, so no information is lost. | X1 | #141 |
| — | — | `tokens.css`: neutral greys, GitHub-like status hues. | No colour belongs to dock-n-roll (pain point 8). | goal 6 | #140 |

### States

| # | Severity | Evidence | Finding | Need | Step |
|---|---|---|---|---|---|
| F35 | Must | 48 buttons, 19 disabled at rest, 21 while an action is pending, 44 with Docker down. Disabled buttons are plain muted text (6.11:1), `cursor: not-allowed`. | Controls that do not apply are shown disabled, against the leading principle; they read as labels. | goal 4 (S2) | #139 |
| F24 | Must | Docker stopped while open: banner "Docker unavailable: connect ENOENT /…/d.sock"; header summary gone; Running tab keeps its last tables. Docker down at start: Running tab says "No container." under the banner; port owners become "unknown (root process)". `audit/docker-down-720-light-fold.png`, `audit/down-at-start-running-720-light-fold.png` | The banner shows a raw socket error; the header loses its status (#88 point 1 confirmed); the Running tab shows stale or false content (#91 confirmed). | D3 | #138, #142 |
| F25 | Must | Empty stream: a blank grey bar 16 px high, no text. `audit/logs-empty-720-light-fold.png` | An empty log stream is not announced (#82 confirmed). | D2 | #142 |
| F26 | Should | No loading output seen in any scenario: the first paint already has rows or the banner. | The loading state exists in code and is never visible; it is not designed. | goal 5 (S4) | #142 |
| F27 | Should | Failed start: reason on the row after 1.1 s, red, row 65 → 158 px, text "Start failed: Container blog-web-1 Starting Error response from daemon: …". `audit/states-720-light.png` | The reason is on the row (D1 met) but raw: Compose progress lines come before the cause. | D1 | #142 |
| F28 | Should | Pin-file errors: 4 banners, chrome 187 → 296 px (394 px at 320). Text "services[0].colour: unknown key". `audit/pin-errors-720-light-fold.png` | One banner per error pushes the list down; the wording is the parser's, with no file name or line. | goal 5 | #138, #142 |
| F29 | Nice | Empty list: "No Compose project found." in muted text, no next step. `audit/empty-720-light-fold.png` | The empty state does not say what to do. | goal 5 | #142 |
| F30 | Nice | Pending: status word "starting"/"stopping", orange badge, every button but Logs disabled. | Progress is a word change only. | V2 | #140 |

### Motion

| # | Severity | Evidence | Finding | Need | Step |
|---|---|---|---|---|---|
| F31 | Should | 0 `transition`, 0 `animation` in the computed styles. Row height jumps 65 → 158 px on a failure; the logs panel appears at once. | State changes are instant and silent: a badge that turns from green to red elsewhere on screen is easy to miss. Reduced motion is met by absence. | V1, V2 | #140 (skill `ui-motion`) |

### Accessibility

| # | Severity | Evidence | Finding | Need | Step |
|---|---|---|---|---|---|
| F17 | Must | `Ctrl+=`, `Ctrl+Shift+=`, `Ctrl+NumpadAdd`: device pixel ratio stays 1, inner width stays 720. | Zoom is not reachable (#89 confirmed): WCAG 1.4.4 fails. | X1 | #142 (code brick) |
| F18 | Should | Buttons 56 to 76 × 30 px; tabs 56 × 38 px. | Targets pass the 24 px minimum (2.5.8) and stay under the 44 px comfort size. | X1 | #141 |
| F19 | Should | Group `summary`: 657 × 20 px. Logs container `select`: 92 × 19 px. | Two targets under 24 px high; they pass 2.5.8 only by the spacing exception. | X1 | #141 |
| F20 | Should | `summary` focus: browser default `outline: auto 1px rgb(229, 151, 0)`, 2.4:1 on white. Buttons: `2px solid` foreground, offset 2 px. `audit/focus-720-light.png` | The group toggle has a weaker focus ring than the buttons and fails 3:1 in the light theme. | X1 | #141 |
| F21 | Should | Two rows named "notes" (name conflict): both buttons are "Logs of notes". | Duplicate accessible names: a screen reader cannot tell the two rows apart. | X1 | #142 |
| F22 | Should | `main`: 32 tab stops (1 tab list, 2 group toggles, 29 enabled buttons) to reach the last row; no skip, no arrow-key movement in the list. | Keyboard reach is complete but linear. | X1 | #138 |
| F23 | Nice | `title` tooltips only on the folder and on shortened reasons. | Hover-only text, not reachable by keyboard. | X1 | #142 |
| — | — | `lang="en"`, one `h1`, `main`, tab list, tab panel, `role="alert"` banners, polite status region ("Blog: Start failed: …" announced). Status never by colour alone. | Kept as it is: the structure is sound. | X1 | — |

### Content resilience

| # | Severity | Evidence | Finding | Need | Step |
|---|---|---|---|---|---|
| F32 | Must | Name of 41 characters: 2 lines at 720 px (row 103 px), 7 at 500, 11 at 480, 27 at 460, 38 at 440 (row 899 px). | No truncation rule: the longest name sets the row height (with F9). Real names today: 5 to 24 characters. | V2, X1 | #139, #141 |
| F33 | Should | Logs: 500 lines kept, long lines wrap, monospace 13 px, panel 316 px. Opens on the newest line and stays there on new output. `audit/logs-chatty-720-light-fold.png` | #64 (logs open in the middle) is not reproduced on this build. Caveat: the fixture sent the backlog in one write; a slow backlog was not tested. | D2 | #142 (re-test) |
| F34 | Should | Running tab, 720 px: a 41-character container name takes 3 lines; "running" and "exited" break after 6 letters. `audit/running-720-light-fold.png` | Column widths ignore their content. | V4 | #139 |

## Issues folded in

| Issue | Result | Finding |
|---|---|---|
| #127 name squeezed between 460 and 540 px | Confirmed, wider than reported: 440 to 600 px | F9, F32 |
| #132 Running tab overflows at 320 px | Confirmed: 22 px | F12 |
| #88 point 1, header status with Docker down | Confirmed | F24 |
| #88 point 2, Groups popover | Obsolete: the popover was removed by #98 | — |
| #89 text zoom | Confirmed | F17, F6 |
| #91 Running tab with Docker down | Confirmed | F24 |
| #82 empty log stream | Confirmed | F25 |
| #64 logs open in the middle | Not reproduced, see caveat | F33 |
| #61 actions menu, #41 stop all, #68 logs window | No place in the current interface: needs A5, A3 and Q4 | S3 baseline |

## Recorded decisions in conflict with a finding

The audit proposes; the owner decides at the step named. Nothing is reopened silently.

| Decision | Finding | Proposal |
|---|---|---|
| Status colour code: red for off (spec § 4; already open to change in the brief) | F14 | Reserve the alarm colour for a fault; a stopped service gets a neutral treatment. Step #140. |
| Logs in a dashboard panel (ADR-0008) | F11 | Evidence for Q4: the panel leaves 204 px of list in the default window and opens below the fold for low rows. Step #138. |
| With Docker unreachable: banner, rows greyed without a badge (spec § 4) | F24 | Keep; extend to the header and the Running tab, and word the banner for the user. Step #138. |
| One banner per pin-file error (spec § 2) | F28 | Group them in one collapsible notice. Step #138. |

## Volume measured on the workstation, 2026-11-22

Figures only. The brief was measured on 2026-11-20, before the clean-up of 2026-11-21.

| Measure | 2026-11-20 (brief) | 2026-11-22 |
|---|---|---|
| Compose projects | 12 (2 running) | 11 (4 running) |
| Pin-file entries | 7 | 10 (4 pinned, 4 hidden; 3 with a URL, 1 with a health URL) |
| Groups | 4 | 4, of 2, 1, 1 and 1 rows |
| Rows listed in the dashboard | — | 8 (3 of them outside a group) |
| Containers outside Compose | 24 (11 running) | 19 (18 running) |
| Listening TCP ports | — | 27 |
| Name length | — | 5 to 24 characters |

## Open questions settled here

### Q1: reference widths and default window size

Evidence:

- Height limits what is seen, not width: 720 × 520 shows 3 rows of 8, 960 × 640 shows 4, 720 × 900 shows 7.
- At 720 px every row of the workstation fits on one line (names up to 24 characters); wider adds empty space between name and actions.
- 320 px is the reflow floor of the brief; 360 × 260 is what 200 % zoom gives in the default window.
- Row budget with a 90 px header and 24 px of bottom padding, 8 rows in 4 groups with 32 px group heads: 34 px per row at 520 px high, 49 px at 640, 59 px at 720.

Owner MCDA 2026-11-22: **B, 720 × 640 px, 430/500**, against 390 (A, 720 × 520 kept) and 375 (C, 960 × 640).

| Criterion (weight) | A: 720 × 520 | B: 720 × 640 | C: 960 × 640 |
|---|---|---|---|
| S1 fits without scrolling (30) | 2 | 4 | 4 |
| Footprint of a quick look (20) | 5 | 4 | 2 |
| Reflow coverage, X1 (20) | 4 | 4 | 4 |
| Study cost (15) | 5 | 5 | 5 |
| Small screens (15) | 5 | 5 | 4 |
| **Total / 500** | **390** | **430** | **375** |

- The mockups target a default window of 720 × 640 px; the reference widths are 320 px (reflow floor), 720 px (default) and 960 px (wide check); 360 × 320 px stands for 200 % zoom.
- The window size itself changes in `src/main/index.ts` with the first code brick, not in the study.
- Sensitivity: with the footprint at 35 and S1 at 15, A scores 405 and B 430; B stays first.

### Q2: services visible without scrolling (S1)

Evidence:

- 8 rows are listed today; 11 Compose projects exist (12 two days earlier); 4 are hidden by the pin file.
- Today's layout needs 949 px for the 8 rows and 1232 px for 12.
- Row budget at 720 × 640, same assumptions as Q1: 49 px per row for 8 rows, 39 px for 10, 33 px for 12. At 720 × 520: 34, 27 and 23 px; 12 rows there fall under the 24 px target minimum.

Owner MCDA 2026-11-22: **A, 8 services, 420/500**, against 380 (B, 10) and 295 (C, 12).

| Criterion (weight) | A: 8 | B: 10 | C: 12 |
|---|---|---|---|
| Realism, measured use (35) | 5 | 4 | 3 |
| Row comfort at the default size (25) | 5 | 4 | 2 |
| Room for growth (20) | 2 | 4 | 5 |
| Design freedom: groups, reasons (20) | 4 | 3 | 2 |
| **Total / 500** | **420** | **380** | **295** |

- S1 is measured with 8 services in 4 groups, at 720 × 640 px: the global state and every service in fault are visible without scrolling.
- Sensitivity: with room for growth at 40 (realism 25, comfort 15), B scores 380 and A 360. The count is re-measured if the listed rows pass 8.

## Checks

| Run | Not run |
|---|---|
| Overflow, target sizes, fonts and contrast at 320, 360 × 260, 440 to 680, 720, 960 px, light and dark | A screen reader pass (structure and names read from the accessibility tree only) |
| Forced colours at 720 px | The owner's own GNOME session: font (Ubuntu Sans there, Noto Sans here), display scale |
| Keyboard: tab order, focus styles, zoom shortcuts | The tray menu (out of the study) |
| States: every status, pending, failed action, pin errors, empty, Docker down at start and while open, logs (chatty, empty, ended, no container) | Logs with a slow backlog (#64) |
| Reduced motion: nothing to reduce | Real Docker in the screenshots (figures only, by rule) |

## Follow-up

- #137 benchmark: compare on F1 to F4 (state first), F7 (density), F13 (containers and ports linked to services), F14 (colour of "stopped").
- #138 information architecture: F3, F11, F13, F22, F24, F28 and Q3, Q4, Q5.
- #139 wireframes: F2, F4, F7 to F10, F12, F32, F34, F35.
- #140 visual direction: F1, F5, F14, F30, F31.
- #141 design system: F6, F15, F16, F18 to F20.
- #142 mockups and specification: F17, F21, F23, F25 to F27, F29, F33.
