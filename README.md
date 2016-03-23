# IDEA: Statusfiles

A __simple, structured, standardized, technology-agnostic__ way to represent the __status of things__.

- [Motivation](#motivation)
- [Format](#format)
- [Infrastructure](#infrastructure)
- [Questions](#questions)

## Motivation

For the past few months, I've been using [BitBar](https://github.com/matryer/bitbar) to keep track data and documents that change on a semi-regular basis. For instance, I use it to automatically check for the FBI's National Instant Criminal Background Check System's [latest monthly reports](https://github.com/BuzzFeedNews/nics-firearm-background-checks).

BitBar is a great, quick solution for building these kinds of status-minders. But, as I script more of them, the mismatch between BitBar's intended usage and my goals — especially re. task-scheduling, output formatting, and message delivery — is becoming clearer. I've come to realize that what I really want is a __simple, structured, standardized, technology-agnostic__ way to represent the __status of things__.

## Format

Statusfiles can be written in JSON or YAML.

### Required Key/Value Pairs

Statusfiles __must__ contain at least these two key/value pairs:

|Key|Value|Description|
|---|---|---|
|`id`|string|A short, unique (to your universe) descriptor of the thing.|
|`status`|string|The current status of the thing. Programs will/should use this value to determine whether the status of a thing has been updated.

### Optional Key/Value Pairs

Statusfiles __can__ also contain these key/value pairs:

|Key|Value|Description|
|---|---|---|
|`name`|string|A longer name for the thing.|
|`status_long`|string|A longer description of the status. Statusfile-readers should *not* use this value to determine whether the status has changed.|
|`description`|string|A description of what this status-tracker tracks, and how.|
|`updated_at`|[ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) timestamp|The most recent time that `status` was updated (though not necessarily changed).|
|`data`|any JSON/YAML-friendly data structure|Additional data associated with the current status.|

### Example

As JSON: 

```json
{
    "id": "latest-nics-report",
    "name": "Latest NICS Monthly Report",
    "status": "2016-02",
    "updated_at": "2016-03-18T05:30:00+00:00",
    "description": "Extracted from https://www.fbi.gov/about-us/cjis/nics/reports/active_records_in_the_nics-index.pdf",
    "data": {
        "total_checks": 2613074,
        "year_over_year": "+40.5%"
    }
}
```

As YAML:

```yaml
id: latest-nics-report
name: Latest NICS Monthly Report
status: 2016-02
updated_at: 2016-03-18T05:30:00+00:00
description: >
    Extracted from https://www.fbi.gov/about-us/cjis/nics/reports/active_records_in_the_nics-index.pdf
data:
    total_checks: 2613074
    year_over_year: +40.5%
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
