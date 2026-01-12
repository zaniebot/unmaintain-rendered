```yaml
number: 16006
title: "Switch default indentation to 2 spaces in `pyproject.toml`"
type: pull_request
state: open
author: browniebroke
labels: []
assignees: []
base: main
head: feat-4-to-2-spaces-indents
created_at: 2025-09-23T19:04:29Z
updated_at: 2025-09-24T08:31:53Z
url: https://github.com/astral-sh/uv/pull/16006
synced_at: 2026-01-12T16:12:04Z
```

# Switch default indentation to 2 spaces in `pyproject.toml`

---

_@browniebroke_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fix #5852 

Switch default `pyproject.toml` indendation from 4 spaces to 2. I've hit this problem because [pyproject-fmt](https://pyproject-fmt.readthedocs.io/en/latest/index.html) uses 2 spaces. As per the linked issue (#5009), the example in [the PyPA guide](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#dependencies-and-requirements) also implies that. 

As far as I can tell, it would stick to 4 spaces if that's the existing indentation (as implemented in https://github.com/astral-sh/uv/issues/5009). I've change the associated test case to use a non-default indentation of 4.

I tried to go with the simplest solution, but I appreciate that it might be considered too disruptive by the maintainers. If the project prefers to go with a more configurable solution, then it's probably best to close this and start over.

## Test Plan

Update existing tests


---

_@browniebroke reviewed on 2025-09-23 19:07_

---

_Review comment by @browniebroke on `crates/uv/tests/it/edit.rs`:3960 on 2025-09-23 19:07_

This is the test checking that non-default indentation is preserved.

---

_Renamed from "Switch default indentation to 2" to "Switch default indentation to 2 spaces in `pyproject.toml`" by @browniebroke on 2025-09-23 19:09_

---

_Comment by @notatallshaw on 2025-09-23 19:46_

> indendation from 4 spaces to 2 to follow [the PyPA guide](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#dependencies-and-requirements).

To be clear, while examples use 2 spaces, there is no guidance or rule from PyPA.

For example, pip's pyproject.toml is completely inconsistent in the number of spaces it uses for indentation. 

---

_Marked ready for review by @browniebroke on 2025-09-23 21:10_

---

_Comment by @browniebroke on 2025-09-24 08:31_

> To be clear, while examples use 2 spaces, there is no guidance or rule from PyPA.
> 
> For example, pip's pyproject.toml is completely inconsistent in the number of spaces it uses for indentation.

Thanks for pointing this out, I've rephrased to make it clearer that it's not a rule from PyPA, but simply implied from the example. I landed on the issue after seeing a discrepancy between uv and pyproject-fmt.

---
