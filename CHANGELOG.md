# Changelog

## v1.2.0 — release candidate (17 November 2026)

- Added five scoped Try/Catch handlers for JSON configuration loading, numeric conversion, browser startup, scraping/pagination, and final CSV writes. Captured exceptions are reported with action line numbers before stopping.
- Added browser-start reset: navigate an existing matching Chrome tab to the configured source URL; on the recognised missing-tab error open a new Chrome window, then verify the actual starting URL via JavaScript.
- Added explicit **COMPLETED**, **PARTIAL RUN**, and **FAILED** messages. The PARTIAL branch warns if more pages remain at the configured attempt cap, including test case 3 attempts / 2 unique pages / 50 records.
- Moved completion reporting after the two final CSV writes and added failure reporting for a write error that could leave one CSV updated and the other stale.
- Added an outer Try/Catch around the main pagination loop, including intermediate CSV processing.
- Verified controlled negative tests and subsequent 582-row positive regressions; see [v1.2.0 validation](docs/v1.2.0-validation.md).
- Sanitised distribution changes only generic configuration-path variable defaults. A separately imported v1.2.0 ZIP was successfully run after setting the external configuration path, with independent full CSV readback on 17 November 2026.
- Remaining limitations include interactive operation, no structured persistent logs or automatic retries, message-text-based browser fallback, and no measured HTTP-level scraping compliance.

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
