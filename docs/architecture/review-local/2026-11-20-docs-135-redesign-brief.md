# Local review — dock-n-roll (docs/135-redesign-brief)

Pre-push review of the dashboard redesign brief for issue #135, three adversarial rounds, push gate open.

- Date: 2026-11-20
- Commit: `bb91f5a`
- Scope: branch diff `origin/main...HEAD` (docs only: `docs/architecture/design/brief.md`, `PROJECT_LOG.md`)
- Checks run: adversarial (fresh reviewer, 3 rounds) · docs · security (secrets) · correctness; design, tests and containers not applicable (no source, test or container file changed)
- Verdict: ship as-is · Must 0 · Should 0 · Nice 0 open (Must 2, Should 9, Nice 8 raised; all fixed except 2 Nice kept as intentional)

## Adversarial review (#236)

- Round 1 on `1604151`: Must 2 · Should 8 · Nice 5 · unverified 1 · Push gate: closed
- Round 2 on `e811ecb`: 14 fixed · 0 not fixed · 0 regressed · 1 Nice not applied (intentional) · new Must 0 · new Should 1 · new Nice 3 · Push gate: open
- Round 3 on `bb91f5a`: 4 fixed · 0 not fixed · 0 regressed · new Must 0 · **Push gate: open**
- Optional second review (owner's own account): not run (never a gate step)

## Pre-merge gate (head SHA: filled by the pre-merge run)

| Step | Result |
|---|---|
| 1 Tests | ✅ `mise x -- just check` exit 0 on `1604151`: 660 unit/integration · `test-local-ci` 64 · `test-attest` 22 |
| 2 Coverage | tests 97.33 % stmts / 92.81 % branches / 97.95 % funcs / 98.6 % lines (floor 80) · docs ⏸ deferred to the pre-merge run |
| 3 SonarQube | ⏸ deferred to the pre-merge run |
| 4 Secrets | ✅ gitleaks clean · no tracked `.env` · diff read: nothing sensitive (workstation volumes as counts only, no project name) |
| 5 E2E/smoke | ⏸ deferred to the pre-merge run |

- Gate: **pre-push green** — `just check` passed on `1604151`; the later commits change `brief.md` and `PROJECT_LOG.md` only. SonarQube, doc coverage and E2E/smoke deferred to `just gate` on the pushed head.
- CI mirror: no `pull_request` workflow (ADR-0011), n/a.

## 🔴 Must Have

- `docs/architecture/design/brief.md:52` `[adversarial]` · the logs window (#68) was stated as decided in Scope and need D2, open in Q4, against ADR-0008 (panel, 435 against 315) · fixed in `e811ecb`: a decision that stays, reopened by Q4.
- `docs/architecture/design/brief.md:130` `[adversarial]` · need O2 (pin editing in the interface) contradicted "the pin file is edited by hand" (spec § 2) · fixed in `e811ecb`, open question Q8 added in `bb91f5a`.

## 🟡 Should Have

All fixed in `e811ecb` unless noted.

- `brief.md:131` `[adversarial]` · tray out of scope against spec R5: tray entries of new actions left to the code bricks, logs dashboard-only (UC7).
- `brief.md:134` `[adversarial]` · the leading principle overrides the disabled buttons of spec UC1 \*a, \*b and UC8 without saying so: listed as superseded, spec updated at #142.
- `brief.md:110` `[adversarial]` · wrong WCAG criterion for 200 % zoom: 1.4.4 and 1.4.10.
- `brief.md:117` `[adversarial|security]` · wrong security sources: R4, `diagrams/launcher/03-component.md`, ADR-0006.
- `brief.md:85` `[adversarial]` · "newest line first" ambiguous: "they open on the newest line".
- `brief.md:98` `[adversarial]` · "one to three disabled": zero to three.
- `brief.md:109` `[adversarial]` · `system-ui` is Ubuntu Sans in the GNOME session, Noto Sans in a clean one.
- `brief.md:149` `[adversarial]` · `ui-review` scores a built interface: how it applies to mockups is set at #142 (S8, Q7).
- `PROJECT_LOG.md:669` `[adversarial]` · "with their open question" was false for O2 · fixed in `bb91f5a` (Q8).

## 🔵 Nice to Have

- `brief.md:101` · pain point 5 wording and evidence · fixed in `e811ecb`.
- `brief.md:43` · acronyms defined at first use (MCDA, WCAG) · fixed in `e811ecb` and `bb91f5a`.
- `brief.md:142` · S1 counted "12 projects" · fixed in `e811ecb` (row count set at the audit, Q2).
- `PROJECT_LOG.md` · #128 merge line missing · fixed in `e811ecb`.
- `brief.md:113` · "the logs window" assumed a window · fixed in `bb91f5a` ("the logs view").
- `brief.md:89` · a need and an open question both named Q1 · fixed in `bb91f5a` (need renamed X1).

## ⚪ Disagree / Intentional

- `docs/architecture/design/brief.md` · rename to `dashboard-redesign-brief.md` · issue #135 names the deliverable `brief.md`.
- `brief.md:88` · YAML not spelled out · a common file-format name, clear to the reader.

## Per-dimension summary

- Adversarial: two contradictions with recorded decisions found and fixed; gate open at round 3.
- Docs: one `#` title, summary line, sentence-case headings; `typos` exit 0.
- Security: gitleaks clean, no personal or project name in the measured volumes.
- Correctness: cross-references (Q1–Q8, S1–S9, spec and ADR citations) checked by the reviewer.
- Design, tests: not applicable (no code).

## Out of scope (pre-existing)

- #129 body calls #128 "step 1", which collides with the step numbering of epic #134.
- #88 point 2 mentions a Groups popover that no longer exists.
