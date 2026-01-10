```yaml
number: 13958
title: Remove unreferenced snapshots
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/remove-unreferenced-snapshots
created_at: 2024-10-28T05:50:46Z
updated_at: 2024-10-28T06:16:08Z
url: https://github.com/astral-sh/ruff/pull/13958
synced_at: 2026-01-10T20:59:37Z
```

# Remove unreferenced snapshots

---

_Pull request opened by @dhruvmanila on 2024-10-28 05:50_

## Summary

This PR removes unreferenced snapshots that exists on `main`.

I'm not exactly sure how did they get into without the test failing but here are the details for each of them:

* The snapshot that references `quote_annotations.py` was likely due to my mistake when I renamed that file in https://github.com/astral-sh/ruff/commit/4d109514d608b45bc11a5ecf3e022112461cb571 but did not remove this snapshot.
* `...r/src/rules/pydocstyle/snapshots/ruff_linter__rules__pydocstyle__tests__D415_D415.py.snap`: This was added in https://github.com/astral-sh/ruff/pull/13399/files#diff-3195a1dd9e1c4331c95958a63f47cfb4496a370837ca02a68d06322e197cd249 although there is not `D415.py` file corresponding to the snapshot nor a reference entry in the `mod.rs` file
* `crates/ruff_python_formatter/tests/snapshots/format.snap`: This references `join_implicit_concatenated_string_preserve.py` for which the snapshot already exists: https://github.com/astral-sh/ruff/blob/ff472412b705faf5e7be42b68d766b0c09cc2037/crates/ruff_python_formatter/tests/snapshots/format@expression__join_implicit_concatenated_string_preserve.py.snap

## Test Plan

`cargo insta test --workspace --unreferenced delete --test-runner nextest`


---

_Label `internal` added by @dhruvmanila on 2024-10-28 05:50_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-10-28 05:50_

---

_@MichaReiser approved on 2024-10-28 06:15_

---

_Merged by @MichaReiser on 2024-10-28 06:16_

---

_Closed by @MichaReiser on 2024-10-28 06:16_

---

_Branch deleted on 2024-10-28 06:16_

---
