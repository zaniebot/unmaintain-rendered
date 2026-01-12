```yaml
number: 9077
title: "[`refurb`] Implement `hashlib-digest-hex` (`FURB181`)"
type: pull_request
state: merged
author: sbrugman
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: refurb-hexdigest
created_at: 2023-12-09T20:59:07Z
updated_at: 2023-12-10T08:39:17Z
url: https://github.com/astral-sh/ruff/pull/9077
synced_at: 2026-01-12T15:55:27Z
```

# [`refurb`] Implement `hashlib-digest-hex` (`FURB181`)

---

_@sbrugman_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implementation of  Refurb FURB181
Part of https://github.com/astral-sh/ruff/issues/1348

## Test Plan

Test cases from Refurb

---

_Renamed from "Implement Refurb FURB181" to "[`refurb`] Implement `hashlib-digest-hex` (`FURB181`)" by @sbrugman on 2023-12-09 21:00_

---

_Comment by @github-actions[bot] on 2023-12-09 21:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh reviewed on 2023-12-09 22:28_

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/refurb/FURB181.py`:42 on 2023-12-09 22:28_

It's not-impossible for us to detect this, you could look at what we do in `TRIO115` where we map from name to value. There's also a general utility in draft here (https://github.com/astral-sh/ruff/pull/8583), but not merged. Either way, not a requirement for merging, just making a mental note for myself if anything.

---

_@sbrugman reviewed on 2023-12-09 23:15_

---

_Review comment by @sbrugman on `crates/ruff_linter/resources/test/fixtures/refurb/FURB181.py`:42 on 2023-12-09 23:15_

Interesting, nice utility. Would be good to revisit once that PR is merged!

For myself, I took this easy rule to get back at ruff developing after a while. I'm keen to go the more challenging implementation of using the fluid interface (https://github.com/dosisod/refurb/issues/286). This impacts a lot of data engineering (e.g. spark) and data science (pytorch etc.) code I see come by.

---

_@charliermarsh approved on 2023-12-10 01:53_

Thanks!

---

_Label `rule` added by @charliermarsh on 2023-12-10 01:54_

---

_Label `preview` added by @charliermarsh on 2023-12-10 01:54_

---

_Merged by @charliermarsh on 2023-12-10 02:00_

---

_Closed by @charliermarsh on 2023-12-10 02:00_

---

_Branch deleted on 2023-12-10 08:39_

---
