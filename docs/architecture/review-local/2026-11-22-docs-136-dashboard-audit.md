# Local review — dock-n-roll (docs/136-dashboard-audit)

Pre-push review of the dashboard audit for issue #136, three adversarial rounds.

- Date: 2026-11-22
- Commit: `7431a27`
- Scope: branch diff `origin/main...HEAD` (docs only: `docs/architecture/design/2026-11-22-dashboard-audit.md`, 17 screenshots under `docs/architecture/design/audit/`, `docs/architecture/design/brief.md`, `PROJECT_LOG.md`)
- Checks run: adversarial (fresh reviewer, 3 rounds) · docs · security (secrets) · correctness; design, tests and containers not applicable (no source, test or container file changed)
- Verdict: ship as-is · Must 0 · Should 0 open · Nice 2 open, filed as #145 (Must 2, Should 13, Nice 11 raised)

## Adversarial review (#236)

- Round 1 on `d7a7740`: Must 2 · Should 9 · Nice 7 · unverified 3 · Push gate: closed
- Round 2 on `db0659d`: 17 fixed · 0 regressed · 2 Nice not applied (hover artefacts on three screenshots, volume reconciliation) · new Must 0 · new Should 1 · new Nice 1 · Push gate: open
- Round 3 on `7431a27`: 2 fixed · 0 not fixed · 0 regressed · new Must 0 · new Should 2 (figures of this report, fixed) · **Push gate: open**
- Optional second review (owner's own account): not run (never a gate step)

## Pre-merge gate

`--push` run: `mise x -- just check` exit 0 on `d7a7740` (docs-only commits since).

| Step | Result |
|---|---|
| 1 Tests | ✅ 660 unit/integration in 38 files; wiring tests 64 and 22 cases |
| 2 Coverage | tests 97.33 % stmts / 92.81 % branches / 97.95 % funcs / 98.6 % lines (floor 80) · docs ⏸ deferred to the pre-merge run (not held by `just gate`, #131) |
| 3 SonarQube | ⏸ deferred to the pre-merge run |
| 4 Secrets | ✅ gitleaks clean (212 commits) · no tracked `.env` · diff read: nothing sensitive (demo stacks only in the screenshots, workstation volumes as counts, PNG files without metadata) |
| 5 E2E/smoke | ⏸ deferred to the pre-merge run |

- Gate: **pre-push green** — `just check` passed; SonarQube, doc coverage and smoke deferred.
- CI mirror: no `pull_request` workflow (ADR-0011), n/a.

## 🔴 Must Have

- `docs/architecture/design/2026-11-22-dashboard-audit.md` § Q1 `[adversarial]` · the sensitivity check was wrong (A 405): with the footprint at 35 and S1 at 15, A scores 435 and B 430, so A leads · fixed in `db0659d`, tipping point corrected after round 2; the owner confirms B before the PR.
- `docs/architecture/design/2026-11-22-dashboard-audit.md` § Follow-up `[adversarial]` · F1, F24, F28 and F32 were missing from the list of a step their Step cell names · fixed in `db0659d`.

## 🟡 Should Have

All fixed in `db0659d` unless noted.

- F22 `[adversarial]` · 32 tab stops counted the tabs twice · 30.
- F32, F34 `[adversarial]` · the long name is 43 characters, its container 52; the break is after 5 letters.
- § Recorded decisions `[adversarial]` · "one banner per pin-file error" cited spec § 2 · current behaviour, spec § 9 Q1.
- `brief.md:16` `[adversarial]` · S1 quoted a volume the brief did not hold · the 2026-11-22 measure added.
- S5 baseline `[adversarial]` · "Contrast passes" against F16 and F20 · text contrast only.
- F20 `[adversarial]` · no rendered evidence of the focused group toggle · marked computed-style only.
- F6 `[adversarial]` · "cannot be enlarged at all" · from the app; GNOME scaling not tested.
- § Scores `[adversarial]` · eight axes out of 40 against the nine of `ui-review` · stated, compared axis by axis later.
- `PROJECT_LOG.md` `[adversarial]` · the method MCDA had no competing scores · 445 vs 430 vs 285, also in § Method.
- F15 `[docs]` · `audit/states-720-dark.png` not referenced · cited.
- § Colour `[docs]` · "pain point 8" without pointer · brief § Pain points, 8.
- § Q1 `[docs]` · "every row fits on one line at 720 px" read against F9 · the squeeze starts under 600 px.
- § Q1 `[adversarial]` round 2 · the tipping point of the sensitivity was misstated · B leads while the footprint weighs less than twice S1; fixed in the commit after `db0659d`.

## 🔵 Nice to Have

- Fixed in `db0659d`: screenshot naming rule, F28 base, F15 badge range, F18 severity (Nice: no touch device), `brief.md:100` range, acronyms (WCAG, MCDA) defined; the name of the ninth axis fixed after round 2.
- Open, filed as #145: hover artefacts on `list-720-light-fold.png`, `list-500-light.png` and `squeeze-480-light-fold.png`; the volume figures (11 projects, 4 hidden, 8 rows, 10 pin entries) are not reconciled in the text.

## ⚪ Disagree / Intentional

- Finding IDs out of page order `[docs|adversarial]` · they follow the order of discovery, stated in § Findings; renumbering would break the references already made.
- Report under `docs/architecture/design/`, findings sent to study steps instead of backlog issues `[adversarial]` · the brief versions each step's deliverable there and each later step owns its design.

## Per-dimension summary

- Adversarial: two Must on a wrong sensitivity check and incomplete follow-up lists, fixed · Docs: links and figures hold, one unreferenced image fixed · Tests: n/a · Security: no secret, no real project in text or screenshots · Correctness: figures recomputed against source, tokens and screenshots.
