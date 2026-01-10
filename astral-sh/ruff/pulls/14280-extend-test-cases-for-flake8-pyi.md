```yaml
number: 14280
title: "Extend test cases for `flake8-pyi`"
type: pull_request
state: merged
author: sbrugman
labels:
  - testing
assignees: []
merged: true
base: main
head: pyi-test-cases
created_at: 2024-11-11T13:57:02Z
updated_at: 2024-11-26T08:58:58Z
url: https://github.com/astral-sh/ruff/pull/14280
synced_at: 2026-01-10T20:50:57Z
```

# Extend test cases for `flake8-pyi`

---

_Pull request opened by @sbrugman on 2024-11-11 13:57_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR extends the test cases for a couple of flake8-pyi rules with nested and mixed union annotations.
Although these are rare, they are valid:

```python-console
>>> from typing import Union
>>> Union[int | int | float]
typing.Union[int, float]

>>> Union[Union[float, complex]]
typing.Union[float, complex]
```

We should be able to handle them anyhow, as users or fixes might write these accidentally.

## Test Plan

<!-- How was it tested? -->

The snapshots are updated with the as-is behaviour. 
I'll follow up with a change to the nested union logic. Having these tests in place before makes it easier to review the diff.

---

_Review requested from @AlexWaygood by @sbrugman on 2024-11-11 13:57_

---

_@AlexWaygood reviewed on 2024-11-11 14:06_

This has merge conflicts; could you take a look?

---

_Comment by @sbrugman on 2024-11-11 14:08_

I'll do it in one go after #14270, #14272 and #14273 are merged to avoid duplicate work.

---

_Label `internal` added by @MichaReiser on 2024-11-11 16:43_

---

_Label `testing` added by @MichaReiser on 2024-11-11 16:43_

---

_Label `internal` removed by @MichaReiser on 2024-11-11 16:43_

---

_Converted to draft by @sbrugman on 2024-11-12 16:08_

---

_Marked ready for review by @sbrugman on 2024-11-25 22:51_

---

_Comment by @github-actions[bot] on 2024-11-25 22:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @MichaReiser on 2024-11-26 08:10_

---

_Closed by @MichaReiser on 2024-11-26 08:10_

---

_Branch deleted on 2024-11-26 08:58_

---
