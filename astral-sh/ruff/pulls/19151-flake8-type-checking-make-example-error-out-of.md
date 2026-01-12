```yaml
number: 19151
title: "[`flake8-type-checking`] Make example error out-of-the-box (`TC001`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-2
created_at: 2025-07-04T21:03:50Z
updated_at: 2025-07-07T16:37:56Z
url: https://github.com/astral-sh/ruff/pull/19151
synced_at: 2026-01-12T15:56:33Z
```

# [`flake8-type-checking`] Make example error out-of-the-box (`TC001`)

---

_@MeGaGiGaGon_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Part of #18972

This PR makes [typing-only-first-party-import (TC001)](https://docs.astral.sh/ruff/rules/typing-only-first-party-import/#typing-only-first-party-import-tc001)'s example error out-of-the-box. The old example raised `TC002` instead of `TC001`, so this makes it a `from .` import to fix that.

[Old example](https://play.ruff.rs/1fdbb293-86fc-4ed2-b2ff-b4836cea0c59)
```py
from __future__ import annotations

import local_module


def func(sized: local_module.Container) -> int:
    return len(sized)
```

[New example](https://play.ruff.rs/b886535c-9203-48bb-812b-1aa306f2c287)
```py
from __future__ import annotations

from . import local_module


def func(sized: local_module.Container) -> int:
    return len(sized)
```

The "Use instead" section was also modified similarly.

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-07-04 21:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @AlexWaygood on 2025-07-05 13:56_

---

_@dylwil3 approved on 2025-07-07 13:53_

Thank you!

---

_Merged by @dylwil3 on 2025-07-07 13:53_

---

_Closed by @dylwil3 on 2025-07-07 13:53_

---

_Branch deleted on 2025-07-07 16:37_

---
