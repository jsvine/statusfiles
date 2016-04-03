# Idea: Statusfiles

A __simple, standardized, technology-agnostic__ format for representing the __status of things__.

- [Motivation](#motivation)
- [Format](#format)
- [Infrastructure](#infrastructure)
- [Questions](#questions)

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
|`updated_at`|[ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) timestamp|The most recent time that the statusfile was updated (though not necessarily changed).|

### Optional Key/Value Pairs

Statusfiles __can__ also contain these key/value pairs:

|Key|Value|Description|
|---|---|---|
|`name`|string|A longer name for the thing.|
|`status_text`|string|A longer description of the status. Statusfile-readers should *not* use this value to determine whether the status has changed.|
|`description`|string|A description of what this status-tracker tracks, and how.|
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

At the heart of this ideas is that any technology should be able to *easily* read and write statusfiles.

Statusfiles could be stored locally, or hosted on the open web. They could be stored as flat files, or served dynamically.

Statusfile-readers — akin to RSS feed readers — could be built to check and track the files. Or you could write simple handlers for tools such as [Huginn](https://github.com/cantino/huginn/).

## Questions

- Does this idea even make sense?
- Are there better, pre-existing approaches?
- Could the format/spec be improved?
- Other ideas/improvements?

Have feedback? [File an issue](https://github.com/jsvine/statusfiles/issues/new) or email me at [jsvine@gmail.com](mailto:jsvine@gmail.com).
