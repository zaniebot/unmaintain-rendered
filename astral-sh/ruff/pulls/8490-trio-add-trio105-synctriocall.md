```yaml
number: 8490
title: "[`TRIO`] Add `TRIO105`: `SyncTrioCall`"
type: pull_request
state: merged
author: qdegraaf
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: feat/TRIO105
created_at: 2023-11-04T16:33:02Z
updated_at: 2023-11-06T15:53:39Z
url: https://github.com/astral-sh/ruff/pull/8490
synced_at: 2026-01-12T15:55:26Z
```

# [`TRIO`] Add `TRIO105`: `SyncTrioCall`

---

_@qdegraaf_

## Summary

Adds `TRIO105` from the [flake8-trio plugin](https://github.com/Zac-HD/flake8-trio). The `MethodName` logic mirrors that of `TRIO100` to stay consistent within the plugin.

It is at 95% parity with the exception of upstream also checking for a slightly more complex scenario where a call to `start()` on a `trio.Nursery` context should also be immediately awaited. Upstream plugin appears to just check for anything named `nursery` judging from [the relevant issue](https://github.com/Zac-HD/flake8-trio/issues/56). 

Unsure if we want to do so something similar or, alternatively, if there is some capability in ruff to check for calls made on this context some other way

## Test Plan

Added a new fixture, based on [the one from upstream plugin](https://github.com/Zac-HD/flake8-trio/blob/main/tests/eval_files/trio105.py)

## Issue link

Refers: https://github.com/astral-sh/ruff/issues/8451


---

_Comment by @github-actions[bot] on 2023-11-04 16:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `rule` added by @charliermarsh on 2023-11-05 19:47_

---

_@charliermarsh approved on 2023-11-05 19:48_

LGTM! Pulled `MethodName` into a shared file and implementation. Thanks!

---

_Label `preview` added by @charliermarsh on 2023-11-05 19:55_

---

_Merged by @charliermarsh on 2023-11-05 19:56_

---

_Closed by @charliermarsh on 2023-11-05 19:56_

---

_Branch deleted on 2023-11-06 15:53_

---
