# NHL Hockey Data Collector — Automation Anywhere 360

A personal portfolio project built in **Automation Anywhere 360 Community Edition**. The bot extracts NHL team-season statistics from the paginated [Scrape This Site hockey table](https://www.scrapethissite.com/pages/forms/), skips repeated pages by comparing their contents, and writes a consolidated CSV.

> **Latest development release: v1.1.0 (release candidate).** The new version adds external JSON settings, basic startup validation, configurable CSV paths and a 3-second pre-navigation delay. It is still interactive and **has not been independently import-tested** from the sanitised ZIP. The v1.0.0 package is retained unchanged.

## Verified results

**v1.0.0 baseline (30 October 2026):** In a clean full test, the bot attempted 25 pages, accepted 24 unique pages and exported **582 data records**, with one nine-column header, no empty rows and no identical duplicate rows. The final page contained seven records. The run stopped with `bHasNextPage = False`, before the 30-attempt safety cap. Earlier controlled tests compared the first two extracted pages against independently retained CSV baselines, with exact matches.

This is a tested result in the author's environment, **not** a claim that the bot is robust against every browser or website failure. Later workflow-cleanup changes passed a three-iteration/50-record test; the current release export is not separately documented as having completed another 582-record run after the final comment edits.

**v1.1.0 local regression (6 November 2026):** A clean full run generated 582 rows with one nine-column header, no empty or identical duplicate rows, matching the first two retained page references and seven final-page diagnostic rows. An earlier run produced only 532 rows after apparently starting at page 3; an old browser window is suspected but not proven. The sanitised distribution ZIP has only passed static checks, not an independent Control Room import. See [v1.1.0 validation notes](docs/v1.1.0-validation.md).

## Repository contents

- `release/NHL_Hockey_Data_Collector_v1.0.0_AA360.zip`: original importable AA360 export, preserved as the historical v1.0.0 baseline.
- `release/NHL_Hockey_Data_Collector_v1.1.0_sanitised_AA360.zip`: sanitised v1.1.0 export (to be added to the release branch before publication).
- [`config.example.json`](config.example.json) and [`docs/v1.1.0-import-setup.md`](docs/v1.1.0-import-setup.md): example settings and import instructions.
- `source/`: unpacked bot definition, manifest and six recorder metadata images for inspection.
- [CHANGELOG.md](CHANGELOG.md): verified milestones and changes.

**Use the native ZIP for import.** Unpacked sources are for inspection/version control and are not a replacement import format.

## How it works

1. Open the source URL in Chrome and read the HTML statistics table.
2. Build a page-content signature from all nine fields of each row.
3. Skip consolidation if the signature was already encountered. The initial website URL and page 1 can return the same data.
4. For each new page, export the captured table to a working CSV, re-import with **Contains header** enabled and merge the resulting named-column table into the accumulated table.
5. Record the signature and increment the unique-page counter only when the page is accepted.
6. Follow the website's **Next** link until the link is not detected, or until the attempt cap is reached.
7. Export the accumulated table to a CSV and display final attempt/unique-page counters.

### Why the CSV round trip?

The Recorder's extracted Data Table exposed empty internal column names in tests, despite exporting correctly to CSV. Direct table merging produced blank appended rows. Re-reading the CSV with headers enabled produced named columns, and the revised merge passed controlled and full-dataset tests. This is a validated workaround, not a proven isolation of every aspect of the original defect.

## Import and setup

**For v1.1.0**, use [the v1.1.0 setup guide](docs/v1.1.0-import-setup.md): create your own `config.json`, point the AA360 File variable `fConfigFile` to it, and configure a writable output folder. The JSON contains source URL, page limit, minimum pre-navigation delay and three CSV filenames. The example paths are illustrative; the bot does not automatically discover the configuration file.

**For the historical v1.0.0 release:**

1. Install/configure Automation Anywhere 360 Community Edition with a compatible local Bot Agent and browser automation support for Chrome.
2. Import the original AA360 ZIP through your Control Room's bot import facility.
3. **Before running, edit every `Data Table: Write to file` and `CSV/TXT: Open` file path** to point to a folder on your own machine. The supplied bot retains the original developer's `I:\My Drive\Automation Projects\NHL Hockey Data Collector\output\` path and will not be portable unchanged.
4. Check that the working CSV export and re-import refer to the same filename, that the output folder is writable, and that the temporary CSV session closes before the next iteration.
5. Start with a small attempt limit (for example 3) and verify the output before running the full 30-attempt configuration.
6. Use a clean Chrome session for predictable starting state, and avoid interacting with the browser during execution.

The bot depends on the current HTML/selector layout of a third-party demo website, which can change.


## Responsible scraping

[Scrape This Site](https://www.scrapethissite.com/) is designed for learning web scraping and explicitly permits educational extraction in its [FAQ](https://www.scrapethissite.com/pages/faq/). Its published guidance, as shown on the FAQ, also asks clients to:

- Make no more than **one request per second**.
- Identify themselves with a **User-Agent**.
- Respect the site's [robots.txt](https://www.scrapethissite.com/robots.txt).
- Avoid interfering with other visitors.

**Compliance status:** Neither v1.0.0 nor v1.1.0 has been independently measured for request frequency, User-Agent identification, or robots.txt compliance. Browser navigation may trigger additional requests beyond the 25 page attempts visible in the bot. Do not run it repeatedly or schedule unattended scraping without checking those requirements first. v1.1.0 adds a configurable minimum delay before clicking Next (tested at 2,000 and 3,000 ms), but that is **not** a measured HTTP rate limit or proof of identification/robots.txt compliance. A future version could introduce measured request controls and explicit safeguards.

## Known limitations (v1.0)

- Basic absence-of-Next detection does not distinguish a genuine final page from every possible browser/selector failure.
- A hard-coded 30-attempt cap prevents indefinite running but does not itself prove completeness.
- The final Message box is interactive; the bot is not fully unattended.
- Output paths are machine-specific and need manual configuration.
- No comprehensive Try/Catch handling, retry policy, structured log or notification system.
- Page signatures are concatenated plain text, not collision-resistant hashes.
- A Windows/browser popup can interrupt UI automation; one such interrupted run was observed and then successfully rerun.

## Planned operational improvements

A future version should check starting browser/page state and report suspected partial runs. This follows a v1.1.0 run that appeared to finish naturally but missed the first 50 rows. The browser-state explanation remains unconfirmed. More complete error handling, run-status reporting and logging are also planned, not implemented.

## Possible subsequent releases

Structured failure handling, non-interactive completion reports, logs, retry/timeout behaviour, configurable paths and improved source-data validation. These are plans, not implemented features.

## Source and attribution

The test dataset is provided by [Scrape This Site](https://www.scrapethissite.com/pages/forms/). This is an independent educational project, not affiliated with the site or Automation Anywhere. The repository includes bot code and capture metadata, not a claim of ownership over the source dataset.
