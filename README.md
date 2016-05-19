# Idea: Statusfiles

A __simple, standardized, technology-agnostic__ format for representing the __status of things__.

- [Motivation](#motivation)
- [Format](#format)
- [Infrastructure](#infrastructure)
- [Use-Case Ideas](#use-case-ideas)
- [Feedback](#feedback)

## Motivation

News, you might say, is when something changes. That's a horrendously oversimplified definition, but contains a kernel of practical truth: People want to know when something important changes. That simple desire involves three basic steps:

1. Knowing the status of the thing
2. Figuring out when that status changes
3. Getting notified about those changes

The idea of tracking changes isn't new; many websites, software, and companies provide the service. But each solution takes its own approach. And each solution typically bundles all of the three steps above into a single, non-transferable toolset. Statusfiles aim to isolate, decentralize, and liberate that first step from the rest of the process. It could be the first step, I hope, of a modular ecosystem of change-tracking.

## Format

Statusfiles are deliriously simple. They're plain-old JSON files with just three required fields.

### Required Key/Value Pairs

Statusfiles __must__ contain at least these two key/value pairs:

|Key|Value|Description|
|---|---|---|
|`id`|string|A short, unique (to your universe) descriptor of the thing.|
|`status`|string|The current status of the thing. Statusfile-readers will/should use this value to determine whether the status of a thing has been updated.|
|`checked_at`|[ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) timestamp|The most recent time that the statusfile-writer checked the source.|

### Optional Key/Value Pairs

Statusfiles __can__ also contain these key/value pairs:

|Key|Value|Description|
|---|---|---|
|`name`|string|A longer name for the thing.|
|`status_text`|string|A longer description of the status. Statusfile-readers should *not* use this value to determine whether the status has changed.|
|`description`|string|A description of what this status-tracker tracks, and how.|
|`link`|string|A URL pointing to the relevant source.|
|`updated_at`|[ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) timestamp|The most recent time that the status changed.|
|`data`|any JSON-friendly data structure|Additional data associated with the current status.|


### Example

Here's what a statusfile tracking the most recent monthly report available from the FBI's National Instant Criminal Background Check System:

```json
{
    "id": "latest-nics-report",
    "name": "Latest NICS Monthly Report",
    "status": "2016-02",
    "status_text": "February 2016",
    "updated_at": "2016-03-18T05:30:00+00:00",
    "description": "Extracted from https://www.fbi.gov/about-us/cjis/nics/reports/active_records_in_the_nics-index.pdf",
    "data": {
        "total_checks": 2613074,
        "year_over_year": "+40.5%"
    }
}
```

## Infrastructure

Statusfiles, in practice, would occupy an intermediary layer between __statusfile-writers__ and __statusfile-readers__:

- Statusfile-writers identify the status of the thing — in most cases, likely via web-scraping or API calls — and write a statusfile to storage somewhere. That *somewhere* could be a local machine, the open web, or places in between. And statusfile-writers could be triggered on schedule (e.g., via `cron`), in response to an HTTP request, or via some other mechanism.

- Statusfile-readers — akin to RSS feed readers — check and track statusfiles. When changes *are* detected, statusfile-readers could notify the user via email, SMS, desktop popup, or other means.

Because statusfiles employ a simple, standardized format, statusfile-readers should be able to consume statusfiles written by any program.


## Use-Case Ideas

Statusfiles can represent the status of *anything*, but here are a few ideas:

- __Website changes__. The motivating example for me. A simple statusfile-writer could help identify when an important part of a webpage changes.
- __Service outages__. A statusfile could represent the availability of a popular service (e.g., Twitter) or crucial API.
- __Sports team records__. A statusfile could track the win/loss record of a team; ideal for casual fans. Or a higher-activity statusfile could track the score of that team's current/most-recent game.
- __Flight statuses__. Could help you know when/if your flight is delayed.

## Feedback

Does this idea make sense? Are there better, pre-existing approaches? Could the format/spec be improved? If you have feedback, please [file an issue](https://github.com/jsvine/statusfiles/issues/new) or email me at [jsvine@gmail.com](mailto:jsvine@gmail.com).
