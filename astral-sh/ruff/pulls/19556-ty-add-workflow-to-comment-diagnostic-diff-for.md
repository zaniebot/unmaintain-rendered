```yaml
number: 19556
title: "[ty] Add workflow to comment diagnostic diff for conformance tests"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ci
  - ty
  - no-test
assignees: []
merged: true
base: main
head: dhruv/conformance-test-comment
created_at: 2025-07-25T14:00:53Z
updated_at: 2025-07-25T15:24:30Z
url: https://github.com/astral-sh/ruff/pull/19556
synced_at: 2026-01-12T15:56:42Z
```

# [ty] Add workflow to comment diagnostic diff for conformance tests

---

_@dhruvmanila_

## Summary

This PR adds a workflow to comment the diff of diagnostics when running ty between `main` and a pull request on the [typing conformance test suite](https://github.com/python/typing/tree/main/conformance/tests).

The main workflow is introduced in https://github.com/astral-sh/ruff/pull/19555 which this workflow depends on. This workflow is similar to the [mypy primer comment](https://github.com/astral-sh/ruff/blob/d781a6ab3f44afe5dd1afe1d06fb58fd3739fab5/.github/workflows/mypy_primer_comment.yaml) workflow.

## Test Plan

I cannot test this workflow without merging it on `main` unless anyone knows a way to do this.


---

_Label `ci` added by @dhruvmanila on 2025-07-25 14:00_

---

_Label `ty` added by @dhruvmanila on 2025-07-25 14:00_

---

_Label `no-test` added by @dhruvmanila on 2025-07-25 14:01_

---

_Marked ready for review by @dhruvmanila on 2025-07-25 14:13_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-07-25 14:14_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-07-25 14:14_

---

_@sharkdp approved on 2025-07-25 14:27_

---

_Merged by @dhruvmanila on 2025-07-25 15:24_

---

_Closed by @dhruvmanila on 2025-07-25 15:24_

---

_Branch deleted on 2025-07-25 15:24_

---
