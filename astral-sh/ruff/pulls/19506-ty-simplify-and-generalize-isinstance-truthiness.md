```yaml
number: 19506
title: "[ty] Simplify and generalize `isinstance()` truthiness inference"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - ty
assignees: []
draft: true
base: main
head: alex/isinstance-truthiness
created_at: 2025-07-23T11:10:28Z
updated_at: 2025-07-23T12:02:16Z
url: https://github.com/astral-sh/ruff/pull/19506
synced_at: 2026-01-12T15:56:41Z
```

# [ty] Simplify and generalize `isinstance()` truthiness inference

---

_@AlexWaygood_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @AlexWaygood on 2025-07-23 11:13_

---

_Comment by @github-actions[bot] on 2025-07-23 11:13_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
apprise (https://github.com/caronc/apprise)
- error[non-subscriptable] test/test_api.py:1897:13: Cannot subscript object of type `None` with no `__getitem__` method
- error[non-subscriptable] test/test_api.py:1913:25: Cannot subscript object of type `None` with no `__getitem__` method
- error[non-subscriptable] test/test_api.py:1924:29: Cannot subscript object of type `None` with no `__getitem__` method
- Found 4312 diagnostics
+ Found 4309 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Closed by @AlexWaygood on 2025-07-23 12:02_

---

_Branch deleted on 2025-07-23 12:02_

---
