```yaml
number: 9135
title: Clarify exclude-newer only allows full timestamps in settings docs
type: pull_request
state: merged
author: manzt
labels:
  - documentation
assignees: []
merged: true
base: main
head: manzt/docs
created_at: 2024-11-14T23:56:55Z
updated_at: 2025-01-06T15:07:25Z
url: https://github.com/astral-sh/uv/pull/9135
synced_at: 2026-01-10T11:44:29Z
```

# Clarify exclude-newer only allows full timestamps in settings docs

---

_Pull request opened by @manzt on 2024-11-14 23:56_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Follow up to #8553

Clarifies that the `exclude-newer` setting must be a full timestamp and not a date.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

N/A

<!-- How was it tested? -->


---

_Label `documentation` added by @samypr100 on 2024-11-21 00:49_

---

_@BurntSushi requested changes on 2024-12-21 13:34_

So I think this mostly looks good to me, but [uv does technically accept a superset of RFC 3339](https://docs.rs/jiff/latest/jiff/struct.Timestamp.html#parsing-and-printing). I think we should still _say_ RFC 3339 because that's what most users know, but the reality is actually an amalgamation of RFC 3339, RFC 9557 and ISO 8601. (Where the precise details are specified by [Temporal's ISO 8601 grammar](https://tc39.es/proposal-temporal/#sec-temporal-iso8601grammar).)

For example, `--exclude-newer '2024-06-19T15:22:45-04[America/New_York]'` is valid and perfectly cromulent despite it not being RFC 3339.

But saying "Temporal's ISO 8601 format" is probably not meaningful to most end users.

So for now, I think just saying "accepts a superset of RFC 3339" is fine. That's what most users will want. So just a small tweak in the wording. :)

---

_Assigned to @BurntSushi by @zanieb on 2024-12-21 17:07_

---

_Comment by @manzt on 2024-12-21 21:10_

@BurntSushi thanks for the feedback! I tweaked the wording. Would it be useful to link to the Temporal grammar in the superset? E.g.,

"Accepts a [superset](https://tc39.es/proposal-temporal/#sec-temporal-iso8601grammar) of [RFC 3339](https://www.rfc-editor.org/rfc/rfc3339.html)"

---

_@BurntSushi approved on 2025-01-06 15:03_

This LGTM. I might suggest waiting on linking to Temporal until we have a more stable link for it (which will probably happen once it's officially part of ECMAScript.)

---

_Merged by @BurntSushi on 2025-01-06 15:03_

---

_Closed by @BurntSushi on 2025-01-06 15:03_

---

_Branch deleted on 2025-01-06 15:07_

---
