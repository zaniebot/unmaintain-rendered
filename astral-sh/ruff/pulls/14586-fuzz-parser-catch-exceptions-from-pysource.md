```yaml
number: 14586
title: "fuzz-parser: catch exceptions from `pysource-minimize`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - testing
assignees: []
merged: true
base: main
head: fuzzer-catch-exceptions
created_at: 2024-11-25T15:09:11Z
updated_at: 2024-11-28T13:28:47Z
url: https://github.com/astral-sh/ruff/pull/14586
synced_at: 2026-01-10T20:50:57Z
```

# fuzz-parser: catch exceptions from `pysource-minimize`

---

_Pull request opened by @AlexWaygood on 2024-11-25 15:09_

## Summary

#14566 added the new `--bin` flag to the `fuzz-parser` script. Running with `--bin red_knot` seems to be triggering quite a few crashes in the script locally due to `pysource-minimize` not being able to minimize the code that triggers a red-knot bug. I'm not sure if that's a bug in red-knot or `pysource-minimize` (or both), but this PR adds some exception handling in case `CouldNotMinimize` is raised by `pysource-minimize`.

## Test Plan

`python scripts/fuzz-parser/fuzz.py --bin red_knot 0-500`. The script aborts with a `CouldNotMinimize` exception on some seeds on `main`, but runs to completion on this branch.

---

_Label `testing` added by @AlexWaygood on 2024-11-25 15:09_

---

_Merged by @AlexWaygood on 2024-11-25 15:14_

---

_Closed by @AlexWaygood on 2024-11-25 15:14_

---

_Branch deleted on 2024-11-25 15:14_

---

_Label `internal` added by @dhruvmanila on 2024-11-28 13:28_

---
