```yaml
number: 17543
title: "Update Reqwest to `0.13.1`"
type: pull_request
state: open
author: salmonsd
labels: []
assignees: []
base: main
head: update-reqwest-tls
created_at: 2026-01-16T22:20:41Z
updated_at: 2026-01-16T22:48:04Z
url: https://github.com/astral-sh/uv/pull/17543
synced_at: 2026-01-16T23:06:08Z
```

# Update Reqwest to `0.13.1`

---

_@salmonsd_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR fixes or improves the TLS experience by upgrading reqwest to `0.13.1` via https://github.com/astral-sh/uv/issues/17427

## Test Plan

This was tested locally behind a Corporate CA running `cargo nextest run`.

Dependent PRs (and publishing) required before merging:
1. https://github.com/astral-sh/reqwest-middleware/pull/11
2. https://github.com/astral-sh/ambient-id/pull/21
3. https://github.com/astral-sh/async_http_range_reader/pull/3


---

_Assigned to @zanieb by @zanieb on 2026-01-16 22:21_

---

_Comment by @salmonsd on 2026-01-16 22:34_

Will work on failing tests (apologies)

---
