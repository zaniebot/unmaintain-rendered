```yaml
number: 13109
title: Add time, tzinfo, and timezone as immutable function calls
type: pull_request
state: merged
author: N-Wouda
labels:
  - bug
  - linter
assignees: []
merged: true
base: main
head: add-missing-immutable-datetime-types
created_at: 2024-08-26T13:10:11Z
updated_at: 2024-08-27T14:53:29Z
url: https://github.com/astral-sh/ruff/pull/13109
synced_at: 2026-01-12T15:55:43Z
```

# Add time, tzinfo, and timezone as immutable function calls

---

_@N-Wouda_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Currently, `datetime.datetime`, `datetime.date`, and `datetime.timedelta` are correctly flagged as immutable calls, and RUF009 does not complain about their use in dataclasses. But the `datetime` module features three more classes that are also immutable: `time`, `tzinfo`, and `timezone`. See [here](https://docs.python.org/3/library/datetime.html#available-types) for details. These are missing, and that causes a false positive for e.g. the following example:

```python
from dataclasses import dataclass
from datetime import time


@dataclass
class Test:
    test: time = time()
```

## Test Plan

<!-- How was it tested? -->

Not sure, I'm not all that familiar with Rust or the Ruff internals. How do I add tests for this? 😄 

---

_Comment by @github-actions[bot] on 2024-08-26 13:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-08-27 14:50_

Thanks!

---

_Label `bug` added by @AlexWaygood on 2024-08-27 14:51_

---

_Label `linter` added by @AlexWaygood on 2024-08-27 14:51_

---

_Merged by @AlexWaygood on 2024-08-27 14:51_

---

_Closed by @AlexWaygood on 2024-08-27 14:51_

---

_Branch deleted on 2024-08-27 14:53_

---
