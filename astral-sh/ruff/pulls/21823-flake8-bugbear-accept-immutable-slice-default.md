```yaml
number: 21823
title: "[`flake8-bugbear`] Accept immutable slice default arguments (`B008`)"
type: pull_request
state: merged
author: LoicRiegel
labels:
  - bug
assignees: []
merged: true
base: main
head: builtin-slice-immutable
created_at: 2025-12-06T13:06:34Z
updated_at: 2025-12-08T22:04:16Z
url: https://github.com/astral-sh/ruff/pull/21823
synced_at: 2026-01-12T15:57:34Z
```

# [`flake8-bugbear`] Accept immutable slice default arguments (`B008`)

---

_@LoicRiegel_

Closes issue #21565

## Summary

As pointed out in the issue, slices are currently flagged by B008 but this behavior is incorrect because slices are immutable.

## Test Plan

Added a test case in the "B006_B008.py" fixture. Sorry for the diff in the snapshots, the only thing that changes in those flies is the line numbers, though.

You can also test this manually with this file:
```py
# test_slice.py
def c(d=slice(0, 3)): ...
```

```sh
> target/debug/ruff check tmp/test_slice.py --no-cache --select B008
All checks passed!
```


---

_Comment by @astral-sh-bot[bot] on 2025-12-06 13:16_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Renamed from "flake8_bugbear (BOO8): accept immutable slice" to "[`flake8-bugbear`] Accept immutable slice default arguments (`B008`)" by @ntBre on 2025-12-08 14:57_

---

_Label `bug` added by @ntBre on 2025-12-08 14:58_

---

_@ntBre approved on 2025-12-08 19:00_

Thank you!

Just a note in case of future reference, I checked the other callers of `is_immutable_return_type` (via `is_immutable_func`), and this change will also affect [function-call-in-dataclass-default-argument (RUF009)](https://docs.astral.sh/ruff/rules/function-call-in-dataclass-default-argument/#function-call-in-dataclass-default-argument-ruf009) and [mutable-contextvar-default (B039)](https://docs.astral.sh/ruff/rules/mutable-contextvar-default/#mutable-contextvar-default-b039).

But I think it's straightforward enough that we don't need to test every rule.

---

_Merged by @ntBre on 2025-12-08 19:00_

---

_Closed by @ntBre on 2025-12-08 19:00_

---

_Branch deleted on 2025-12-08 22:04_

---
