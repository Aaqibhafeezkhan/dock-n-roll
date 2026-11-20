# Local review — dock-n-roll (docs/128-penpot-dashboard-capture)

Pre-push review of the Penpot capture of the current dashboard for issue #128, three adversarial rounds, push gate open.

- Date: 2026-11-20
- Commit: `e03f01b`
- Scope: branch diff `origin/main...HEAD` (docs, images, `.penpot` archive and tokens JSON; no source or test change)
- Checks run: adversarial (fresh reviewer, 3 rounds) · docs · security (gitleaks, personal-data scan of the archive) · `just check`
- Verdict: ship as-is · Must 0 · Should 0 · Nice 0 open (Must 3, Should 5, Nice 3 raised; fixed or deferred below)

## Adversarial review

- Round 1 on `1837818`: Must 2 · Should 3 · Nice 3 · unverified 1 · Push gate: closed
- Round 2 on `19e46ef`: 2 Must fixed · new Must 1 (regression) · new Should 2 · Push gate: closed
- Round 3 on `e03f01b`: 3 fixed · 0 not fixed · 0 regressed · new Must 0 · **Push gate: open**
- `mise x -- just check` exit 0 on each round (statements 97.33 %, gitleaks clean).

## 🔴 Must Have

- `design/reference/*.png` `[adversarial]` · the references were laid out at 960/320 px then cropped to 945/305 · fixed in `19e46ef` (re-shot at 945/305 with a page-tall window).
- `design/tokens.json` `[adversarial]` · `font.family.mono` named Noto Sans Mono; Chromium on Linux renders DejaVu Sans Mono · fixed in `19e46ef` (DejaVu uploaded to Penpot as a team font; owner choice A, MCDA 450/500).
- `design/reference/*.png` `[adversarial]` · the round-2 re-shoot ran in the desktop session (Ubuntu Sans, colour management) · fixed in `e03f01b` (clean environment, `--force-color-profile=srgb`).

## 🟡 Should Have

- `design/exports/components/*.svg` · fonts loaded from the local Penpot server · fixed in `19e46ef` (SVG files dropped, PNG files kept).
- `PROJECT_LOG.md` · "no export changes" contradicted the diff · fixed in `19e46ef`.
- `design/dashboard-capture.md` Method · geometry and screenshots from different layouts · fixed in `19e46ef` / `e03f01b`.
- `design/dashboard-capture.md` Known differences · stale height range · fixed in `e03f01b`.
- `design/dashboard-capture.md` Running tab at 320 px · app and frame tables 10 px apart · fixed in `e03f01b`; the overflow itself is an app defect, #132.

## 🔵 Nice to Have

- Container `<select>` arrow not listed · fixed in `19e46ef`.
- Stale thumbnails in the `.penpot` archive (about 3 MB, no private data) · deferred.
- PR body must carry `Closes #128` · on PR creation.
