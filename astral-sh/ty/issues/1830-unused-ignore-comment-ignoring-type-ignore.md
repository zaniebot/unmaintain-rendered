```yaml
number: 1830
title: "`unused-ignore-comment`:  ignoring `# type:ignore` comments that are intended for mypy"
type: issue
state: closed
author: jorenham
labels: []
assignees: []
created_at: 2025-12-09T23:37:33Z
updated_at: 2025-12-09T23:39:49Z
url: https://github.com/astral-sh/ty/issues/1830
synced_at: 2026-01-12T15:54:25Z
```

# `unused-ignore-comment`:  ignoring `# type:ignore` comments that are intended for mypy

---

_@jorenham_

scipy-stubs supports both mypy and ty. It uses `# type: ignore` comments for mypy, and `# ty:ignore` for ty. When I enable `unused-ignore-comment`, ty reports many unused `Unused blanket `type:ignore` directive`, even though those are specifically meant for mypy, and only for mypy.
If I now turn on `unused-ignore-comment`, ty reports 95 "Unused blanket `type: ignore` directive", and all of them are needed for mypy.

With Pyright I can avoid this problem by setting `enableTypeIgnoreComments=false`, which causes it to ignore all `# type:ignore` comments in favor of `# pyright:ignore`. Maybe something like that also be added to ty? 

Alternatively, I suppose that you could whitelist all of mypy's error codes, but that wouldn't be ideal in case of non-unique ones (like `deprecated` if I remember correctly).

---

_Renamed from "`unused-ignore-comment`:  ignoring `# type:ignore` comments for mypy compatibility" to "`unused-ignore-comment`:  ignoring `# type:ignore` comments that are intended for mypy" by @jorenham on 2025-12-09 23:37_

---

_Comment by @carljm on 2025-12-09 23:39_

Yes, we intend to add this setting; see https://github.com/astral-sh/ty/issues/1422

---

_Closed by @carljm on 2025-12-09 23:39_

---
