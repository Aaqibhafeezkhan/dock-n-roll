# Dashboard redesign brief

The users, use scene, goals, ranked needs, constraints and success criteria of the dashboard redesign (#135), step 1 of the design study (epic #134), from the owner interview of 2026-11-20.

The study is done in Penpot before any redesign code. It starts from the capture of the current dashboard ([dashboard-capture.md](dashboard-capture.md), #128).

## Users and use scene

| Point | Answer |
|---|---|
| User | One developer, the owner, on a single Ubuntu workstation (GNOME). No other user is targeted. |
| Scene | A quick look: the dashboard is opened from the tray a few times a day, for one check or one action, then closed. It is not kept open. |
| Device | Desktop screen, mouse and keyboard. No touch, no phone. |
| Window | Opens at 720 × 520 px, resizable, no minimum size set (`src/main/index.ts`). |
| Light | Office, light or dark theme following the system. |
| Volume, measured 2026-11-20 | 12 Compose projects (2 running), 7 pin-file entries, 4 groups, 1 hidden service; 24 containers outside Compose (11 running). |

Surface mode (skill `ui-design` § 2): **Operate**. The user succeeds when the task is done; scanability, consistency and density come first.

## Leading principle: a minimal, dynamic interface

Owner rule (2026-10-22, restated 2026-11-20). It ranks above every other design choice of the study.

- The interface is minimal: at rest it shows the state and almost nothing else.
- Buttons and fields are displayed dynamically: what appears depends on the previous click and on the current state.
- A control that does not apply is absent, not disabled.
- A follow-up field or choice appears only after the choice that triggers it.
- Each wireframe and mockup states, per screen, what is visible at rest and what each click reveals.

## Goals

1. The state of the services is read at a glance: what runs, what is in fault.
2. When the state is not the wanted one, the action that fixes it is next to it: start, stop, restart.
3. A failure is understood without leaving the app: reason on the row, readable logs.
4. A minimal interface: controls appear dynamically, from the previous click (the leading principle above).
5. Every state of every screen is designed, none left blank.
6. The dashboard gets its own visual identity, recorded as tokens.

## Non-goals

- The redesign code: separate bricks after the study (epic #134).
- Generating `tokens.css` from the Penpot tokens (#129).
- The tray menu: a native GNOME menu, out of the study (owner MCDA 2026-11-20).
- New service types (shell commands, `systemd --user` units), other operating systems, other users.
- A marketing page or any Persuade surface.

## Scope

Owner MCDA (multi-criteria decision analysis) 2026-11-20: option B, 435/500, against 400 (B plus the tray menu) and 355 (current screens only).

- The current screens: service list with groups, Running tab, logs, banners.
- The planned functions, designed with them: stop all (#41), service actions menu (#61), and the request for logs in their own window (#68, open question Q4).

## Ambition

Owner MCDA 2026-11-20: **replacement world**, 425/500, against 330 (new composition, current tokens kept) and 250 (polish).

A full direction is made (colour, type, layout, components) and recorded as tokens. The current look is evidence, not a base to polish (skill `ui-design` § 3). The owner is open to every change: no part of the current interface is frozen.

## Tasks, ranked

1. **See the state** of the services and groups.
2. **Act** when the state is not the wanted one: start, stop, restart a service or a group.
3. **Diagnose** a failure: reason, logs, ports.
4. **Inspect** what else runs on the machine: containers outside Compose, listening ports.
5. **Organise**: pin, hide, group, search.

## Needs

Must: without it the redesign fails. Should: designed in the study, may be coded after the core. Nice: a place is kept, no detailed design.

| ID | Need | Class | Issues |
|---|---|---|---|
| V1 | Global state at a glance: what runs, what is in fault | Must | |
| V2 | Status of each service, with its reason | Must | |
| V3 | Summary of a collapsed group | Should | |
| V4 | Containers outside Compose and listening ports, linked to the services | Should | #132 |
| V5 | CPU and memory use | Nice | #61 (backlog) |
| A1 | Start, stop, restart a service | Must | |
| A2 | Start, stop a group | Must | |
| A3 | Stop everything with one action | Should | #41 |
| A4 | Open the service's URL | Should | |
| A5 | Service actions menu: open the folder, terminal in the container, start with the PC | Should | #61 |
| D1 | Reason of a failed action on the row | Must | |
| D2 | Readable logs: they open on the newest line, an empty stream is announced | Must | #64, #82 |
| D3 | Docker unavailable said clearly on every screen | Must | #88, #91 |
| O1 | Search or filter | Nice | |
| O2 | Pin, hide, rename, group from the interface instead of the YAML file | Nice | |
| Q1 | Keyboard, 200 % zoom, narrow window, light and dark themes, WCAG (Web Content Accessibility Guidelines) 2.2 level AA | Must | #89, #127, #132 |

## Pain points of the current dashboard

Evidence: the reference screenshots under `reference/` and the open issues. The audit (#136) measures each one.

| # | Pain point | Evidence |
|---|---|---|
| 1 | The state is the least visible thing: a 10 px badge, the status in muted text, the summary ("1 of 5 on") in the top right corner. | `reference/service-list-960-light.png` |
| 2 | Crowded rows: four or five buttons each, zero to three of them disabled. Five services show 21 controls. | same |
| 3 | Inverted hierarchy: the black Start button draws the eye before the status. | same |
| 4 | At 320 px a row takes three levels; five services need 981 px, so the list scrolls. Between 460 and 540 px the name is squeezed. | `reference/service-list-320-light.png`, #127 |
| 5 | Groups: "Start all" and "Stop all" always visible, on a line of their own under the group title. | `reference/service-list-960-light.png` |
| 6 | Running tab: two raw tables, no sort, no filter, no link to the services; it overflows at 320 px. | `reference/running-tab-960-light.png`, #132 |
| 7 | States not designed: Docker unavailable, empty log stream, logs opened in the middle of the buffer; hover, focus, empty and loading are not captured. | #88, #91, #82, #64 |
| 8 | Generic look: system font, neutral greys, nothing that belongs to dock-n-roll. | `src/renderer/src/styles/tokens.css` |
| 9 | The text cannot be zoomed and the sizes are in `px`. | #89 |

## Constraints

- **Platform**: Electron renderer (React, ADR-0001, ADR-0004), Chromium on Linux only. Fonts must be bundled or present on Ubuntu; `system-ui` is Ubuntu Sans in the owner's GNOME session and Noto Sans in a clean session (the references).
- **Window**: no horizontal scroll and no clipped content from 320 to 960 px wide, and at 200 % zoom (WCAG 2.2 success criteria 1.4.4 Resize text and 1.4.10 Reflow).
- **Themes**: light and dark, following `prefers-color-scheme`.
- **Accessibility**: WCAG 2.2 AA; a status never relies on colour alone; every action reachable by keyboard.
- **Minimal and dynamic**: the leading principle above applies to every screen, the logs window and the dialogs included.
- **Tokens**: one source. `tokens.css` is the master until #129; the new direction is recorded as Penpot tokens (#141).
- **Branding**: no dock-n-roll brand rules exist; the study creates them (#140).
- **Design tool**: Penpot, self-hosted; exports, tokens JSON and `.penpot` file versioned under `docs/architecture/design/` at each step.
- **Security**: local-only app (spec § 6 R4); the renderer is sandboxed and reaches Docker only through the main process (`diagrams/launcher/03-component.md`, ADR-0006). A design must not need a remote resource.

## Recorded decisions that stay

They describe behaviour, not appearance. The study may propose to reopen one, as an explicit question to the owner, never silently.

| Decision | Source |
|---|---|
| The status is derived, with eight values and their precedence; which actions each status allows | spec § 4 |
| Stop is `compose stop`, never `down` | spec UC2 |
| With Docker unreachable: an app-level banner, rows greyed without a status badge | spec § 4 |
| Logs: text only, read-only, last 500 lines then follow | spec UC7, ADR-0008 |
| Logs open in a dashboard panel, not in a window (ADR-0008, 435 against 315). #68 asks to reopen it: open question Q4; the decision stays until the owner rules on it. | ADR-0008, #68 |
| The pin file is edited by hand. Need O2 (Nice, owner 2026-11-20) keeps a place for editing in the interface; the decision stays until the owner rules on it. | spec § 2 |
| Every action is reachable from the tray menu, except the logs (dashboard only, UC7). The tray entries of new actions (A3, A5) are added by the code bricks: the native menu is not designed here. | spec § 6 R5, UC7 |
| A failed action shows its reason on its row and is announced | #85 |

Superseded by the leading principle (owner, 2026-11-20): the disabled buttons of spec UC1 extensions \*a and \*b and of the UC8 extension (Docker unavailable). A control that does not apply is absent; the reason it gave in a tooltip must stay readable elsewhere. The spec is updated with the specification of step 8 (#142).

The status colour code (green on, red off, orange transitional, grey unavailable; spec § 4) is open to change like the rest of the look, as long as the four families stay distinct in both themes.

## Success criteria

| # | Criterion | Measure |
|---|---|---|
| S1 | State read in 2 seconds | At the default window size, the global state and every service in fault are visible without scrolling, for the services measured on the workstation (the row count is fixed at the audit, Q2). |
| S2 | Minimal at rest | At rest, at most 2 visible controls per service row (4 to 5 today) and no disabled control on screen. |
| S3 | Actions stay close | Every action of the Must and Should needs is at most 2 clicks away from the open dashboard. |
| S4 | Every state designed | Each screen has its empty, loading, error, Docker-unavailable, hover and focus states in the mockups. |
| S5 | Accessible | No WCAG 2.2 AA defect in the mockups review; contrast checked on every text and status pair, in both themes. |
| S6 | Reflow | No overflow from 320 to 960 px and at 200 % zoom, on every screen. |
| S7 | Every Must covered | Each Must need maps to at least one mockup and one flow. |
| S8 | Reviewed | `ui-review` score above a threshold. The skill scores a built interface: how it applies to Penpot mockups (on their exports, or on the first coded brick) is set at step 8 (#142) with the threshold. |
| S9 | Validated | The owner validates each step before the next. |

## Study steps

One issue and one chat per step; each ends with a versioned deliverable and the owner's validation.

| Step | Issue | Deliverable |
|---|---|---|
| 1 | #135 | This brief |
| 2 | #136 | Audit of the current dashboard |
| 3 | #137 | Benchmark of comparable container dashboards |
| 4 | #138 | Information architecture and user flows |
| 5 | #139 | Low-fidelity wireframes in Penpot |
| 6 | #140 | Visual direction |
| 7 | #141 | Design system in Penpot |
| 8 | #142 | High-fidelity mockups and specification |

## Open questions

| # | Question | Settled at |
|---|---|---|
| Q1 | Reference widths: the capture used 960 and 320 px, the window opens at 720 × 520 px. Which sizes do the mockups target, and does the default size change? | #136 |
| Q2 | How many services must be visible without scrolling (S1)? | #136 |
| Q3 | Does the Running tab stay a tab, or do containers and ports join the service list? | #138 |
| Q4 | Logs: own window, inline panel, or both (#68)? | #138 |
| Q5 | Where do "stop all" (#41) and the actions menu (#61) live? | #138, #139 |
| Q6 | Font: bundled family or system font? | #140 |
| Q7 | Threshold of the `ui-review` score and how it is measured on mockups (S8) | #142 |
