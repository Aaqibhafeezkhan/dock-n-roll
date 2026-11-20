# Local review — dock-n-roll (fix/84-actions-overflow-narrow-width)

Pre-push review of the reflow fix for issue #84, three adversarial rounds, push gate open.

- Date: 2026-11-20
- Commit: `ea1f917`
- Scope: branch diff `origin/main...HEAD`
- Checks run: adversarial (fresh reviewer, 3 rounds) · design · docs · tests · security · correctness
- Verdict: ship as-is · Must 0 · Should 0 · Nice 0 open (Must 1, Should 7, Nice 3 raised and fixed)

## Adversarial review (#236)

- Round 1 on `7e4b6dc`: Must 1 · Should 2 · Nice 2 · unverified 0 · Push gate: closed
- Round 2 on `a963519`: 5 fixed · 0 not fixed · 0 regressed · new Must 0 · new Should 1 · new Nice 1 · Push gate: open
- Round 3 on `ea1f917`: 2 fixed · 0 not fixed · 0 regressed · new Must 0 · **Push gate: open**
- Optional second review (owner's own account): not run (never a gate step)

## Pre-merge gate (head SHA `f992fbe`)

| Step | Result |
|---|---|
| 1 Tests | ✅ 660 unit/integration (38 files) · `test-local-ci` 64 · `test-attest` 22 |
| 2 Coverage | tests 97.33 % stmts / 92.81 % branches / 97.95 % funcs / 98.6 % lines (floor 80) · docs ⚠️ not run by `just gate`: 72.1 % by hand (106 of 147 exports, skill counter), under the 90 % floor, debt older than this branch, which changes no export (#131) |
| 3 SonarQube | ✅ `QUALITY GATE STATUS: PASSED` |
| 4 Secrets | ✅ gitleaks clean · no tracked `.env` · diff read: nothing sensitive; QA screenshots cropped to the Design group |
| 5 E2E/smoke | ✅ smoke 15/15 on the packaged build, the four #84 checks included · QA pass: [2026-11-20-84-reflow](../qa/2026-11-20-84-reflow.md) |

- Gate: **attested, one step not run** — `mise x -- just gate` exit 0 on `f992fbe`, `attest: gate verified on f992fbeef60d763de77d8ed6ccc4ff30234d157f`. Doc coverage is not held by the gate and is under its floor: the owner allowed the merge on 2026-11-20 with that step recorded as not run, tracked in #131. Not claimed green.
- A first run with `SONAR_TOKEN` exported failed `scripts/test-local-ci.sh:155` ("sonar: no token"), which inherits the variable; `just sonar` reads the token file itself, so the gate runs without the prefix.
- This report-only commit is gated again before the merge; it differs from `f992fbe` by this file alone.
- CI mirror: no `pull_request` workflow (ADR-0011), n/a.

## 🔴 Must Have

- `scripts/test-attest.sh:105` `[adversarial]` · `just check` red: "unsigned tag refused" fails under a global `tag.gpgSign=true` · fixed in `23ae2b6` (`git config tag.gpgSign false` in the test repository; owner choice A, MCDA 435/500).

## 🟡 Should Have

- `e2e/smoke.mjs:113` `[adversarial|tests]` · the smoke did not check that the five actions render and stay visible · fixed in `a963519`.
- `e2e/smoke.mjs:118` `[adversarial]` · right edge only: a row that stops wrapping slips off the left edge · fixed in `ea1f917` (both edges).
- `e2e/smoke.mjs:120` `[tests]` · failure message without the overflow size · fixed in `a963519`.
- `e2e/smoke.mjs:111` `[tests]` · comment did not say why each button is checked · fixed in `a963519`.
- `e2e/smoke.mjs:75` `[docs]` · no reason given for the pin's `url` · fixed in `a963519`.
- `e2e/smoke.mjs` `[design]` · dead viewport reset after the loop · removed in `a963519`.
- `docs/architecture/decisions/0010-e2e-smoke-over-cdp.md:32` `[adversarial]` · ADR did not list the reflow check · fixed in `a963519`.

## 🔵 Nice to Have

- `e2e/smoke.mjs:19` `[adversarial]` · 360 px case used height 568 · fixed in `a963519` (360 × 640).
- `PROJECT_LOG.md` `[adversarial]` · no `Next:` line · fixed in `a963519`.
- `docs/architecture/decisions/0010-e2e-smoke-over-cdp.md:42` `[adversarial]` · consequence bullet contradicted itself · fixed in `ea1f917`.

## ⚪ Disagree / Intentional

- `src/renderer/src/styles/global.css:260` `[design]` · `flex: 0 1 auto` restates the default · kept: it replaces `flex: 0 0 auto` and states the intent to shrink.
- `e2e/smoke.mjs:17` `[tests]` · two widths for one rule · kept: the issue names 320 and 360 px.
- Out of scope, pre-existing: service name squeezed between 460 and 540 px, filed as #127.

## Per-dimension summary

- Adversarial: gate open after three rounds, fix proven red without `flex-wrap` · Design: one dead line removed, CSS change minimal · Docs: ADR-0010, CHANGELOG, PROJECT_LOG and QA note agree · Tests: layout checked on the packaged build, both edges, five buttons · Security: no new input, target or secret · Correctness: no finding.
