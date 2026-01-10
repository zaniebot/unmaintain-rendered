```yaml
number: 22320
title: "[`pyupgrade`] Apply `UP045` to string arguments of `typing.cast`"
type: pull_request
state: open
author: ntBre
labels:
  - bug
assignees: []
base: main
head: brent/up045
created_at: 2025-12-31T22:41:05Z
updated_at: 2026-01-05T10:28:32Z
url: https://github.com/astral-sh/ruff/pull/22320
synced_at: 2026-01-10T16:36:19Z
```

# [`pyupgrade`] Apply `UP045` to string arguments of `typing.cast`

---

_Pull request opened by @ntBre on 2025-12-31 22:41_

## Summary

Fixes #20096 by applying `UP045` to string type definitions as well as annotations.

This is kind of a niche issue, especially now that Python 3.9 has reached end-of-life, but UP045 was not applying to string `Optional` arguments in `typing.cast` calls because these aren't annotations. This PR just augments the `in_annotation` check to `in_annotation || in_string_type_definition`.

This means that `UP045` will also apply to other string type definitions like the rhs in:

```py
from __future__ import annotations

from typing_extensions import TypeAlias

x: TypeAlias = "Optional[str]"
```

which I think is also okay.

## Test Plan

New tests based on the issue


---

_Label `bug` added by @ntBre on 2025-12-31 22:41_

---

_Comment by @astral-sh-bot[bot] on 2025-12-31 22:49_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Marked ready for review by @ntBre on 2025-12-31 22:56_

---

_@MichaReiser reviewed on 2026-01-05 10:28_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP045_py39.py`:15 on 2026-01-05 10:28_

Can we add some tests for "Complex" string annotations (an annotation that uses implicit string concatenation). 

---
