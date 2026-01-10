```yaml
number: 9221
title: Set target versions in Black tests
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: set-target-version-in-black-tests
created_at: 2023-12-21T03:19:28Z
updated_at: 2023-12-21T04:25:11Z
url: https://github.com/astral-sh/ruff/pull/9221
synced_at: 2026-01-10T23:07:18Z
```

# Set target versions in Black tests

---

_Pull request opened by @MichaReiser on 2023-12-21 03:19_

## Summary

This PR updates our black test importer to parse the `--minimum-version` header and set the specified value as a Ruff compliant `target-version` option. 

This PR aso updates the most recent black test set

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2023-12-21 03:19_

---

_Label `formatter` added by @MichaReiser on 2023-12-21 03:19_

---

_@dhruvmanila approved on 2023-12-21 03:31_

---

_Comment by @github-actions[bot] on 2023-12-21 03:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Renamed from "Add target_version to formatter options" to "Set target versions in Black tests" by @MichaReiser on 2023-12-21 04:15_

---

_Merged by @MichaReiser on 2023-12-21 04:20_

---

_Closed by @MichaReiser on 2023-12-21 04:20_

---

_Branch deleted on 2023-12-21 04:20_

---
