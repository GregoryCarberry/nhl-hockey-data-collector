# Changelog

## v1.1.0 — release candidate (6 November 2026)

- Added external JSON configuration for source URL, maximum page attempts (1–30), minimum pre-navigation delay (2,000–60,000 ms), output directory and three CSV filenames.
- Added basic startup validation for numerical ranges, blank/whitespace-only output directory and CSV filenames, with interactive error messages and bot termination.
- Added filename/directory trimming and removal of trailing directory separators before constructing the three CSV paths.
- Reordered CSV path assignments to execute after normalisation; preserved the working-page CSV re-import and merge workaround.
- Added a configurable Delay immediately before the Next navigation click; actual HTTP request frequency and site-rule compliance have not been measured.
- Local full regression on 6 November: 582 records, one nine-column header, no blank/identical duplicate rows, first two page baselines matched, last-page diagnostic matched last seven records.
- Earlier 532-row apparently complete run highlighted a possible stale-browser starting-state risk; underlying cause unproven and deferred to a future version.
- Sanitised distribution ZIP changes only the machine-specific defaults of `fConfigFile` and unused `sConfigPath`. Independent import/run of that distribution ZIP is **not verified**.
- Still interactive and not production-grade unattended automation.

## v1.0 — 30 October 2026

- Implemented content-based duplicate-page identification and UI-driven pagination.
- Added CSV export/import normalisation to work around missing Recorder table-column metadata during merging.
- Corrected signature-list insertion and unique-page counting to occur only for newly accepted pages.
- Verified full extraction: 25 attempts, 24 unique pages, 582 records, zero identical duplicate rows and zero blank records (before final comment-only changes).
- Removed obsolete diagnostics and experimental variables; added section comments and maintained action annotations.
- Retained simple interactive completion message and hardcoded local paths as known v1.0 limitations.
